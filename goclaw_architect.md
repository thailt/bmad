# GoClaw Architecture

```text
CLI / Bootstrap
    ↓
Gateway / API Surface
    ↓
Agent Runtime / Orchestration Core
    ↓
Providers + Tools + Skills + MCP
    ↓
Persistence / Store

Channels / Bus / Cron / Heartbeat / Tasks / Tracing
        ↖ chạy bất đồng bộ, bơm sự kiện và phân phối kết quả ↗
```

## Tóm tắt nhanh

GoClaw có thể được nhìn như một **modular monolith AI agent platform runtime**: một codebase thống nhất nhưng tách lớp rõ giữa bootstrap, gateway, vòng lặp agent, tích hợp provider/tool/skill, persistence, và runtime bất đồng bộ.

## 1) Entry & Runtime Bootstrap

**Package/path chính**
- `main.go`
- `cmd/root.go`
- `cmd/gateway.go`
- `cmd/gateway_setup.go`

**Trách nhiệm**
- Điểm vào tiến trình.
- Parse CLI, nạp config, dựng dependency graph ban đầu.
- Chọn mode chạy: gateway runtime, setup flow, hoặc lệnh điều khiển liên quan.

**Luồng chính**
- Người dùng/chạy service gọi binary.
- `main.go` chuyển vào cây lệnh trong `cmd/*`.
- Lệnh gateway khởi tạo config/runtime rồi hand-off sang lớp server/gateway.

**Điểm mạnh**
- Entry rõ ràng, dễ đóng gói thành CLI/service.
- Tách bootstrap khỏi business/runtime logic.

**Rủi ro**
- Nếu config bootstrap phình to, `cmd/*` dễ trở thành nơi trộn quá nhiều wiring logic.
- Startup path là choke point: lỗi config/dependency làm toàn bộ runtime không lên được.

## 2) Gateway & API Surface

**Package/path chính**
- `internal/gateway/server.go`
- `internal/gateway/router.go`
- `internal/gateway/methods/*`
- `internal/http/*`

**Trách nhiệm**
- Expose bề mặt giao tiếp chính qua HTTP/gateway.
- Định tuyến request vào đúng method/handler.
- Chuẩn hóa request/response, middleware, transport-level concern.

**Luồng chính**
- Client/plugin/UI gửi request vào gateway.
- `router.go` phân tuyến sang `methods/*`.
- Handler gọi agent runtime, store hoặc integration layer.
- Kết quả trả về đồng bộ hoặc được gắn với luồng async phía dưới.

**Điểm mạnh**
- Bề mặt API tập trung, dễ kiểm soát auth, routing, observability.
- `methods/*` giúp chia nhỏ theo use case thay vì nhồi vào một server file.

**Rủi ro**
- Gateway dễ trở thành “God layer” nếu business logic bị kéo ngược lên handler.
- Coupling mạnh giữa transport shape và agent internals sẽ làm khó mở rộng API sau này.

## 3) Agent Runtime / Orchestration Core

**Package/path chính**
- `internal/agent/router.go`
- `internal/agent/loop*.go`
- `internal/agent/systemprompt*.go`
- `internal/agent/inject.go`
- `internal/agent/toolloop.go`

**Trách nhiệm**
- Đây là lõi điều phối agent.
- Quyết định route tác vụ, dựng system prompt/runtime context, inject dependency/context, chạy loop suy luận và tool loop.
- Điều phối qua lại giữa model output, tool calls và kết quả cuối.

**Luồng chính**
- Gateway chuyển request vào agent router.
- Agent runtime dựng context + prompt.
- `loop*.go` điều khiển vòng đời phiên xử lý.
- `toolloop.go` gọi tools khi model yêu cầu.
- Kết quả được tổng hợp rồi trả về gateway/channels/store.

**Điểm mạnh**
- Tách biệt orchestration với transport.
- Có không gian riêng cho prompt construction, injection và tool execution.
- Thuận lợi để thêm mode xử lý mới mà không đụng trực tiếp gateway.

**Rủi ro**
- Agent loop là vùng phức tạp nhất: dễ phát sinh state khó debug.
- Nếu injection/prompt/tool loop phụ thuộc chéo quá nhiều, việc thay đổi model behavior sẽ khó đoán.

## 4) Integrations Layer: Providers + Tools + Skills + MCP

**Package/path chính**
- `internal/providers/*`
- `internal/providerresolve/*`
- `internal/oauth/*`
- `internal/tools/*`
- `internal/mcp/*`
- `internal/skills/*`
- `skills/*`

**Trách nhiệm**
- Kết nối model providers, resolve provider phù hợp theo runtime/config.
- Quản lý OAuth/integration auth.
- Cấp công cụ cho agent (`tools`), kỹ năng tái sử dụng (`skills`), và kết nối hệ sinh thái MCP.

**Luồng chính**
- Agent runtime cần model/tool/skill.
- `providerresolve/*` chọn provider; `providers/*` thực thi gọi model.
- `tools/*`, `skills/*`, `mcp/*` mở rộng khả năng hành động và truy cập ngữ cảnh ngoài core.
- OAuth hỗ trợ các integration cần ủy quyền.

**Điểm mạnh**
- Khả năng mở rộng cao: thêm provider/tool/skill mà không phá lõi.
- Phân tách tốt giữa orchestration core và capability adapters.

**Rủi ro**
- Bề mặt tích hợp rộng làm tăng chi phí bảo trì, auth, rate limit và compatibility.
- Nếu hợp đồng giữa agent core và tools/providers không đủ chặt, lỗi runtime sẽ xuất hiện ở biên tích hợp.

## 5) Persistence & Data Domain

**Package/path chính**
- `internal/store/*.go`
- `internal/store/stores.go`
- `migrations/*`

**Trách nhiệm**
- Định nghĩa lớp truy cập dữ liệu và gom các store implementation.
- Lưu session, task, event, state hoặc metadata runtime.
- Quản lý schema evolution qua migrations.

**Luồng chính**
- Gateway/agent/channels/tasks cần đọc-ghi trạng thái.
- Gọi qua `internal/store/*` thay vì truy cập DB trực tiếp.
- `migrations/*` đảm bảo schema khớp với version runtime.

**Điểm mạnh**
- Có lớp persistence riêng giúp runtime không dính trực tiếp vào DB driver/sql cụ thể.
- Migration-first giúp triển khai và nâng cấp ổn định hơn.

**Rủi ro**
- Nếu domain model trong store chỉ phản chiếu bảng DB mà không phản ánh nghiệp vụ, logic dễ rò rỉ lên layer trên.
- Migration/backward compatibility là điểm rủi ro lớn khi runtime chạy lâu dài.

## 6) Async Runtime & Channel/Event Delivery

**Package/path chính**
- `internal/channels/*`
- `internal/bus/*`
- `internal/cron/*`
- `internal/heartbeat/*`
- `internal/tasks/*`
- `internal/tracing/*`

**Trách nhiệm**
- Chạy các luồng bất đồng bộ và phân phối sự kiện/kết quả.
- Kết nối channel giao tiếp, event bus, scheduler/cron, heartbeat, task execution và tracing.
- Hỗ trợ runtime vận hành liên tục thay vì chỉ request-response thuần túy.

**Luồng chính**
- Request có thể sinh task/sự kiện nền.
- `tasks/*` xử lý công việc async; `bus/*` lan truyền event.
- `channels/*` gửi/nhận với các bề mặt giao tiếp.
- `cron/*` và `heartbeat/*` kích hoạt tác vụ định kỳ.
- `tracing/*` bọc observability xuyên suốt các flow này.

**Điểm mạnh**
- Giúp nền tảng vượt khỏi mô hình synchronous API đơn giản.
- Tạo nền cho scheduling, delivery đa kênh, telemetry và background processing.

**Rủi ro**
- Async flow làm tăng độ khó về ordering, retry, idempotency và debug liên tiến trình.
- Nếu tracing không đủ sâu, rất khó lần theo request gốc đến event/task hậu kỳ.

## Luồng tổng thể end-to-end

1. Binary khởi chạy qua `main.go` và `cmd/*`.
2. Gateway nhận request qua `internal/gateway/*` và `internal/http/*`.
3. Request được chuyển vào lõi điều phối `internal/agent/*`.
4. Agent gọi model/tool/skill/MCP qua integration layer.
5. State và metadata được lưu qua `internal/store/*`.
6. Nếu cần xử lý nền hoặc phân phối ra channel, async runtime tiếp nhận qua `internal/tasks/*`, `internal/bus/*`, `internal/channels/*`, `internal/cron/*`, `internal/heartbeat/*`.

## Kết luận ngắn

GoClaw là một **modular monolith AI agent platform runtime**: lõi điều phối tập trung trong một codebase, nhưng được chia thành các khối tương đối rõ ràng để mở rộng theo chiều ngang về provider, tools, skills, channel và background runtime mà chưa cần tách thành nhiều service độc lập.
# BMAD Agile Mapping

## Mục tiêu

Tài liệu này map ngắn gọn giữa **Agile process** và **BMAD method** để dễ chọn đúng command khi làm việc.

## Bảng mapping

| Agile process | BMAD method | BMAD command gợi ý | Ý nghĩa ngắn |
|---|---|---|---|
| Product vision / Product goal | Business framing | `bmad-product-brief` | Xác định vấn đề, mục tiêu, scope |
| Epic / User story definition | Requirement / domain modeling | `bmad-create-prd`, `bmad-create-epics-and-stories` | Chuyển nhu cầu thành capability, story, rule, concept |
| Backlog refinement | Business + model refinement | `bmad-product-brief`, `bmad-create-prd` | Làm rõ rule, cắt nhỏ, ưu tiên |
| Sprint planning | Delivery planning | `bmad-sprint-planning` | Chốt scope iteration, dependency, hướng triển khai |
| Technical design / spike | Architecture design | `bmad-create-architecture`, `bmad-quick-dev` | Làm rõ solution, boundary, interface, risk |
| Development | Build / delivery | `bmad-dev-story`, `bmad-create-story` | Implement feature và artifact liên quan |
| QA / testing | Validation | `bmad-check-implementation-readiness`, `bmad-code-review` | Kiểm tra correctness, readiness, quality |
| Sprint review / demo | Delivery review | `bmad-code-review` | Review increment, tìm gap, nhận feedback |
| Retrospective | Method refinement | `bmad-retrospective` | Cải tiến workflow, assumption, quality loop |
| Release / rollout | Delivery / adoption | `bmad-check-implementation-readiness` | Kiểm tra mức sẵn sàng trước khi đưa value ra production |

## Rule of thumb

- **Agile** = nhịp làm việc, feedback loop, cách team vận hành.
- **BMAD** = khung tư duy và delivery discipline để làm rõ context, planning, design, verification.
- Dùng tốt nhất khi **chạy BMAD bên trong Agile**, không thay Agile bằng BMAD.

## Flow ngắn gọn dễ nhớ

```text
Vision / Goal
  -> bmad-product-brief

Story / Requirement clarification
  -> bmad-create-prd
  -> bmad-create-epics-and-stories

Sprint planning
  -> bmad-sprint-planning

Design / Architecture
  -> bmad-create-architecture
  -> bmad-quick-dev

Development
  -> bmad-create-story
  -> bmad-dev-story

Validation / Review
  -> bmad-check-implementation-readiness
  -> bmad-code-review

Retrospective
  -> bmad-retrospective
```

## Ghi chú

- `bmad-quick-dev` hợp với discovery / spike / cheap scan trước khi đọc sâu.
- `bmad-create-prd` hợp khi cần chưng cất requirement rõ hơn trước khi plan/build.
- `bmad-create-architecture` hợp khi cần làm rõ technical shape hoặc design decision.
- `bmad-dev-story` hợp để đi vào execution cho một story cụ thể.
- `bmad-code-review` và `bmad-check-implementation-readiness` hợp để siết quality gate.

# BMAD

## A. Core idea

- BMAD không chỉ là prompting.
- Nó là một operating model để agent/con người làm việc với project thật theo cách có cấu trúc.
- Trọng tâm: context, workflow, role, planning, verification, quality, reusable artifacts.

## B. Mindset shift

### 1. Từ chat với AI sang vận hành AI

- Không chỉ hỏi–đáp ad-hoc.
- Phải nghĩ theo workflow, role, context, quality bar.

### 2. Từ prompt-centric sang context-centric

- Prompt hay chưa đủ.
- Context đúng mới quyết định chất lượng đầu ra.

### 3. Từ execution-first sang planning-first

- Đừng nhảy vào làm quá sớm.
- Hiểu đúng trước, rồi mới plan và execute.

### 4. Từ knowledge trong đầu sang knowledge trong artifact

- Offload know-how vào tài liệu.
- Biến kinh nghiệm thành thứ search được, maintain được, reuse được.

## C. Context engineering

### 1. Project context là first-class

- Agent phải biết đang làm trên cái gì.
- System prompt và project context là hai thứ khác nhau:
  - system prompt = phải làm như thế nào
  - project context = đang làm trên cái gì

### 2. Token là budget

- Không đọc càng nhiều càng tốt.
- Phải đọc đúng phần quan trọng nhất.

### 3. Read cheap before read deep

- Scan cấu trúc, README, config, entrypoint trước.
- Đào sâu sau.

### 4. Context lớn thì phải shard

- Chia theo module, domain, workflow, bounded context.
- Có overview, shard summary, deep evidence.

### 5. Retrieval-driven thay vì preload-driven

- Không nhét tất cả vào prompt.
- Chỉ load đúng lát cắt context cho task hiện tại.

## D. Search & discovery

### 1. Search stack nên có

- keyword search
- semantic search
- git history search
- GitHub issues / PR / discussions
- spec / docs lookup
- submodule awareness

### 2. Mục tiêu search

- Không phải đọc mọi thứ.
- Mà là tìm đúng thứ cần đọc nhanh hơn.

### 3. Repo lạ

- Discovery trước execution.
- Không biết repo không sao; nguy hiểm là không biết mà vẫn hành động như đã biết.

## E. Workflow BMAD

1. Load system prompt
2. Load role / workflow rules
3. Load project context
4. Cheap scan repo
5. Pattern scan code
6. Search context
7. Deep read đúng vùng liên quan
8. Analysis / brainstorming
9. Planning
10. Verify assumptions
11. Execute / handoff
12. QA / đánh kết quả

## F. Role playing

- Role không phải để “diễn”, mà để ép góc nhìn rõ ràng.
- Các role chính:
  - architect
  - domain analyst
  - code analyst
  - planner
  - QA / verifier
  - maintainer
- Giá trị: giảm lẫn lộn, tăng chiều sâu phân tích, dễ scale workflow.

## G. Planning & control

### 1. Planning quan trọng hơn execution nhanh trên hướng sai

- Planning quyết định hướng đi.
- Planning chọn đúng context.
- Planning expose assumption, risk, checkpoint.

### 2. Viết code dễ, kiểm soát khó hơn

- Trong thời AI, code generation rẻ hơn.
- Nhưng context control, quality control, correctness, blast radius khó hơn.

### 3. 4 điểm kiểm soát

- planning
- test tự động
- con người
- artifact / spec / context

### 4. Chất lượng không đàm phán

- Không bỏ verify chỉ để nhanh.
- Không hy sinh maintainability một cách mù quáng.
- AI có thể tăng tốc, nhưng không được hạ chuẩn.

## H. Spec & documents

### 1. BMAD vs SpecKit

- SpecKit = clarity of input
- BMAD = intelligence of execution
- Hai cái nên đi cùng nhau.

### 2. Spec là living document

Flow:

- create
- link context
- validate
- execute against spec
- update on learning
- version / archive
- verify after implementation

### 3. Chưng cất PRD

- Biến PRD dài thành artifact ngắn, dùng được:
  - summary
  - scope
  - rules
  - acceptance criteria
  - ambiguity log
  - impact area

### 4. Document là infrastructure của understanding

- Tài liệu tốt phải:
  - rõ mục đích
  - đúng độ sâu
  - dễ search
  - dễ update
  - có lifecycle
  - tách fact / assumption / opinion

### 5. Offload know-how vào .md

- Project context
- glossary
- decision log
- architecture notes
- QA checklist
- lessons learned

## I. Domain understanding

### 1. Làm kỹ data model

- Data model lộ ra business concept, relationship, lifecycle, invariant, hidden constraint.
- Muốn hiểu sâu nghiệp vụ thì thường phải hiểu sâu data model.

### 2. Domain invariants

- Khi chưa hiểu hết một lĩnh vực, hãy tìm cái bất biến của nó.
- Ví dụ:
  - finance = correctness, auditability
  - security = least privilege, threat awareness
  - healthcare = safety, traceability

### 3. Đánh giá khi không có chuyên môn

- Đừng giả làm expert.
- Tách fact / inference / judgment.
- Dùng checklist.
- Hỏi expert-style questions.
- Nhờ expert ở điểm leverage cao.

## J. Thinking tools

### 1. Socratic questions

- vấn đề thật là gì?
- symptom hay root cause?
- assumption nào đang ẩn?
- evidence ở đâu?
- còn cách diễn giải nào khác không?

### 2. Câu hỏi “tại sao”

- Dùng để đào xuống rationale, root cause, hidden constraint.

### 3. Design thinking

- Giúp chọn đúng problem để giải.
- Tập trung vào problem framing, user/business need, option exploration, feedback sớm.

## K. People & transformation

### 1. Dev → expert

- Từ execution-centric sang judgment-centric.
- Từ local optimization sang context-aware, planning-oriented, trade-off thinking.

### 2. AI làm scope tăng mạnh

- Tăng bề rộng context, chiều sâu analysis, số lượng option, số lượng artifact có thể maintain.
- BMAD giúp kiểm soát sự mở rộng này.

### 3. Productivity vs delivery

- Productivity = làm nhanh hơn.
- Delivery = tạo ra nhiều kết quả hoàn chỉnh hơn.
- AI/BMAD có thể tăng delivery mạnh hơn productivity nếu workflow được tái thiết kế đúng.

### 4. Người dùng AI tốt là người đổi tư duy

- Không chỉ biết prompt.
- Mà biết context, workflow, planning, verification, knowledge system.

## L. Risks in the AI era

### 1. Tối ưu sai mục tiêu

- Paperclip maximizer mindset: tối ưu mạnh nhưng lệch mục tiêu thật.

### 2. Software như short content

- Sản xuất nhanh, tiêu thụ nhanh, nhiều volume nhưng thiếu chiều sâu hệ thống.

### 3. Người không chuyển đổi dễ tụt lại

- Không phải AI tự động thay tất cả.
- Nhưng người biết leverage AI tốt hơn sẽ thay người không chịu đổi cách làm việc.

## M. Skills

### 1. Skill là gì?

- Skill là kỹ năng đã được chưng cất đủ để lặp lại.
- Có pattern, input/output, workflow, quality bar tương đối rõ.

### 2. Skill writer

- Biến know-how thành workflow có thể tái sử dụng.
- Giúp chuẩn hoá cách làm, scale chất lượng, giảm phụ thuộc cá nhân.

## N. Agent architecture

- Agent không chỉ là system prompt.
- Agent gồm:
  - system prompt
  - context
  - workflow
  - tools
  - memory / artifacts
  - output structure
  - verification loop

## O. One-line takeaways

- BMAD không chỉ là prompt; nó là operating model.
- Context đúng quan trọng hơn prompt hay.
- Read cheap before read deep.
- Planning tốt quan trọng hơn execution nhanh trên hướng sai.
- Viết code dễ, kiểm soát khó hơn.
- Chất lượng là thứ không đem ra mặc cả.
- Spec phải là living document.
- Document là infrastructure của understanding.
- Skill là kỹ năng đã được chưng cất đủ để lặp lại.
- AI mở rộng scope; BMAD giúp kiểm soát scope đó.
- Agent không chỉ là system prompt.
- Khi chưa hiểu lĩnh vực, hãy tìm cái bất biến của nó.


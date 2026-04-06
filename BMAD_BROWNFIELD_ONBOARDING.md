# BMAD Brownfield Onboarding

## Mục tiêu

Tài liệu này mô tả cách dùng BMAD để **onboard vào một brownfield project** và tạo ra **as-is docs** cho feature, module, và workflow đã tồn tại.

Mục tiêu của brownfield onboarding không phải là hiểu toàn bộ repo hay redesign sớm.
Mục tiêu là **hiểu đủ đúng để làm việc an toàn**, rồi chưng cất understanding thành artifact có thể search, review, handoff.

## First principles

- **Feature-first, không system-first**
- **As-is trước, to-be sau**
- **Search trước, deep read sau**
- **Facts trước, opinions sau**
- Luôn tách rõ:
  - **Fact**
  - **Inference**
  - **Unknown**

## Khi nào dùng

Dùng khi:
- mới vào repo cũ
- cần hiểu một feature/workflow đã tồn tại
- cần viết doc as-is để handoff / review / maintain
- chuẩn bị refactor nhưng chưa hiểu hiện trạng

Không cần full flow khi:
- fix nhỏ, local, rollback dễ
- bug đã rõ vùng ảnh hưởng

## Flow ngắn gọn

```text
Orient
  -> bmad-help

Cheap repo scan
  -> bmad-quick-dev

Choose a slice
  -> bmad-product-brief

Pattern scan
  -> bmad-quick-dev

Functional understanding (as-is)
  -> bmad-create-prd

Technical understanding (as-is)
  -> bmad-create-architecture

Readiness / gap check
  -> bmad-check-implementation-readiness

Create onboarding artifacts
  -> bmad-create-story

Review quality
  -> bmad-code-review
  -> bmad-review-edge-case-hunter
  -> bmad-review-adversarial-general

Retrospective / synthesis
  -> bmad-retrospective
```

## Workflow reference

| Workflow item | Action / Command | Meaning | Output | Command prompt nếu cần |
|---|---|---|---|---|
| Orient | `bmad-help` | Chốt đây là brownfield reverse-documentation | Scope ngắn, flow phù hợp | `Tôi đang onboard vào một brownfield project. Mục tiêu là reconstruct tài liệu as-is cho feature/module/workflow đã tồn tại, không redesign vội. Hãy giúp tôi chọn mức BMAD phù hợp và flow discovery phù hợp.` |
| Cheap repo scan | `bmad-quick-dev` | Scan rẻ để lấy system shape sơ bộ | Repo map, entry points, hot modules, keywords | `Hãy làm cheap scan cho repo này. Đừng đọc sâu toàn bộ codebase. Mục tiêu: xác định cấu trúc repo, entry points, module/domain quan trọng, keywords đáng chú ý, và đề xuất lát cắt đầu tiên để đào sâu. Tách fact và inference.` |
| Choose a slice | `bmad-product-brief` | Chọn 1 feature/workflow/module để đào | Brief về scope, actors, boundaries, expected artifacts | `Tạo brief cho brownfield feature <feature-name>. Mục tiêu là hiểu và document trạng thái hiện tại. Hãy chốt business purpose, actors, in-scope/out-of-scope, entry points nghi ngờ, expected artifacts.` |
| Pattern scan | `bmad-quick-dev` | Search trước khi đọc sâu | Candidate files/classes, write path, read path, integrations | `Hãy pattern scan feature <feature-name> trong repo. Tìm entry points, service/use case, DB tables, events/messages, external dependencies. Chỉ gom evidence và nhóm theo write path / read path / integration path.` |
| Functional understanding | `bmad-create-prd` | Reconstruct behavior thật của feature | Happy path, alternate flows, validation rules, side effects | `Từ evidence đã tìm được, hãy tạo Functional Spec (As-Is) cho feature <feature-name>. Bao gồm goal, actors, triggers, happy path, alternate flows, validation rules, side effects, ambiguity log. Chỉ ghi điều có evidence.` |
| Technical understanding | `bmad-create-architecture` | Map technical shape dạng as-is | Text diagram, components, source of truth, write/read path, risks | `Tạo Technical Notes dạng as-is cho feature <feature-name>. Mô tả text diagram, involved components/modules, write path/read path, sync/async interactions, source of truth, failure points, extension risks.` |
| Readiness / gap check | `bmad-check-implementation-readiness` | Kiểm tra đã hiểu đủ để sửa an toàn chưa | Readiness verdict, gap list, unknowns, câu hỏi cần confirm | `Hãy review bộ tài liệu as-is cho feature <feature-name> và đánh giá readiness. Chỉ ra fact, inference, unknown, và thiếu gì để dev mới có thể sửa feature an toàn.` |
| Create artifacts | `bmad-create-story` | Chia understanding thành artifact onboarding | `Feature Snapshot`, `Functional Spec (As-Is)`, `Technical Notes`, `Open Questions and Gaps` | `Hãy chia kết quả reverse-documentation của feature <feature-name> thành các artifact onboarding cụ thể: Feature Snapshot, Functional Spec (As-Is), Technical Notes, Open Questions and Gaps. Tách rõ fact / assumption / unknown.` |
| Review quality | `bmad-code-review` / `bmad-review-edge-case-hunter` / `bmad-review-adversarial-general` | Soát doc để tránh overclaim | Missing edge cases, hidden assumptions, contradictions | `Hãy review bộ brownfield onboarding docs cho feature <feature-name>. Tập trung vào missing edge cases, hidden assumptions, overclaim, mâu thuẫn giữa functional doc và technical doc.` |
| Retrospective / synthesis | `bmad-retrospective` | Rút kinh nghiệm và tổng hợp lên level cao hơn | Lessons learned, glossary/module/context candidates | `Từ quá trình reverse-documentation feature <feature-name>, hãy capture lesson learned: phần nào khó hiểu nhất, knowledge gap ở đâu, nên bổ sung artifact nào cho lần sau, template nào nên chuẩn hóa.` |

## Core artifacts

### Per feature

- **Feature Snapshot**
  - Purpose
  - Actors
  - Entry Points
  - Main Components
  - Main Flow
  - Risks / Unknowns

- **Functional Spec (As-Is)**
  - Goal
  - Trigger
  - Happy Path
  - Alternate / Error Flows
  - Validation Rules
  - Side Effects
  - Data Changes

- **Technical Notes**
  - System Shape
  - Source of Truth
  - Write Path
  - Read Path
  - Sync/Async Dependencies
  - Failure Modes
  - Extension Risks

- **Open Questions and Gaps**
  - Facts
  - Inferences
  - Assumptions
  - Unknowns
  - Need Confirmation From
  - Suspected Tech Debt

### Across multiple features

- **Glossary**
- **Module Map**
- **Dependency Map**
- **Architecture Overview (As-Is)**
- **Context Map Draft**

## Usage order

```text
Per feature:
Feature Snapshot
  -> Functional Spec (As-Is)
  -> Technical Notes
  -> Open Questions and Gaps

Across multiple features:
Glossary
  -> Module Map
  -> Dependency Map
  -> Architecture Overview (As-Is)
  -> Context Map Draft
```

## Rule of thumb

- **Feature docs = evidence**
- **System-level docs = synthesis**
- Chỉ tổng hợp lên architecture/context khi đã có đủ evidence từ nhiều feature thật
- Brownfield onboarding là bài toán **reconstruct reality**, không phải invent narrative

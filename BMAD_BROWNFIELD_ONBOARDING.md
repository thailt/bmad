# BMAD Brownfield Onboarding

## Mục tiêu

Tài liệu này mô tả cách dùng BMAD để **onboard vào một brownfield project** và tạo ra **as-is documents** cho feature, module, và workflow đã tồn tại.

Brownfield onboarding không bắt đầu bằng thiết kế mới.
Nó bắt đầu bằng việc **reconstruct reality** từ code, schema, contract, runtime behavior, và knowledge của team.

BMAD ở đây được dùng như một **operating workflow để hiểu hệ thống cũ có cấu trúc**, không phải để tạo ra tài liệu đẹp nhưng sai trạng thái thật.

---

## 1. First Principles

### 1.1 Mục tiêu thật của onboarding brownfield

Không phải:
- đọc hết repo
- viết tài liệu tổng quát quá sớm
- cố hiểu mọi thứ trước khi chạm vào một feature thật

Mà là:
- hiểu đủ để làm việc an toàn
- xác định đúng feature/module/workflow cần đào
- tái dựng behavior hiện tại của hệ thống
- chưng cất understanding thành artifact có thể search, review, handoff

### 1.2 Source of truth trong brownfield thường phân tán

Thông tin thật thường nằm rải ra ở:
- code
- database schema
- API contracts
- event definitions
- config / feature flags
- tests
- logs / monitoring
- tribal knowledge từ dev hoặc domain owner

Vì vậy onboarding brownfield phải luôn phân biệt:
- **Fact**: thấy rõ từ code, schema, runtime, hoặc tài liệu đáng tin
- **Inference**: suy luận hợp lý từ evidence
- **Unknown**: chưa đủ bằng chứng để kết luận

### 1.3 Quy tắc cốt lõi

- **Read cheap before read deep**
- **Discovery before execution**
- **Feature-first before system-wide docs**
- **As-is documentation before to-be design**
- **Facts before opinions**

---

## 2. Khi nào dùng flow này?

Dùng khi:
- mới vào một repo cũ
- phải hiểu một chức năng đã có sẵn
- cần document hóa feature cũ để handoff / review / maintain
- cần giảm phụ thuộc vào knowledge trong đầu một vài người
- chuẩn bị refactor nhưng chưa hiểu hiện trạng

Không dùng full flow này khi:
- task chỉ là fix rất nhỏ, local, rollback dễ
- chỉ cần tìm nhanh một bug đã rõ vùng ảnh hưởng

---

## 3. Brownfield Onboarding Flow

```text
0. Orient
1. Cheap repo scan
2. Choose a slice
3. Pattern scan
4. Deep read relevant paths
5. Reconstruct actual behavior
6. Extract business rules and invariants
7. Write as-is artifacts
8. Review gaps and unknowns
9. Expand upward if needed
```

---

## 4. Step-by-Step Flow with BMAD Commands

## Step 0. Orient

### Goal
Hiểu BMAD nên dùng ở mức nào, và xác định task là **onboarding / reverse-documentation**, không phải greenfield design.

### Expected output
- onboarding scope ngắn
- level of BMAD cần dùng
- initial hypothesis về feature/module/workflow cần đào

### Suggested command
```text
bmad-help
```

### Usage note
- nêu rõ đây là brownfield project
- nói mục tiêu là tạo tài liệu **as-is**
- yêu cầu workflow thiên về discovery hơn là implementation

### Prompt shape
```text
Tôi đang onboard vào một brownfield project.
Mục tiêu là reconstruct tài liệu as-is cho feature/module/workflow đã tồn tại,
không redesign vội.
Hãy giúp tôi chọn mức BMAD phù hợp và flow discovery phù hợp.
```

---

## Step 1. Cheap Repo Scan

### Goal
Lấy **system shape sơ bộ** mà không đọc sâu toàn repo.

### Read first
- README
- docs index
- package/module structure
- build files
- config directories
- migrations / schema
- API specs
- message / event definitions
- deployment manifests nếu cần

### Expected output
- repo map ngắn
- suspected entry points
- suspected hot modules
- glossary keyword ban đầu

### Suggested command
```text
bmad-quick-dev
```

### Why this command?
Ở bước này cần discovery có định hướng, chưa cần full planning chain.

### Prompt shape
```text
Hãy làm cheap scan cho repo này.
Đừng đọc sâu toàn bộ codebase.
Mục tiêu:
1. xác định cấu trúc repo
2. tìm entry points chính
3. tìm module/domain quan trọng
4. liệt kê keywords/business terms đáng chú ý
5. đề xuất lát cắt đầu tiên để đào sâu
Output ngắn gọn, tách fact và inference.
```

---

## Step 2. Choose a Slice

### Goal
Không onboard bằng cách hiểu “cả hệ thống” một lần.
Phải chọn một **slice** cụ thể:
- 1 feature
- 1 workflow
- hoặc 1 bounded module

### Good slice examples
- user registration
- membership renewal
- order confirmation
- payment callback handling
- notification dispatch

### Expected output
- chosen slice
- boundaries ban đầu
- success criteria cho tài liệu sẽ tạo

### Suggested command
```text
bmad-product-brief
```

### Why this command?
Trong brownfield, “brief” không phải để phát minh requirement mới, mà để chốt:
- vấn đề đang muốn hiểu
- scope hiểu đến đâu
- actor nào liên quan
- thành công của onboarding slice là gì

### Prompt shape
```text
Tạo brief cho brownfield feature `<feature-name>`.
Mục tiêu không phải thiết kế mới mà là hiểu và document trạng thái hiện tại.
Hãy chốt:
- business purpose
- actors
- in-scope / out-of-scope
- entry points nghi ngờ
- expected artifacts sau khi phân tích
```

---

## Step 3. Pattern Scan

### Goal
Search trước khi deep read.
Tìm đúng vùng code cần đọc.

### What to search
- endpoint path
- controller/router names
- service/use case names
- table names
- event names
- DTO/request/response names
- feature flags
- scheduler/job names
- domain keywords

### Expected output
- candidate files/classes
- likely write paths
- likely read paths
- external integrations liên quan

### Suggested command
```text
bmad-quick-dev
```

### Prompt shape
```text
Hãy pattern scan feature `<feature-name>` trong repo.
Tìm:
- entry points
- service/usecase liên quan
- DB tables liên quan
- events / messages liên quan
- external dependencies
Đừng kết luận quá sớm; chỉ gom evidence và nhóm theo write path / read path / integration path.
```

---

## Step 4. Deep Read Relevant Paths

### Goal
Đọc sâu đúng chỗ để reconstruct behavior thật.

### Deep read priority
1. write path
2. validation logic
3. state transition
4. side effects
5. async consumers/producers
6. failure handling
7. tests nếu có

### Expected output
- happy path
- alternate paths
- validation rules
- state changes
- side effects

### Suggested command
```text
bmad-create-prd
```

### Why this command?
Trong brownfield, có thể dùng command này như một cách tạo **functional understanding artifact** cho trạng thái hiện tại.
Nó buộc ta phải làm rõ:
- scope
- actors
- rules
- acceptance-like behavior
- ambiguity

### Prompt shape
```text
Từ evidence đã tìm được, hãy tạo tài liệu functional understanding cho feature `<feature-name>` theo trạng thái as-is.
Bao gồm:
- goal
- actors
- triggers
- happy path
- alternate flows
- validation rules
- side effects
- ambiguity log
Chỉ ghi điều có evidence. Chỗ chưa chắc để vào unknowns/assumptions.
```

---

## Step 5. Reconstruct Technical Shape

### Goal
Biến understanding chức năng thành technical map.

### Things to capture
- component interaction
- source of truth
- sync vs async path
- ownership
- external systems
- consistency expectations
- failure points

### Expected output
- system-shape text diagram
- dependency notes
- write/read ownership
- risk points

### Suggested command
```text
bmad-create-architecture
```

### Prompt shape
```text
Tạo architecture note dạng as-is cho feature `<feature-name>`.
Đây là brownfield reverse-documentation, không phải future-state design.
Hãy mô tả:
- text diagram
- involved components/modules
- write path / read path
- sync/async interactions
- source of truth
- failure points
- extension risks
```

---

## Step 6. Validate Readiness of Understanding

### Goal
Kiểm tra đã hiểu đủ để người khác làm việc tiếp chưa.

### Checklist
- scope đã rõ chưa?
- entry points đã đủ chưa?
- business rules còn lỗ hổng nào?
- unknowns nào blocking?
- phần nào mới là inference chứ chưa phải fact?
- có đủ để sửa/extend feature an toàn chưa?

### Expected output
- readiness verdict
- gap list
- câu hỏi cần confirm với owner hoặc domain expert

### Suggested command
```text
bmad-check-implementation-readiness
```

### Prompt shape
```text
Hãy review bộ tài liệu as-is cho feature `<feature-name>` và đánh giá readiness.
Kiểm tra:
- chỗ nào đã là fact
- chỗ nào mới là inference
- chỗ nào còn unknown
- thiếu gì để dev mới có thể sửa feature an toàn
- câu hỏi nào cần confirm với domain owner
```

---

## Step 7. Create Final Brownfield Artifacts

### Goal
Tách understanding thành các artifact dễ dùng cho onboarding.

### Recommended artifacts
```text
docs/features/<feature-name>/
  01-feature-snapshot.md
  02-functional-spec-as-is.md
  03-technical-notes.md
  04-open-questions-and-gaps.md
```

### Suggested command
```text
bmad-create-story
```

### Why this command?
Không phải để code ngay, mà để chia hiểu biết thành **artifact units**:
- feature snapshot
- as-is functional spec
- technical notes
- gap log

Nếu muốn, có thể dùng nó để tạo task tiếp theo như:
- confirm unknowns
- add missing tests
- verify event contract
- write glossary

### Prompt shape
```text
Hãy chia kết quả reverse-documentation của feature `<feature-name>` thành các artifact onboarding cụ thể:
1. Feature Snapshot
2. Functional Spec (As-Is)
3. Technical Notes
4. Open Questions and Gaps
Mỗi artifact phải ngắn gọn, search-friendly, và tách rõ fact / assumption / unknown.
```

---

## Step 8. Review Documentation Quality

### Goal
Đảm bảo doc không “nghe hay nhưng sai”.

### What to review
- có khẳng định nào thiếu evidence không?
- có bỏ sót edge cases quan trọng không?
- có nhầm business rule với implementation detail không?
- có phần nào mơ hồ khiến người sau hiểu sai không?
- có phần nào nên tách thành glossary / ADR / architecture note riêng không?

### Suggested commands
```text
bmad-review-edge-case-hunter
bmad-review-adversarial-general
bmad-code-review
```

### Prompt shape
```text
Hãy review bộ brownfield onboarding docs cho feature `<feature-name>`.
Tập trung vào:
- missing edge cases
- overclaimed conclusions
- hidden assumptions
- mâu thuẫn giữa functional doc và technical doc
- các điểm có thể gây sai khi onboarding dev mới
```

---

## Step 9. Capture Lessons and Expand Upward

### Goal
Sau khi có nhiều feature docs, mới tổng hợp dần lên level cao hơn.

### Expand from feature docs to
- glossary
- module map
- context map
- bounded context draft
- dependency map
- architecture overview
- decision log

### Suggested command
```text
bmad-retrospective
```

### Prompt shape
```text
Từ quá trình reverse-documentation feature `<feature-name>`,
hãy capture lesson learned:
- phần nào khó hiểu nhất
- knowledge gap ở đâu
- nên bổ sung artifact nào cho lần sau
- template nào nên chuẩn hóa cho các feature khác
```

---

## 5. Recommended Command Chains

## A. Quick Brownfield Onboarding for One Small Feature

```text
bmad-help
→ bmad-quick-dev
→ bmad-product-brief
→ bmad-create-prd
→ bmad-create-architecture
→ bmad-code-review
```

Use when:
- feature vừa/nhỏ
- cần doc nhanh
- ít module liên quan

---

## B. Standard Brownfield Documentation Flow

```text
bmad-help
→ bmad-product-brief
→ bmad-quick-dev
→ bmad-create-prd
→ bmad-create-architecture
→ bmad-check-implementation-readiness
→ bmad-create-story
→ bmad-code-review
→ bmad-retrospective
```

Use when:
- feature có mơ hồ vừa phải
- chạm vài module
- cần artifact đủ dùng cho onboarding thật

---

## C. High-Risk Brownfield Understanding Flow

```text
bmad-help
→ bmad-product-brief
→ bmad-create-prd
→ bmad-create-architecture
→ bmad-check-implementation-readiness
→ bmad-review-edge-case-hunter
→ bmad-review-adversarial-general
→ bmad-code-review
→ bmad-retrospective
```

Use when:
- feature chạm contract / schema / event
- rollback khó
- rất dễ hiểu sai domain rule
- tài liệu sẽ được dùng cho refactor hoặc redesign tiếp theo

---

## 6. Core Brownfield Artifacts

## 6.1 Feature Snapshot

```md
# Feature Snapshot: <feature-name>

## Purpose
## Actors
## Entry Points
## Main Components
## Main Flow
## Data / Contracts
## Business Rules
## Risks / Constraints
## Unknowns
```

## 6.2 Functional Spec (As-Is)

```md
# Functional Spec (As-Is): <feature-name>

## Goal
## Actors
## Preconditions
## Trigger
## Happy Path
## Alternate / Error Flows
## Validation Rules
## Side Effects
## Data Changes
## Observability
## Out of Scope
```

## 6.3 Technical Notes

```md
# Technical Notes: <feature-name>

## System Shape
## Source of Truth
## Write Path
## Read Path
## Sync/Async Dependencies
## Failure Modes
## Performance Notes
## Extension Risks
```

## 6.4 Open Questions and Gaps

```md
# Open Questions and Gaps: <feature-name>

## Facts
## Inferences
## Assumptions
## Unknowns
## Need Confirmation From
## Suspected Tech Debt
```

---

## 7. Prompt Pack

## Prompt 1 — Start Discovery

```text
Tôi đang onboard vào một brownfield project.
Hãy dùng BMAD theo hướng reverse-documentation.
Mục tiêu là hiểu và document feature `<feature-name>` theo trạng thái as-is.
Bắt đầu bằng cheap scan, pattern scan, rồi đề xuất vùng cần deep read.
Tách rõ fact / inference / unknown.
```

## Prompt 2 — Build Functional Understanding

```text
Từ evidence đã tìm được, hãy tạo tài liệu Functional Spec (As-Is) cho feature `<feature-name>`.
Không redesign.
Không ghi điều chưa có evidence như fact.
Bao gồm actors, trigger, happy path, alternate flows, validation rules, side effects, data changes.
```

## Prompt 3 — Build Technical Understanding

```text
Hãy tạo Technical Notes dạng as-is cho feature `<feature-name>`.
Mô tả text diagram, component interaction, source of truth, write/read path, sync-async dependencies, failure points, extension risks.
```

## Prompt 4 — Review Documentation Quality

```text
Hãy review bộ brownfield onboarding docs cho feature `<feature-name>`.
Tìm edge cases bị thiếu, hidden assumptions, overclaim, mâu thuẫn giữa doc và code understanding.
```

---

## 8. Common Mistakes

### Sai lầm 1: Viết architecture doc tổng quát quá sớm
Hậu quả:
- đẹp nhưng rỗng
- không giúp sửa feature thật
- dễ sai ở vùng quan trọng

### Sai lầm 2: Không tách fact với inference
Hậu quả:
- tài liệu nghe hợp lý nhưng không đáng tin

### Sai lầm 3: Cố preload toàn repo
Hậu quả:
- tốn token
- loãng focus
- khó giữ đúng context

### Sai lầm 4: Document theo “mong muốn hệ thống nên như thế nào”
Hậu quả:
- onboarding sai trạng thái thật
- refactor dựa trên premise sai

### Sai lầm 5: Bỏ qua unknowns
Hậu quả:
- dev mới tưởng đã hiểu đủ
- sửa sai chỗ nguy hiểm

---

## 9. Rule of Thumb

- **Onboard theo feature trước, không theo cả hệ thống trước**
- **As-is trước, to-be sau**
- **Search trước, deep read sau**
- **Write short docs early, refine later**
- **Tài liệu phải nói rõ điều gì là fact, điều gì chưa chắc**

---

## 10. One-Line Takeaways

- Brownfield onboarding là bài toán **reconstruct reality**, không phải invent narrative.
- BMAD giúp biến việc hiểu code cũ thành một workflow có cấu trúc.
- Feature-level artifacts là điểm bắt đầu đúng.
- Tài liệu as-is tốt sẽ giảm rework, giảm tribal knowledge, và làm refactor an toàn hơn.

---

## 11. Brownfield Workflow Reference Table

Bảng này dùng như một **cheat sheet** khi chạy BMAD cho brownfield onboarding.
Mục tiêu là nhìn nhanh được:
- đang ở bước nào
- nên dùng action/command gì
- output mong đợi là gì
- prompt nào nên dùng nếu cần

| Workflow item | Action / Command | Meaning | Output | Command prompt nếu cần |
|---|---|---|---|---|
| 0. Orient | `bmad-help` | Xác định đây là **brownfield reverse-documentation**, không phải greenfield design | Scope ngắn, cách tiếp cận phù hợp, level BMAD cần dùng | `Tôi đang onboard vào một brownfield project. Mục tiêu là reconstruct tài liệu as-is cho feature/module/workflow đã tồn tại, không redesign vội. Hãy giúp tôi chọn mức BMAD phù hợp và flow discovery phù hợp.` |
| 1. Cheap Repo Scan | `bmad-quick-dev` | Scan rẻ để lấy **system shape sơ bộ** mà không đọc sâu toàn repo | Repo map ngắn, entry points nghi ngờ, hot modules, glossary keywords ban đầu | `Hãy làm cheap scan cho repo này. Đừng đọc sâu toàn bộ codebase. Mục tiêu: (1) xác định cấu trúc repo (2) tìm entry points chính (3) tìm module/domain quan trọng (4) liệt kê keywords/business terms đáng chú ý (5) đề xuất lát cắt đầu tiên để đào sâu. Output ngắn gọn, tách fact và inference.` |
| 2. Choose a Slice | `bmad-product-brief` | Chốt **1 feature / workflow / module** để đào, tránh ôm cả hệ thống | Brief cho feature: purpose, actors, in-scope/out-of-scope, expected artifacts | `Tạo brief cho brownfield feature <feature-name>. Mục tiêu không phải thiết kế mới mà là hiểu và document trạng thái hiện tại. Hãy chốt: business purpose, actors, in-scope/out-of-scope, entry points nghi ngờ, expected artifacts sau khi phân tích.` |
| 3. Pattern Scan | `bmad-quick-dev` | Search trước khi deep read để tìm đúng vùng code | Candidate files/classes, write path, read path, integration path | `Hãy pattern scan feature <feature-name> trong repo. Tìm: entry points, service/use case liên quan, DB tables liên quan, events/messages liên quan, external dependencies. Đừng kết luận quá sớm; chỉ gom evidence và nhóm theo write path / read path / integration path.` |
| 4. Deep Read Relevant Paths | `bmad-create-prd` | Đọc sâu các path liên quan để reconstruct behavior thật | Functional understanding draft: happy path, alternate flows, validation rules, side effects | `Từ evidence đã tìm được, hãy tạo tài liệu functional understanding cho feature <feature-name> theo trạng thái as-is. Bao gồm: goal, actors, triggers, happy path, alternate flows, validation rules, side effects, ambiguity log. Chỉ ghi điều có evidence. Chỗ chưa chắc để vào unknowns/assumptions.` |
| 5. Reconstruct Technical Shape | `bmad-create-architecture` | Biến functional understanding thành **technical map as-is** | Text diagram, involved components, source of truth, write/read path, failure points, risks | `Tạo architecture note dạng as-is cho feature <feature-name>. Đây là brownfield reverse-documentation, không phải future-state design. Hãy mô tả: text diagram, involved components/modules, write path/read path, sync/async interactions, source of truth, failure points, extension risks.` |
| 6. Validate Readiness of Understanding | `bmad-check-implementation-readiness` | Kiểm tra đã hiểu **đủ để sửa an toàn chưa** | Readiness verdict, gap list, unknowns, câu hỏi cần confirm | `Hãy review bộ tài liệu as-is cho feature <feature-name> và đánh giá readiness. Kiểm tra: chỗ nào đã là fact, chỗ nào mới là inference, chỗ nào còn unknown, thiếu gì để dev mới có thể sửa feature an toàn, câu hỏi nào cần confirm với domain owner.` |
| 7. Create Final Brownfield Artifacts | `bmad-create-story` | Chia understanding thành các artifact onboarding/search-friendly | `Feature Snapshot`, `Functional Spec (As-Is)`, `Technical Notes`, `Open Questions and Gaps` | `Hãy chia kết quả reverse-documentation của feature <feature-name> thành các artifact onboarding cụ thể: (1) Feature Snapshot (2) Functional Spec (As-Is) (3) Technical Notes (4) Open Questions and Gaps. Mỗi artifact phải ngắn gọn, search-friendly, và tách rõ fact / assumption / unknown.` |
| 8. Review Documentation Quality | `bmad-review-edge-case-hunter` / `bmad-review-adversarial-general` / `bmad-code-review` | Soát chất lượng doc để tránh “nghe hay nhưng sai” | Missing edge cases, hidden assumptions, overclaims, contradictions | `Hãy review bộ brownfield onboarding docs cho feature <feature-name>. Tập trung vào: missing edge cases, overclaimed conclusions, hidden assumptions, mâu thuẫn giữa functional doc và technical doc, các điểm có thể gây sai khi onboarding dev mới.` |
| 9. Capture Lessons and Expand Upward | `bmad-retrospective` | Rút kinh nghiệm và tổng hợp từ feature-level lên module/system-level | Lessons learned, template improvements, glossary/module/context candidates | `Từ quá trình reverse-documentation feature <feature-name>, hãy capture lesson learned: phần nào khó hiểu nhất, knowledge gap ở đâu, nên bổ sung artifact nào cho lần sau, template nào nên chuẩn hóa cho các feature khác.` |

---

## 12. Artifact Usage Map

Sau khi chạy workflow cho từng feature, nên dùng các artifact như sau.

| Artifact | Dùng để làm gì trong BMAD | Khi nào nên tạo | Ghi chú |
|---|---|---|---|
| Feature Snapshot | Entry document để vào feature nhanh | Ngay sau khi hiểu feature ở mức cơ bản | Tốt cho onboarding và handoff |
| Functional Spec (As-Is) | Khóa behavior hiện tại của hệ thống | Sau deep read và reconstruct behavior | Dùng làm baseline cho bug analysis, regression, change planning |
| Technical Notes | Bản đồ kỹ thuật để sửa/refactor an toàn | Sau khi đã nắm write path/read path/source of truth | Nên ưu tiên ghi rõ transaction, side effects, failure points |
| Open Questions and Gaps | Quản trị uncertainty, tránh overclaim | Song song với các artifact khác | Bắt buộc phải tách fact / inference / unknown |
| Glossary | Chuẩn hóa ubiquitous language | Sau khi có vài feature docs | Tránh mỗi người hiểu domain một kiểu |
| Module Map | Tổng hợp trách nhiệm thực tế của module | Khi có evidence từ nhiều feature | Dùng cho boundary analysis và ownership clarification |
| Dependency Map | Thấy dependency thật và blast radius | Khi bắt đầu thấy pattern phụ thuộc lặp lại | Tách rõ sync vs async |
| Architecture Overview (As-Is) | Nhìn macro system shape nhưng vẫn bám reality | Khi đã có đủ feature-level evidence | Không nên viết từ suy đoán |
| Context Map Draft | Draft domain/context boundary từ evidence thật | Khi muốn nâng từ code understanding lên domain understanding | Chỉ nên làm sau khi đã có đủ feature docs |

---

## 13. Recommended Usage Order

```text
Per feature:
Feature Snapshot
→ Functional Spec (As-Is)
→ Technical Notes
→ Open Questions and Gaps

Across multiple features:
Glossary
→ Module Map
→ Dependency Map
→ Architecture Overview (As-Is)
→ Context Map Draft
```

Quy tắc:
- **Feature docs = evidence**
- **System-level docs = synthesis**
- Chỉ nên tổng hợp lên architecture/context khi đã có đủ evidence từ nhiều feature thật.

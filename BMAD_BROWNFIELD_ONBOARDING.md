# BMAD Brownfield Onboarding

## Mục tiêu

Tài liệu này mô tả cách dùng BMAD để **onboard vào một brownfield project** và tạo ra **document as-is** cho các chức năng đã tồn tại.

Brownfield onboarding không bắt đầu bằng thiết kế mới.
Nó bắt đầu bằng việc **reconstruct reality** từ code, schema, contract, behavior đang chạy, và knowledge của team.

BMAD ở đây được dùng như một **operating workflow để hiểu hệ thống cũ có cấu trúc**, không phải để bịa ra tài liệu đẹp nhưng sai.

---

## 1. First Principles

### 1.1 Mục tiêu thật của onboarding brownfield

Không phải:
- đọc hết repo
- viết tài liệu tổng quát quá sớm
- cố hiểu mọi thứ trước khi làm gì

Mà là:
- hiểu đủ để làm việc an toàn
- xác định được feature/module nào liên quan
- tái dựng behavior hiện tại của hệ thống
- chưng cất understanding thành artifact có thể search, review, handoff

### 1.2 Source of truth trong brownfield thường phân tán

Thông tin thật thường nằm rải ra ở:
- code
- database schema
- API contracts
- event definitions
- configs / feature flags
- tests
- logs / monitoring
- tribal knowledge từ dev/domain owner

Vì vậy onboarding brownfield phải luôn phân biệt:
- **Fact**: thấy rõ từ code/schema/runtime
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
- phải hiểu chức năng đã có sẵn
- cần document hóa feature cũ để handoff / review / maintain
- cần giảm phụ thuộc vào knowledge trong đầu một vài người
- chuẩn bị refactor nhưng chưa hiểu hiện trạng

Không dùng full flow này khi:
- task chỉ là fix rất nhỏ, local, rollback dễ
- chỉ cần tìm nhanh một bug đã biết rõ vùng ảnh hưởng

---

## 3. Brownfield Onboarding Flow

```text
0. Orient
1. Cheap repo scan
2. Choose feature or module slice
3. Pattern scan
4. Deep read relevant paths
5. Reconstruct actual behavior
6. Extract business rules and invariants
7. Write as-is documents
8. Review gaps and unknowns
9. Expand into higher-level artifacts if needed
```

---

## 4. Step-by-Step Flow with BMAD Commands

## Step 0. Orient

### Goal
Hiểu BMAD nên dùng ở mức nào, và xác định task là **onboarding / reverse-documentation**, không phải greenfield design.

### Output
- onboarding scope ngắn
- level of BMAD cần dùng
- initial hypothesis về feature/module cần đào

### Suggested command
```text
bmad-help
```

### How to use
- nêu rõ đây là brownfield project
- nói mục tiêu là tạo tài liệu **as-is**
- yêu cầu workflow thiên về discovery hơn là implementation

### Prompt shape
```text
Tôi đang onboard vào một brownfield project.
Mục tiêu là reconstruct tài liệu as-is cho feature/module đã tồn tại,
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
- message/event definitions
- deployment manifests nếu cần

### Output
- repo map ngắn
- suspected entry points
- suspected hot modules
- glossary keyword ban đầu

### Suggested command
```text
bmad-quick-dev
```

### Why `bmad-quick-dev`?
Vì ở bước này cần tốc độ, discovery có định hướng, chưa cần full planning chain.

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

## Step 2. Choose a Feature or Module Slice

### Goal
Không onboard bằng cách hiểu “cả hệ thống” một lần.
Phải chọn một **slice** cụ thể:
- 1 feature
- hoặc 1 workflow
- hoặc 1 bounded module

### Good slice examples
- user registration
- membership renewal
- order confirmation
- payment callback handling
- notification dispatch

### Output
- chosen slice
- boundaries ban đầu
- success criteria cho tài liệu sẽ tạo

### Suggested command
```text
bmad-product-brief
```

### Why?
Vì ở brownfield, “brief” không phải để phát minh requirement mới, mà để chốt:
- vấn đề đang muốn hiểu
- scope hiểu đến đâu
- ai là actor
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

### Output
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

### Output
- happy path
- alternate path
- validation rules
- state changes
- side effects

### Suggested command
```text
bmad-create-prd
```

### Why use `bmad-create-prd` here?
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

### Output
- system shape text diagram
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

### Output
- readiness verdict
- gap list
- câu hỏi cần confirm với owner/domain expert

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
Tách understanding thành các document dễ dùng cho onboarding.

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

### Why use `bmad-create-story`?
Không phải để code ngay, mà để chia hiểu biết thành **workable unit** hoặc artifact unit:
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

## Step 8. Review Quality of the Documentation

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

### Expand from feature docs to:
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
- cần artifact có thể dùng cho onboarding thật

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
- feature chạm contract/schema/event
- rollback khó
- rất dễ hiểu sai domain rule
- tài liệu sẽ được dùng cho refactor hoặc redesign tiếp theo

---

## 6. Document Templates for Brownfield Onboarding

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

## 7. Recommended Prompts

## Prompt 1 — Start discovery

```text
Tôi đang onboard vào một brownfield project.
Hãy dùng BMAD theo hướng reverse-documentation.
Mục tiêu là hiểu và document feature `<feature-name>` theo trạng thái as-is.
Bắt đầu bằng cheap scan, pattern scan, rồi đề xuất vùng cần deep read.
Tách rõ fact / inference / unknown.
```

## Prompt 2 — Build functional understanding

```text
Từ evidence đã tìm được, hãy tạo tài liệu Functional Spec (As-Is) cho feature `<feature-name>`.
Không redesign.
Không ghi điều chưa có evidence như fact.
Bao gồm actors, trigger, happy path, alternate flows, validation rules, side effects, data changes.
```

## Prompt 3 — Build technical understanding

```text
Hãy tạo Technical Notes dạng as-is cho feature `<feature-name>`.
Mô tả text diagram, component interaction, source of truth, write/read path, sync-async dependencies, failure points, extension risks.
```

## Prompt 4 — Review documentation quality

```text
Hãy review bộ tài liệu brownfield onboarding cho feature `<feature-name>`.
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

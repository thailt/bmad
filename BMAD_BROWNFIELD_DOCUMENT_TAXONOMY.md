# BMAD Brownfield Document Taxonomy

## Mục tiêu

Tài liệu này chuẩn hóa **document taxonomy** cho brownfield project khi mục tiêu chính là:
- hiểu hệ thống đang tồn tại
- hiểu domain đang vận hành
- giảm tribal knowledge
- hỗ trợ onboarding, review, handoff, và refactor an toàn

Tài liệu này **không thiên về greenfield design**.
Nó thiên về **understanding artifacts**: biến hiểu biết về hệ thống/domain thành tài liệu có cấu trúc, search-friendly, và đủ tin cậy để người khác dựa vào.

---

## 1. First Principle

Trong brownfield, tài liệu tốt không phải là tài liệu dài nhất.
Tài liệu tốt là tài liệu giúp trả lời nhanh các câu hỏi sau:

- Feature này dùng để làm gì?
- Workflow thực tế chạy thế nào?
- Domain rule nào là bất biến?
- Dữ liệu nào là source of truth?
- Module nào sở hữu logic gì?
- Chỗ nào là integration boundary?
- Chỗ nào còn mơ hồ hoặc chưa được xác nhận?

Vì vậy taxonomy nên ưu tiên:
- **feature-first**
- **as-is before to-be**
- **domain and flow clarity**
- **fact / inference / unknown separation**
- **small reusable artifacts**

---

## 2. Taxonomy Overview

```text
A. Orientation & Scope
B. Feature Understanding
C. Domain Understanding
D. System / Module Understanding
E. Integration & Data Flow
F. Gaps / Risks / Validation
G. Knowledge Consolidation
```

---

## 3. Document Taxonomy

## A. Orientation & Scope

Các tài liệu nhóm này trả lời:
- đang tìm hiểu cái gì?
- phạm vi tới đâu?
- vì sao tài liệu này tồn tại?

### 1. Brownfield Brief

**Purpose**
- chốt scope của nỗ lực document
- xác định feature/module/domain nào đang được phân tích
- align mục tiêu onboarding/documentation

**Use when**
- mới bắt đầu một đợt document hóa
- muốn tránh đọc lan man cả hệ thống
- cần chốt in-scope / out-of-scope

**Typical sections**
- problem / purpose
- scope
- actors / stakeholders
- suspected boundaries
- expected output artifacts

**Primary BMAD command**
- `bmad-product-brief`

---

## B. Feature Understanding

Các tài liệu nhóm này trả lời:
- feature này làm gì?
- flow thực tế ra sao?
- business rules nào đang chi phối?

### 2. Feature Snapshot

**Purpose**
- cho cái nhìn ngắn, nhanh, đủ định hướng về một feature

**Use when**
- onboarding dev mới
- cần entry artifact trước khi đọc sâu
- cần inventory các feature quan trọng

**Typical sections**
- purpose
- actors
- entry points
- main components
- main flow
- data / contracts
- business rules
- risks / unknowns

**Primary BMAD command**
- `bmad-create-story` hoặc `bmad-create-prd`

### 3. Functional Spec (As-Is)

**Purpose**
- mô tả behavior thực tế của feature ở mức chức năng
- không redesign
- không mô tả “nên như thế nào”, mà mô tả “đang như thế nào”

**Use when**
- feature có rule/flow đáng kể
- cần handoff hoặc review
- sắp sửa feature nhưng cần hiểu chính xác hiện trạng

**Typical sections**
- goal
- actors
- trigger
- preconditions
- happy path
- alternate / error flows
- validation rules
- side effects
- data changes
- observability

**Primary BMAD command**
- `bmad-create-prd`

### 4. Workflow Note

**Purpose**
- tập trung vào một luồng nghiệp vụ cụ thể end-to-end
- hữu ích khi một feature trải dài qua nhiều module/service

**Use when**
- flow khó hiểu
- nhiều bước sync/async
- nhiều actor hoặc nhiều state transitions

**Typical sections**
- workflow goal
- trigger
- steps
- branching points
- failure / retry points
- resulting state changes

**Primary BMAD command**
- `bmad-create-prd`

---

## C. Domain Understanding

Các tài liệu nhóm này trả lời:
- domain concept là gì?
- thuật ngữ nào phải hiểu đúng?
- business invariant nào không được phá?

### 5. Domain Glossary

**Purpose**
- chuẩn hóa ubiquitous language
- giảm chuyện mỗi người gọi một kiểu

**Use when**
- domain có nhiều thuật ngữ
- team nói chuyện dễ lệch meaning
- feature names và DB/API names không đồng nhất

**Typical sections**
- term
- meaning
- equivalent technical term
- non-equivalent / avoid confusion
- related concepts

**Primary BMAD command**
- `bmad-create-story`

### 6. Domain Rules / Invariants Note

**Purpose**
- ghi lại các rule nghiệp vụ quan trọng và các invariant
- tách rule khỏi implementation detail

**Use when**
- domain phức tạp
- feature logic dễ sai nếu không hiểu đúng rule
- chuẩn bị refactor hoặc split module

**Typical sections**
- domain rule
- rationale / evidence
- enforced where
- exceptions
- unknowns

**Primary BMAD command**
- `bmad-create-prd`

### 7. Domain Concept Note

**Purpose**
- giải thích một khái niệm domain lớn: membership, order, entitlement, invoice, settlement...

**Use when**
- domain concept bị overload
- business và code mapping không rõ
- cần onboarding theo domain trước code

**Typical sections**
- concept definition
- lifecycle
- states
- relationships
- invariants
- boundaries

**Primary BMAD command**
- `bmad-product-brief` + `bmad-create-prd`

---

## D. System / Module Understanding

Các tài liệu nhóm này trả lời:
- module này sở hữu gì?
- chỗ nào là source of truth?
- cấu trúc hệ thống hiện tại ra sao?

### 8. Module Overview

**Purpose**
- mô tả trách nhiệm, boundaries, dependencies của một module

**Use when**
- hệ thống lớn
- nhiều package/service
- dev mới không biết nên đọc từ đâu

**Typical sections**
- module purpose
- responsibilities
- owned data / state
- main entry points
- dependencies
- key flows
- extension risks

**Primary BMAD command**
- `bmad-create-architecture`

### 9. System Overview (As-Is)

**Purpose**
- tạo bản đồ hệ thống ở mức cao
- giúp người mới có “system shape” trước khi đọc từng feature

**Use when**
- onboarding team member mới
- repo/hệ thống có nhiều module hoặc service
- chuẩn bị làm tài liệu tổng thể

**Typical sections**
- text diagram
- major modules / services
- data stores
- async flows
- external systems
- ownership summary

**Primary BMAD command**
- `bmad-create-architecture`

### 10. Technical Notes (Feature/Module As-Is)

**Purpose**
- mô tả technical shape thực tế cho feature hoặc module

**Use when**
- cần đi sâu hơn Feature Snapshot
- cần support modify / debug / refactor

**Typical sections**
- system shape
- source of truth
- write path
- read path
- sync/async dependencies
- failure modes
- performance notes
- extension risks

**Primary BMAD command**
- `bmad-create-architecture`

---

## E. Integration & Data Flow

Các tài liệu nhóm này trả lời:
- dữ liệu đi qua đâu?
- integration boundary nằm chỗ nào?
- event/contract nào quan trọng?

### 11. Integration Note

**Purpose**
- mô tả kết nối giữa module/service/external system

**Use when**
- nhiều external dependency
- contract mismatch gây lỗi thường xuyên
- cần hiểu sync vs async interaction

**Typical sections**
- integration purpose
- producer / consumer
- request-response hoặc event contract
- retry / error behavior
- ownership
- observability

**Primary BMAD command**
- `bmad-create-architecture`

### 12. Data Flow / State Transition Note

**Purpose**
- mô tả lifecycle dữ liệu hoặc state transition của một entity/domain concept

**Use when**
- logic stateful
- dễ sai ở lifecycle
- nhiều side effects theo state

**Typical sections**
- entity / concept
- states
- transitions
- triggers
- validation / invariants
- resulting side effects

**Primary BMAD command**
- `bmad-create-prd` + `bmad-create-architecture`

---

## F. Gaps / Risks / Validation

Các tài liệu nhóm này trả lời:
- điều gì đã chắc?
- điều gì chưa chắc?
- còn lỗ hổng nào trong understanding?

### 13. Open Questions and Gaps

**Purpose**
- giữ ambiguity visible thay vì che đi
- làm tài liệu đáng tin hơn

**Use when**
- brownfield understanding chưa hoàn chỉnh
- nhiều chỗ phải suy luận
- cần list câu hỏi cho owner/domain expert

**Typical sections**
- facts
- inferences
- assumptions
- unknowns
- need confirmation from
- suspected tech debt

**Primary BMAD command**
- `bmad-check-implementation-readiness`

### 14. Risk / Edge Case Note

**Purpose**
- capture edge cases, failure points, hidden assumptions

**Use when**
- feature dễ lỗi production
- flow có retry, duplicate, callback trễ, hoặc eventual consistency
- chuẩn bị sửa/refactor vùng nhạy cảm

**Typical sections**
- scenario
- trigger
- expected behavior
- current observed behavior
- risk
- mitigation / unknown

**Primary BMAD commands**
- `bmad-review-edge-case-hunter`
- `bmad-review-adversarial-general`

---

## G. Knowledge Consolidation

Các tài liệu nhóm này trả lời:
- sau khi document nhiều feature/module, tri thức nên được chưng cất ra sao?

### 15. Onboarding Guide

**Purpose**
- tạo đường đi cho người mới vào hệ thống

**Use when**
- đã có đủ artifact rời rạc
- muốn tạo reading path cho newcomer

**Typical sections**
- where to start
- recommended reading order
- critical concepts
- dangerous areas
- glossary references
- feature / module map

**Primary BMAD command**
- `bmad-create-story`

### 16. Lessons Learned / Retrospective

**Purpose**
- rút ra bài học từ quá trình document hóa / hiểu hệ thống

**Use when**
- đã hoàn tất một round onboarding/documentation
- muốn cải thiện process cho vòng sau

**Typical sections**
- what was hard to understand
- recurring ambiguity
- missing artifacts
- recommended improvements

**Primary BMAD command**
- `bmad-retrospective`

---

## 4. Command Table for Brownfield Documentation

| Document Type | Purpose | When to Use | Input Needed | Primary BMAD Command | Typical Output |
|---|---|---|---|---|---|
| Brownfield Brief | chốt scope của đợt document hóa | bắt đầu một effort mới | mục tiêu, phạm vi, feature/module nghi ngờ | `bmad-product-brief` | scope note, in/out scope, expected artifacts |
| Feature Snapshot | nhìn nhanh một feature | onboarding, inventory feature | tên feature, actors, entry points, rules sơ bộ | `bmad-create-story` hoặc `bmad-create-prd` | summary ngắn, flow chính, risks, unknowns |
| Functional Spec (As-Is) | mô tả behavior thực tế | cần hiểu feature để sửa hoặc handoff | feature understanding, flow, rules, side effects | `bmad-create-prd` | as-is functional artifact |
| Workflow Note | mô tả một luồng end-to-end | flow kéo qua nhiều module/service | trigger, steps, branching, state changes | `bmad-create-prd` | workflow artifact, alternate paths |
| Domain Glossary | chuẩn hóa ngôn ngữ domain | domain nhiều thuật ngữ, dễ gọi lệch | term list, business meaning, code names | `bmad-create-story` | glossary dùng cho onboarding |
| Domain Rules / Invariants Note | capture rule nghiệp vụ quan trọng | logic phức tạp, dễ hiểu sai | rules, evidence, exceptions | `bmad-create-prd` | rule list có rationale và unknowns |
| Domain Concept Note | giải thích một khái niệm domain lớn | concept bị overload hoặc mapping mơ hồ | concept definition, lifecycle, relations | `bmad-product-brief` + `bmad-create-prd` | concept note có lifecycle và boundary |
| Module Overview | giải thích trách nhiệm module | nhiều package/module, cần map ownership | module purpose, owned state, dependencies | `bmad-create-architecture` | module note, responsibilities, boundaries |
| System Overview (As-Is) | bản đồ hệ thống mức cao | onboarding hệ thống lớn | modules/services, DBs, integrations | `bmad-create-architecture` | text diagram + ownership summary |
| Technical Notes | technical shape thực tế của feature/module | cần support modify/debug/refactor | paths, source of truth, dependencies | `bmad-create-architecture` | write/read path, failure modes, risks |
| Integration Note | mô tả integration boundary | nhiều external/system dependencies | contract, producer/consumer, retry model | `bmad-create-architecture` | integration artifact, contracts, retry behavior |
| Data Flow / State Transition Note | mô tả lifecycle data/state | stateful logic, nhiều side effects | states, triggers, transitions | `bmad-create-prd` + `bmad-create-architecture` | state-machine note / lifecycle artifact |
| Open Questions and Gaps | giữ ambiguity visible | understanding chưa hoàn chỉnh | fact, inference, unknown list | `bmad-check-implementation-readiness` | gap log / confirmation list |
| Risk / Edge Case Note | soi blind spots và failure case | vùng nhạy cảm, lỗi prod, async complexity | known failures, retries, duplicate cases | `bmad-review-edge-case-hunter`, `bmad-review-adversarial-general` | risk log / edge-case artifact |
| Onboarding Guide | tạo reading path cho người mới | đã có nhiều artifact rời | feature/module map, glossary, priorities | `bmad-create-story` | onboarding sequence / reading guide |
| Lessons Learned / Retrospective | cải thiện process document hóa | sau một round documentation | notes về chỗ khó, missing artifacts | `bmad-retrospective` | lessons learned / process improvements |

---

## 5. Recommended Minimal Brownfield Document Set

Nếu mục tiêu là **thiên về hiểu hệ thống/domain**, bộ tối thiểu nên có là:

### Cho mỗi feature quan trọng
- Feature Snapshot
- Functional Spec (As-Is)
- Technical Notes
- Open Questions and Gaps

### Cho mỗi domain/module quan trọng
- Domain Glossary
- Domain Rules / Invariants Note
- Module Overview
- Integration Note

### Cho toàn hệ thống
- System Overview (As-Is)
- Onboarding Guide
- Lessons Learned / Retrospective

---

## 6. Suggested Reading / Authoring Order

### Nếu mới bắt đầu document một brownfield system

```text
1. Brownfield Brief
2. System Overview (As-Is)
3. Domain Glossary
4. Module Overview
5. Feature Snapshot
6. Functional Spec (As-Is)
7. Technical Notes
8. Open Questions and Gaps
9. Integration Note
10. Risk / Edge Case Note
11. Onboarding Guide
12. Retrospective
```

### Nếu đang đào một feature cụ thể

```text
1. Brownfield Brief
2. Feature Snapshot
3. Functional Spec (As-Is)
4. Technical Notes
5. Open Questions and Gaps
6. Risk / Edge Case Note
```

### Nếu đang muốn hiểu domain rõ hơn code

```text
1. Domain Glossary
2. Domain Concept Note
3. Domain Rules / Invariants Note
4. Workflow Note
5. Module Overview
6. Integration Note
```

---

## 7. Rule of Thumb

- Muốn **chốt scope** → `bmad-product-brief`
- Muốn **mô tả behavior / flow / rules** → `bmad-create-prd`
- Muốn **mô tả structure / boundary / ownership / integration** → `bmad-create-architecture`
- Muốn **biến kết quả thành artifact nhỏ, dễ dùng** → `bmad-create-story`
- Muốn **kiểm tra understanding đã đủ tin cậy chưa** → `bmad-check-implementation-readiness`
- Muốn **soi edge cases / blind spots** → `bmad-review-edge-case-hunter`, `bmad-review-adversarial-general`
- Muốn **rút kinh nghiệm và chuẩn hóa dần** → `bmad-retrospective`

---

## 8. One-Line Takeaways

- Brownfield documentation nên ưu tiên **understanding artifacts** hơn là design artifacts.
- Taxonomy tốt giúp biết **đang thiếu loại tài liệu nào**, không chỉ biết “cần viết doc”.
- Với brownfield, loại tài liệu quan trọng nhất thường là: **Feature Snapshot, Functional Spec (As-Is), Domain Rules, Module Overview, Technical Notes, Gaps Log**.
- BMAD commands nên được dùng như **workflow shapers** để ép hiểu biết thành structure rõ ràng.

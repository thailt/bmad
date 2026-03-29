# BMAD Decision Matrix

## Mục tiêu

Tài liệu này giúp quyết định **khi nào nên dùng BMAD**, nên dùng ở **mức nào**, và nên chạy **flow + command chain** nào cho từng loại công việc.

BMAD phù hợp nhất khi:
- bài toán có độ mơ hồ
- thay đổi chạm nhiều module
- có domain rule / contract / schema / event
- cần artifact để suy nghĩ, review, handoff, hoặc làm việc nhiều session

BMAD **không nên bị lạm dụng** cho các task rất nhỏ, rõ ràng, rollback dễ.

---

## 1. Decision Matrix

| Tình huống | Dấu hiệu | Mức dùng BMAD | Flow khuyên dùng | Command chain |
|---|---|---|---|---|
| **Tiny local task** | `< 2 files`, `< 1 giờ`, không đổi contract, rollback dễ | **Không cần BMAD** | Prompt trực tiếp | _Không cần command BMAD_ |
| **Small but slightly unclear** | 2–8 files, có chút mơ hồ, task local nhưng không trivial | **BMAD-lite / Quick Flow** | Clarify → Implement → Review | `bmad-help` → `bmad-quick-dev` → `bmad-code-review` |
| **Standard feature** | Có requirement rõ vừa phải, chạm vài module, cần thiết kế vừa đủ | **Standard BMAD** | Brief → PRD → Architecture → Story → Dev → Review | `bmad-help` → `bmad-product-brief` → `bmad-create-prd` → `bmad-create-architecture` → `bmad-create-story` → `bmad-dev-story` → `bmad-code-review` |
| **High-risk change** | Chạm contract/schema/event/domain rule, rollback khó, review cost cao | **Full BMAD** | Scope → Requirements → Design → Readiness → Stories → Sprint → Dev → Reviews → Retro | `bmad-help` → `bmad-create-prd` → `bmad-create-architecture` → `bmad-check-implementation-readiness` → `bmad-create-epics-and-stories` → `bmad-sprint-planning` → `bmad-create-story` → `bmad-dev-story` → `bmad-review-edge-case-hunter` → `bmad-review-adversarial-general` → `bmad-code-review` → `bmad-retrospective` |
| **Cross-module / multi-session initiative** | Nhiều module, cần nhiều lần quay lại, cần artifact để giữ context | **Full BMAD** | Full plan-driven flow | Dùng full chain như trên |
| **Incident / prod bug** | Cần fix nhanh, ưu tiên tốc độ hơn tài liệu | **BMAD tối giản sau khi chặn cháy** | Mitigate first → mini analysis → review → lessons learned | `bmad-help` → `bmad-quick-dev` → `bmad-code-review` → `bmad-retrospective` |

---

## 2. A 10-Second Heuristic

### Không cần BMAD
Dùng khi:
- `< 2 files`
- `< 1 giờ`
- không đổi contract
- rollback dễ

**Quyết định:** prompt trực tiếp, không cần quy trình BMAD.

### BMAD-lite / Quick Flow
Dùng khi:
- 2–8 files
- có chút mơ hồ
- local nhưng không còn trivial

**Quyết định:** chạy quick flow để giảm rework.

### Full BMAD
Dùng khi:
- nhiều file / nhiều module
- có domain rule / contract / schema / event
- review cost cao
- có khả năng phải làm nhiều session

**Quyết định:** dùng full BMAD để tạo artifact và giữ alignment.

---

## 3. Master Flow When BMAD Is Appropriate

Đây là flow tổng quát khi BMAD thực sự phù hợp:

1. Decide scope
2. Clarify problem
3. Capture requirements
4. Design solution
5. Validate readiness
6. Break into stories
7. Implement story-by-story
8. Review output
9. Capture lessons

### Suggested command chain

```text
bmad-help
→ bmad-product-brief
→ bmad-create-prd
→ bmad-create-architecture
→ bmad-check-implementation-readiness
→ bmad-create-epics-and-stories
→ bmad-sprint-planning
→ bmad-create-story
→ bmad-dev-story
→ bmad-code-review
→ bmad-retrospective
```

---

## 4. Recommended Flows by Situation

### 4.1 Fast and Safe
Use when you just want a little structure.

**Flow**
1. Clarify task quickly
2. Implement with lightweight guidance
3. Review result

**Commands**
```text
bmad-help
bmad-quick-dev
bmad-code-review
```

### 4.2 Standard Feature Flow
Use when the feature is real but not too risky.

**Flow**
1. Clarify problem and user value
2. Capture requirements
3. Sketch architecture
4. Create implementation unit
5. Build
6. Review

**Commands**
```text
bmad-help
bmad-product-brief
bmad-create-prd
bmad-create-architecture
bmad-create-story
bmad-dev-story
bmad-code-review
```

### 4.3 High-Risk Change Flow
Use when the change is hard to rollback or affects important boundaries.

**Flow**
1. Define requirements carefully
2. Create architecture explicitly
3. Validate readiness before coding
4. Split into epics/stories
5. Plan execution
6. Build step by step
7. Run adversarial and edge-case review
8. Run final code review
9. Capture lessons

**Commands**
```text
bmad-help
bmad-create-prd
bmad-create-architecture
bmad-check-implementation-readiness
bmad-create-epics-and-stories
bmad-sprint-planning
bmad-create-story
bmad-dev-story
bmad-review-edge-case-hunter
bmad-review-adversarial-general
bmad-code-review
bmad-retrospective
```

---

## 5. Step-by-Step Command Intent

| Bước | Mục đích | Command gợi ý |
|---|---|---|
| Clarify / orient | hiểu BMAD nên dùng mức nào | `bmad-help` |
| Brief the problem | chốt scope, business problem, success framing | `bmad-product-brief` |
| Capture requirements | biến ý tưởng thành requirement rõ | `bmad-create-prd` |
| Design solution | chốt kiến trúc, boundary, trade-off | `bmad-create-architecture` |
| Validate readiness | kiểm tra đã đủ rõ để implement chưa | `bmad-check-implementation-readiness` |
| Break into implementation units | chia epics, stories, work units | `bmad-create-epics-and-stories` |
| Plan execution | lên thứ tự làm việc / sprint | `bmad-sprint-planning` |
| Pick next slice | chọn story cụ thể để làm | `bmad-create-story` |
| Implement | code theo story | `bmad-dev-story` |
| Stress review | soi edge cases / adversarial cases | `bmad-review-edge-case-hunter`, `bmad-review-adversarial-general` |
| Final review | review tổng hợp | `bmad-code-review` |
| Learn | capture lesson, cải tiến process | `bmad-retrospective` |

---

## 6. Personal Recommendation Template

### Small + clear + local
**Recommendation:** prompt normally

### Some ambiguity + moderate risk
**Recommendation:** `bmad-quick-dev`

### Cross-module + domain-heavy + hard to rollback
**Recommendation:** full BMAD

---

## 7. Notes

- BMAD có giá trị nhất khi **process giúp giảm rework**.
- Không nên ép full BMAD vào task quá nhỏ.
- Với incident mitigation, **speed wins first**; cấu trúc đến ngay sau khi chặn cháy.
- Artifact có giá trị nhất khi quyết định cần sống qua nhiều session hoặc nhiều người.
- Nếu chưa chắc, bắt đầu bằng:

```text
bmad-help
```

hoặc

```text
bmad-quick-dev
```

---

## 8. Short Rule of Thumb

- **Tiny + obvious** → no BMAD
- **Medium + somewhat unclear** → BMAD-lite
- **Big + cross-boundary + expensive to review** → full BMAD

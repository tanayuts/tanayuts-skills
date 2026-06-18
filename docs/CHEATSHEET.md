# 💡 Claude Skills Cheat Sheet | คู่มือสรุปคำสั่งลัด

Quick-reference cards for invoking and controlling your 9 modular Claude skills.
คู่มือสรุปคำสั่งและคีย์เวิร์ดสำหรับเรียกใช้ 9 สกิลของ Claude เพื่อความสะดวกรวดเร็วในการพัฒนาซอฟต์แวร์

---

## 🚀 Installation & Toggle | การติดตั้งและการสลับโหมด

* **Install Plugin / ติดตั้งปลั๊กอิน:**
  ```bash
  claude plugin install github:tanayuts/tanayuts
  ```
* **Ponytail Level / ปรับระดับความประหยัด (YAGNI):**
  * `ponytail lite` — *Suggests alternatives / เสนอทางเลือกรวมถึง stdlib*
  * `ponytail full` — *(Default) Enforces YAGNI / เน้นสั้นที่สุด ตัดโค้ดส่วนเกิน*
  * `ponytail ultra` — *Max YAGNI, challenges requirements / ตัดขาดทุกอย่าง ลบก่อนเพิ่มโค้ด*
  * `stop ponytail` / `normal mode` — *Revert to normal Claude / กลับสู่โหมดปกติของ Claude*

---

## 🇬🇧 English Cheat Sheet

### 1. Coding Philosophy (`coding-philosophy`)
* **Trigger:** Auto-activated on any code request.
* **Commands:**
  * `ponytail-review` — Scan current diff/code for over-engineering.
  * `ponytail-audit` — Scan the entire repository for unused code/dependencies.
  * `ponytail-debt` — List all technical debt (`// ponytail:` comments) with ceilings and upgrade triggers.
* **Example:** *"ponytail-review this code"* or *"ponytail ultra"*

### 2. Debugging & Review (`debugging-review`)
* **Trigger Words:** `debug`, `error`, `crash`, `scrutinize`, `/post-mortem`
* **Commands:**
  * `debug` / `error` — Verbatim recital of 4-step debug mantra. Proposes fixes only *after* repro exists.
  * `scrutinize` — End-to-end trace checking paths, edge cases, and test validity.
  * `/post-mortem` — Create RCA document. Needs 4 checked inputs (repro, root cause, fix, validation).
* **Example:** *"debug the timeout error"* or *"scrutinize this PR"*

### 3. Incident Response (`incident-response`)
* **Trigger Words:** `prod is down`, `incident`, `sev 1`, `outage`, `rollback`
* **Workflow:** Triage -> Mitigate (rollback/disable flag/restart) -> Communicate (Slack/Stakeholder/Status page) -> UTC Timeline.
* **Example:** *"prod is down! orders table is locked since 5 mins ago"*

### 4. Architecture & Estimation (`architecture-estimation`)
* **Trigger Words:** `design`, `architect`, `estimate`, `t-shirt size`
* **Workflow:** Proposes simple architectures with tradeoff tables (max 3 options). Sizes tasks using XS, S, M, L, XL.
* **Example:** *"design the data flow for file uploads"* or *"estimate the effort to migrate to oauth"*

### 5. Refactoring & Performance (`refactoring-performance`)
* **Trigger Words:** `refactor`, `optimize`, `profile`, `it's slow`, `latency`
* **Workflow:** Safe step-by-step refactoring (runs tests first, small changes, commit per step). Profiles bottleneck before optimizing.
* **Example:** *"optimize this query, it takes 3 seconds"* or *"refactor database connections"*

### 6. Security & Testing (`security-testing`)
* **Trigger Words:** `security review`, `security audit`, `test plan`, `vulnerability check`
* **Workflow:** Maps trust boundaries and runs OWASP checklists. Formulates unit, integration, and E2E test plan pyramid.
* **Example:** *"do a security review of login endpoint"* or *"write a test plan for refunds"*

### 7. Teaching & Writing (`teaching-writing`)
* **Trigger Words:** `teach me`, `socratic`, `ADR`, `runbook`, `README`
* **Workflow:** Socratic mode guides you via questions. Generates strict docs (ADRs, Runbooks with commands, Changelogs).
* **Example:** *"teach me how Docker handles networking"* or *"write an ADR for switching to Postgres"*

### 8. Template Spec (`template-spec`)
* **Trigger Words:** `create a project spec`, `system directive`, `implementation guide`
* **Workflow:** Asks 7 questionnaire questions first, then writes comprehensive system spec for AI coding agents.
* **Example:** *"create a project spec for a task planner app"*

### 9. Communication (`communication-selfregulation`)
* **Trigger Words:** `write for management`, `slack update`, `standup note`, `status update`
* **Workflow:** Translates deep technical jargon into business value metrics for Slack (short), JIRA, Email, or Meetings.
* **Example:** *"write a slack update for the PM explaining why we rolled back"*

---

## 🇹🇭 คู่มือสรุปภาษาไทย (Thai Cheat Sheet)

### 1. ปรัชญาการเขียนโค้ด (`coding-philosophy`)
* **การเรียกใช้:** เปิดใช้งานโดยอัตโนมัติเมื่อสั่งให้เขียนโค้ด (เน้นกฎ YAGNI และประหยัดสุดขีด)
* **คำสั่งเฉพาะ:**
  * `ponytail-review` — ตรวจสอบส่วนต่างโค้ด (diff) เพื่อหาการดีไซน์เกินจำเป็น (over-engineering)
  * `ponytail-audit` — ตรวจสอบทั้งโปรเจกต์เพื่อหาไฟล์, โค้ด หรือ dependency ที่ไม่ได้ใช้
  * `ponytail-debt` — ดึงรายการหนี้ทางเทคนิค (`// ponytail:`) มาสรุปพร้อมเกณฑ์อัปเกรด
* **ตัวอย่าง:** *"ช่วยทำ ponytail-review ให้โค้ดไฟล์นี้หน่อย"* หรือ *"เปลี่ยนเป็นระดับ ponytail ultra"*

### 2. การค้นหาข้อผิดพลาดและรีวิวโค้ด (`debugging-review`)
* **คำหลักที่ใช้เรียก:** `debug`, `error`, `แก้บั๊ก`, `ตรวสอบโค้ดอย่างละเอียด` (`scrutinize`), `/post-mortem`
* **คำสั่งเฉพาะ:**
  * `debug` / `error` — ท่องกฎเหล็ก 4 ข้อ (Reproduce -> Fail Path -> Falsify -> Breadcrumbs) บังคับหาสาเหตุก่อนแก้โค้ด
  * `scrutinize` — ตรวจความถูกต้องของโค้ดแบบ end-to-end วิเคราะห์อินพุตที่อาจทำให้พัง และความถูกต้องของเทส
  * `/post-mortem` — เขียนรายงาน RCA หลังแก้บั๊กสำเร็จ (ต้องระบุ repro, สาเหตุ, วิธีแก้ และการตรวจสอบ)
* **ตัวอย่าง:** *"debug บั๊กข้อมูลไม่บันทึกลง DB"* หรือ *"ทำ scrutinize โค้ดของ PR นี้ที"*

### 3. จัดการระบบล่มในโปรดักชัน (`incident-response`)
* **คำหลักที่ใช้เรียก:** `ระบบล่ม`, `prod ล่ม`, `incident`, `sev 1`, `outage`, `rollback`
* **ขั้นตอนทำงาน:** Triage (วิเคราะห์ใน 60 วินาที) -> Mitigate (กู้ระบบด่วนที่สุดด้วยการ Rollback/ปิดธง/Restart) -> Communicate (ร่างคำแถลง Slack/หัวหน้า) -> Log Timeline (เวลา UTC)
* **ตัวอย่าง:** *"prod ล่ม! ลูกค้าชำระเงินไม่ได้ ได้รับโค้ด 500 ถี่มาก"*

### 4. ออกแบบระบบและประเมินเวลา (`architecture-estimation`)
* **คำหลักที่ใช้เรียก:** `ออกแบบระบบ`, `สถาปัตยกรรม`, `ประเมินเวลา`, `t-shirt size`
* **ขั้นตอนทำงาน:** เสนอการออกแบบที่ง่ายที่สุดพร้อมตารางเปรียบเทียบข้อดีข้อเสีย (Tradeoffs) ไม่เกิน 3 ตัวเลือก และประเมินงานเป็นขนาดเสื้อ (XS, S, M, L, XL) พร้อมระบุข้อสมมติฐาน
* **ตัวอย่าง:** *"ออกแบบสถาปัตยกรรมสำหรับระบบจองคิว"* หรือ *"ช่วยประเมินเวลา (t-shirt size) งานย้าย database"*

### 5. ปรับปรุงโค้ดและประสิทธิภาพ (`refactoring-performance`)
* **คำหลักที่ใช้เรียก:** `refactor`, `ปรับปรุงโค้ด`, `ช้ามาก`, `optimize`, `profile`
* **ขั้นตอนทำงาน:**
  * **Refactor:** เขียนเทสครอบคลุม API ก่อนแก้ไข ➡️ แก้โค้ดทีละสเต็ปเล็ก ๆ ➡️ รันเทส ➡️ commit ทีละสเต็ป
  * **Performance:** กำหนดเป้าหมาย (เช่น p95 < 200ms) ➡️ รันโปรไฟเลอร์หาคอขวด ➡️ แก้ไขจุดเดียว ➡️ วัดผลซ้ำ
* **ตัวอย่าง:** *"refactor โค้ดโมดูลส่งอีเมลให้สะอาดขึ้น"* หรือ *"optimize API ค้นหาสินค้า ช้ามาก"*

### 6. ตรวจสอบความปลอดภัยและเทสแพลน (`security-testing`)
* **คำหลักที่ใช้เรียก:** `ตรวจสอบความปลอดภัย`, `security review`, `เขียน test plan`
* **ขั้นตอนทำงาน:** ระบุขอบเขตความน่าเชื่อถือ (Trust boundaries) ไล่เช็คช่องโหว่ OWASP (Injection, Auth, CORS, secrets) และวางแผนเทสแบบพีระมิด (Unit, Integration, E2E)
* **ตัวอย่าง:** *"ตรวจความปลอดภัยช่องโหว่ SQL Injection ในหน้านี้ที"* หรือ *"เขียน test plan สำหรับฟังก์ชันการคืนเงิน"*

### 7. โหมดสอนและงานเขียนเอกสาร (`teaching-writing`)
* **คำหลักที่ใช้เรียก:** `สอนหน่อย`, `socratic`, `ADR`, `runbook`, `README`
* **ขั้นตอนทำงาน:**
  * **Socratic:** โหมดจับคู่สอนโดยใช้คำถามนำทางทีละคำถาม เพื่อให้ผู้ใช้คิดหาทางออกเอง (พิมพ์ `semi-socratic` เพื่อขอคำอธิบายสั้น ๆ พร้อมคำถามทดสอบความเข้าใจ)
  * **Templates:** สร้าง ADR (บันทึกการตัดสินใจ), Runbook (คู่มือกู้ระบบเมื่อแจ้งเตือนดัง มีคำสั่งพร้อมก๊อป), README, Changelog
* **ตัวอย่าง:** *"สอนเรื่อง async/await ใน js หน่อย"* หรือ *"เขียน ADR สำหรับการเลือกใช้ JWT"*

### 8. ร่างสเปกโครงการสำหรับเอเจนต์ (`template-spec`)
* **คำหลักที่ใช้เรียก:** `เขียน project spec`, `สร้างแบบร่างโครงการ`
* **ขั้นตอนทำงาน:** Claude จะถามคำถามคีย์เวิร์ด 7 ข้อก่อน เพื่อนำข้อมูลไปเจาะจงโครงร่างไฟล์สเปกสำหรับ AI Agent ตัวอื่นให้ไปนำไปเขียนโค้ดต่อได้ทันที
* **ตัวอย่าง:** *"เขียน project spec สำหรับทำแอปคำนวณแคลอรี"*

### 9. สรุปความคืบหน้าแจ้งผู้บริหาร (`communication-selfregulation`)
* **คำหลักที่ใช้เรียก:** `เขียนสรุปให้หัวหน้า`, `อัปเดต slack`, `เขียนรายงาน standup`
* **ขั้นตอนทำงาน:** แปลงรายละเอียดโค้ดยาก ๆ ให้กลายเป็นข้อความสรุปทางธุรกิจ ไม่ใส่ชื่อฟังก์ชันและ SHA บอร์ดลงรายละเอียดช่องทาง Slack (สั้นกระชับ), JIRA (ละเอียดระบุกระทบและเจ้าของ), อีเมล หรือหัวข้อเข้าประชุม
* **ตัวอย่าง:** *"สรุปรายงานการแก้ไขบั๊กตัวนี้เพื่อนำไปอัปเดตลง Slack ของทีม"*

---

## 🔄 Workflow Pipelines | ลำดับการสั่งงานต่อเนื่อง

```
🔥 เมื่อเกิดเหตุระบบล่ม (Outage Lifecycle):
[Incident Response (กู้ระบบ)] ➡️ [Debug Mantra (หาบั๊ก)] ➡️ [Post-Mortem (บันทึก RCA)] ➡️ [Management Talk (สรุปแจ้งผู้ใหญ่)]

🌱 เมื่อเริ่มต้นสร้างระบบใหม่ (New Project Lifecycle):
[Template Spec (สเปกโครงการ)] ➡️ [Architecture & Estimation (ออกแบบ & ประเมิน)] ➡️ [Security & Testing (เทสแพลน)] ➡️ [Coding (ลุยเขียนโค้ด)]
```

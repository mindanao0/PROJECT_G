# 07 — ส่งต่อโมเดลอื่นโดยไม่เริ่มใหม่

**สถานะย่อ 2026-10-14:** PROJECT_G คือ AFTERIMAGE เกม VN ไซไฟลึกลับ มีแต่เอกสาร รอบล่าสุดขอแผนและคำถามให้ครบ ยังไม่อนุญาต code; repo ต้นทางมี brief เดียว ตอนนี้จัดชุดแผนและแบบทดลอง S0 ที่ยังไม่ใช่ canon

## 1. สิ่งที่ห้ามหลุดจาก context

- เจ้าของเลือก: ตัวเอกคนธรรมดา, โลกอนาคต, ปริศนา/จิตวิทยา/ความสัมพันธ์/สยอง/อาชญากรรม/การเติบโต
- ต้องมี branching ที่เปลี่ยนเหตุการณ์จริง ข้อมูลต่างคนต่างรู้ ความเชื่อใจ/ทรยศ ทีม ค่าความสัมพันธ์/สถานะ/Level และการนำเสนอแบบ VN
- 12 บท × 1 ชั่วโมงยังเป็นประมาณการ ไม่ใช่ requirement ตายตัว
- ยังไม่มีคำตอบใหม่ของ 28 คำถาม และยังไม่ยืนยันพล็อต กฎโลก engine/platform หรืองบ
- S0 ที่ลงรายละเอียดแล้วเป็นข้อเสนอ: 12 nodes / 22 traces / 4 choices / 3 checkpoints; แก้ได้ตามคำตอบ
- ไม่มี game implementation, assets, script เต็ม, playtest หรือ CI ที่ผ่านแล้ว; อย่ารายงานเกินนี้
- อย่านำ local-only/GPU/Linux/ข้อห้ามจากโครงการอื่นมาใช้เป็นคำสั่งของ PROJECT_G โดยอัตโนมัติ

## 2. Minimal read map

อ่าน `AGENTS.md` + `00_PROJECT_STATUS.md` + ไฟล์นี้ก่อน แล้วเลือกเฉพาะงาน ไม่ต้องอ่าน brief 266 บรรทัดหรือเอกสารทั้งหมดซ้ำ

| งานที่ได้รับ | อ่านเพิ่ม | แก้ที่ไหนเป็นหลัก |
|---|---|---|
| รับคำตอบ/ถามต่อ P01 | 02 เฉพาะข้อที่เกี่ยว + 01 gate | 02 answer log → 00 decision register |
| premise/rules/truth P02 | 03 + D01–D04 | 03; ซิงก์ 04/06 ถ้ากระทบ |
| character/knowledge P03 | 03 + Q03/Q04/Q07/Q21 | 03; ซิงก์ scene dialogue constraints |
| graph/state P05/P06 | 04 + 03 clues + 06 §3 | 04; ซิงก์ 05 counts/06 tests |
| ending P04 | 03 §7 + full graph ที่มีในขณะนั้น | 03 + 06 coverage |
| slice script P07 | 03/04 ที่อนุมัติ + 02 Q06 | script เฉพาะ slice และ references ใน 04; ยังไม่ทำในรอบวางแผนแรก |
| UX/engine/estimate P08/P09 | 05 + Q09–Q24 เฉพาะข้อเกี่ยว | 05 พร้อมหลักฐาน/ข้อจำกัด |
| implementation ในอนาคต | คำสั่งใหม่ + 01 task + 04/05/06 | code ที่เลือกหลัง G2/G3; ไม่คิดว่าต้องสร้างตาม doc ทั้งหมดครั้งเดียว |
| review-only | diff + acceptance ของ task | รายงานข้อผิดเฉพาะจุดและไฟล์ที่ต้องแก้ |

## 3. ข้อมูลหนึ่งเรื่องมีเจ้าของไฟล์เดียว

| ข้อมูล | Source of truth | ไฟล์อื่นทำอะไรได้ |
|---|---|---|
| ข้อกำหนด/สถานะ decision | 00 | อ้าง ID ไม่คัดลอกเป็น requirement ชุดใหม่ |
| คำตอบผู้ใช้ | 02 answer log | สรุปเท่าที่มีหลักฐาน; ไม่แก้ประวัติคำตอบเดิมให้ตรงร่าง |
| dependency/task/gate | 01 | อ้าง task ID และผลจริง |
| ความจริง/กฎ/ตัวละคร/clues/ending families | 03 | ใช้เป็น premise สำหรับ scene ไม่คิดความจริงใหม่ใน script เงียบ ๆ |
| scene graph/choice/state S0 | 04 | ใช้ count/conditions เดียวกัน |
| asset/tech/estimate | 05 | ปรับเมื่อ scene หรือ platform เปลี่ยน |
| acceptance/coverage/risk | 06 | เพิ่มกรณีเมื่อเจอ failure จริง |
| ประวัติเดิม | reference/brief v0.1 | ใช้ตรวจเจตนา ไม่กลับไปใช้ขั้นตอนเก่าเป็นแผนปัจจุบัน |

## 4. ขั้นทำงานหนึ่ง session

1. ตรวจ branch/head/PR ล่าสุด เทียบกับงานของคนอื่นก่อนแก้; อย่าอาศัยเลข commit ใน handoff ว่ายังล่าสุด
2. เลือก task ID หนึ่งงานหรือชุดที่ต่อเนื่องกัน อ่านไฟล์ตามตาราง
3. ถ้ามีคำตอบใหม่ บันทึกใน 02 พร้อมวันที่/คำสั่ง แล้วซิงก์ decision และส่วนที่กระทบ
4. ถ้าไม่มีคำตอบ ทำเฉพาะงานที่เดินต่อได้ใน 01 §6 ติดป้าย PROPOSED ไม่สร้างคำตอบแทน
5. ส่ง artifact ของ task จริงพร้อม acceptance: เช่นปรับ rule ledger/edge table ไม่จบแค่เสนอว่าจะปรับ
6. ตรวจ diff และจุดเสี่ยงที่ task เปลี่ยน บันทึกสิ่งที่ไม่ได้ทดสอบ แยกเอกสาร/runtime/playtest
7. ปิดด้วย task เสร็จจริง, decision ค้าง, หลักฐานตรวจ, next task และคำสั่งที่ต้องมีถ้าจะข้ามจาก docs ไป code

งานไม่ต้องกลับไปหาโมเดลใหญ่ทุกขั้น ใช้โมเดลทั่วไปแก้ตาม contract ที่ชัดแล้ว ใช้การ review เชิงลึกเมื่อเปลี่ยนแกนปริศนา กฎเวลา/ความจำ หรือ full ending graph เท่านั้น ไม่ต้องระบุชื่อโมเดลเป็น dependency ของ project

## 5. Prompt พร้อมคัดลอก — ทำเอกสารต่อ

> ทำงานต่อใน mindanao0/PROJECT_G แบบเอกสารเท่านั้น อ่าน AGENTS.md, docs/00_PROJECT_STATUS.md และ docs/07_HANDOFF.md ก่อน จากนั้นดู task ที่พร้อมใน docs/01_MASTER_PLAN.md ใช้คำตอบของฉันอัปเดต docs/02_OPEN_QUESTIONS.md และ decision register อย่าถามสิ่งที่ยืนยันแล้วซ้ำ อย่าถือ S0 เป็น canon โดยไม่มีคำตอบหรือคำสั่งมอบหมายให้เลือก ถ้ายังไม่มีคำตอบให้ตรวจ/ปรับร่างแบบมีเงื่อนไขและระบุสิ่งที่ยังเปิดอยู่ ห้ามสร้างเกม โค้ด dependencies หรือ assets จริง สรุปงานที่เสร็จพร้อม next task ID เมื่อจบ

## 6. Prompt พร้อมคัดลอก — ตรวจแผนเฉพาะจุด

> ตรวจ task [ใส่ ID] ใน PROJECT_G โดยอ่านไฟล์ตาม handoff ตรวจความขัดแย้งของ requirement/decision, cause-and-effect, clue provenance, character knowledge, branch/state, ending eligibility และ scope เท่าที่ task กระทบ เสนอหรือแก้จุดที่มีหลักฐานจริง ห้ามขยายเป็นพล็อตใหม่หรือถามคำถามทั้งหมดซ้ำ แยก bug ของแผนออกจากความชอบที่ยังรอเจ้าของเลือก รอบนี้ยัง docs only

## 7. การรับคำตอบแบบสั้น

ถ้าเจ้าของตอบ `Q01=A Q02=A Q03=C` ให้เปลี่ยนเฉพาะสามข้อแล้วไล่ impact ใน 02/00 ถ้าบอก “ตามคำแนะนำทั้งหมด” ให้บันทึกว่ารับตัวเลือกแนะนำที่ตัดสินได้ พร้อมคง Q17/Q18 เป็น UNKNOWN ถ้าไม่มีข้อมูลจริง และถามเฉพาะส่วนที่เหลือเมื่อจำเป็นต่อ task

อย่าถือคำว่า “ตามแผน” หรือ “merge เอกสาร” ว่าอนุญาต code เมื่อ scope ล่าสุดยัง docs only แต่หากเจ้าของสั่งชัดว่า “ทำ P10/เริ่ม implementation” แล้ว ให้ใช้คำสั่งใหม่และเดินงานที่อนุญาตโดยไม่ขอซ้ำ

## 8. Checkpoint ปัจจุบันสำหรับผู้รับช่วง

| ช่อง | ค่า |
|---|---|
| Source audit base | `62ed09c5fc88ea528ce7531e65d69a55a144fd1d` |
| Current package | แผนรอบ 2026-10-14; อ่าน commit/PR ล่าสุดจาก GitHub เมื่อเริ่ม session |
| Completed | P00; draft specification/คำถาม/แผน/acceptance/handoff |
| Next | P01 รับคำตอบ → P02/P03/P04 ตาม dependency |
| Pending owner decisions | Q01–Q28; เริ่ม core Q01–Q06/Q08 ก่อน |
| Allowed without more answers | document consistency review, conditional design, scope comparison |
| Not yet authorized | code/engine setup/full script/final assets/release |
| Evidence status | ดู 06 §8; runtime และ playtest ยัง NOT RUN |

ถ้าต้องทำ handoff ครั้งหน้า ให้อัปเดตตารางนี้และ task status ที่เป็นเจ้าของข้อมูล แทนการคัดลอกเรื่องทั้งหมดเป็นไฟล์ context ใหม่

# 06 — แผนตรวจรับและความเสี่ยง

**เอกสารนี้กำหนดสิ่งที่จะตรวจ ไม่ได้อ้างว่ามีเกมผ่านแล้ว** รอบปัจจุบันทำได้เฉพาะ document review / ตรวจแบบจำลองเงื่อนไข ส่วน playtest, engine test และ runtime QA ยังไม่เริ่ม

## 1. Traceability: ความต้องการต้องมีหลักฐานอย่างไร

| Requirement | สิ่งออกแบบที่ตอบโจทย์ | หลักฐานรับงานในอนาคต |
|---|---|---|
| R01–R04 ทิศทาง/ตัวเอก/โลก/แนว | 03 premise, M01–M08, cast และ S01 ชีวิตปกติ | เจ้าของยืนยัน G1; continuity review ไม่มีผู้ถูกเลือก/กฎวิเศษเพิ่ม |
| R05 อารมณ์ | choice trade-offs และ payoff S08 | คำอธิบายของผู้เล่นว่าใครเสียอะไร เพราะการเลือกใด; ไม่ใช้ rating อย่างเดียว |
| R06 branching จริง | S03A ≠ S03B, E02 ≠ E03 และ exposure หลัง merge | เล่นสอง route พบคนละเหตุการณ์/หลักฐาน และ S08 ยังแตกต่าง |
| R07 knowledge/trust | 03 knowledge map, CH02/S05A/S05B | ตัวละครอ้างข้อมูลที่มีสิทธิ์รู้; ไม่พูดถึงเสียงเมื่อ withheld |
| R08 team/survival | lead → ally → วิธีช่วยฝากหลักฐาน | ally มีผลต่อ action ไม่ใช่ portrait เปลี่ยน; full game ต้องเพิ่ม survival payoff ที่ยืนยันภายหลัง |
| R09 VN presentation | 05 UI01–UI06 | อ่าน เลือก journal และ scene assets ทำงานบน target จริง |
| R10 relation/state/Level | collaboration, custody, investigation_level | Level เปิด seal ด้วยตนเอง; relationship เปิดทางช่วยชดเชย; outcome ต่าง |
| R11 ขนาดยังไม่สรุป | P16/P17 + capacity model | ใช้จำนวนงานจริง/เวลาเล่นจริงก่อนเลือกจำนวนบท |
| R12 docs only | diff ของชุดนี้ | มีเฉพาะ Markdown และย้ายต้นฉบับเดิม ไม่มี source/assets/dependencies |

## 2. Narrative review ก่อนล็อกเรื่อง (P02/P03/P04)

| Check | ถามอะไร / วิธีตรวจ | ไม่ผ่านเมื่อ |
|---|---|---|
| N01 ความจริงคงที่ | ไล่ timeline T01–T07 เทียบทุก route | ต้องเปลี่ยนผู้กระทำผิด/อดีตเพื่อให้ route ทำงาน |
| N02 กฎเทคโนโลยี | ทุกการแทรกแซงระบุขอบเขต สิทธิ์ เวลา คน และร่องรอย | มี remote erase ทั้งเมืองหรือ restore ทุกอย่างโดยไม่มี setup |
| N03 ความเป็นอิสระของหลักฐาน | วาด provenance ของข้อสรุปสำคัญ | นับสำเนาจากคนเดียวกันเป็นสองแหล่งยืนยัน |
| N04 Fair clues | ก่อน irreversible choice ผู้เล่นได้ข้อมูลเจตนา/ความเสี่ยงที่ตัวเอกรู้ | ผลเลวเกิดเพราะข้อความ choice หลอกการกระทำจริง |
| N05 Character agency | ระบุเหตุผลช่วย/โกหก/เปลี่ยนฝ่ายกับหลักฐานก่อนหน้า | “Trust < x จึงทรยศ” เป็นเหตุผลเดียว หรือพยานไม่มีสิทธิ์ปฏิเสธ |
| N06 Scope of knowledge | ทุกบทพูดสำคัญมีแหล่งรู้และเวลาที่รู้ | คนหนึ่งรู้จาก route อื่น/อนาคต/author bible |
| N07 Endings | ตรวจ prerequisites, alternative clue path และ END03 fallback | ต้องรักคนใดคนหนึ่ง/เล่นทุก route โดยไม่แจ้ง หรือมี ending ที่ไปไม่ถึง |
| N08 Emotional pacing | แต่ละช่วงมีชีวิตปกติ ความสัมพันธ์ เหตุค้นพบ และผลตามมา | ทุกฉากเป็น exposition/หักมุม/สูญเสียจนความผูกพันไม่เกิด |

ความกำกวมที่ตั้งใจต้องระบุว่า “ผู้เล่นตีความคุณค่าหรือแรงจูงใจส่วนไหน” ไม่ใช้เป็นข้อยกเว้นให้ข้อเท็จจริงคดีขัดกัน

## 3. Exhaustive slice coverage (P05/P06 และ P13 ในอนาคต)

ให้ตรวจ 8 input classes และ 22 terminal traces จาก 04 **ทั้งหมด** ใน slice ขนาดนี้ ไม่ใช้ random play แทน structural coverage

| กลุ่ม | lead | receipt_checked | disclosure | ผลที่ถูกต้อง | จำนวน trace |
|---|---|---|---|---|---|
| TR01 | archive | false | withheld | public / private | 2 |
| TR02 | archive | false | shared | public / sealed / private | 3 |
| TR03 | archive | true | withheld | public / sealed / private | 3 |
| TR04 | archive | true | shared | public / sealed / private | 3 |
| TR05 | witness | false | withheld | public / private | 2 |
| TR06 | witness | false | shared | public / sealed / private | 3 |
| TR07 | witness | true | withheld | public / sealed / private | 3 |
| TR08 | witness | true | shared | public / sealed / private | 3 |

Expected structural coverage: **12 scene nodes, 15 scene-to-scene edges, 4 choice IDs, 9 choice alternatives, 22 feasible terminal traces** CH00 มีสอง alternative ไป S02 เดียวกัน จึงนับเป็นหนึ่ง scene edge

Terminal evidence: ทุก trace มี E01/E04 + E02 เมื่อ archive หรือ E03 เมื่อ witness เท่านั้น ต้นทาง audit หมุนทิ้งไม่ลบ evidence ที่พบแล้ว

### Test scenarios ที่มีความหมาย

| ID | Given / เมื่อทำ | Expected |
|---|---|---|
| ST01 | TR01 หรือ TR05 ไป S06 | seal ไม่พร้อมพร้อมเหตุผล; publish/private ไปต่อได้ ไม่มี softlock |
| ST02 | TR02/TR06 ไม่มี Level แต่ shared | seal ทำได้ด้วย ally; ไม่เพิ่ม Level อัตโนมัติ |
| ST03 | TR03/TR07 มี Level แต่ withheld | seal ทำเองได้; ally ไม่รู้เสียงจากไหน |
| ST04 | TR04/TR08 มีทั้งสอง | เลือกวิธีตนเอง+ally ตรวจตาม contract; ไม่ใช้ RNG |
| ST05 | เปรียบเทียบ archive กับ witness โดย choice อื่นเหมือนกัน | พบ scene/evidence/แหล่ง exposure ต่างกันถึง S08 |
| ST06 | เล่น private | ไปถึง S08, journal ยังครบ; hook สืบ E05 ต่อ ไม่จบเกมหรือค้าง |
| ST07 | ดู evidence ซ้ำ/เปิด journal/กลับ scene | ไม่เพิ่ม Level/trust/choice effects และไม่เลื่อน event clock |
| ST08 | muted/ใช้ transcript/text instant | ได้ข้อมูลเดียวกับเปิดเสียง ไม่พลาด clue ที่จำเป็น |
| ST09 | กดซ้ำเร็วที่ choice / กลับจาก menu | commit choice ครั้งเดียว ไม่ข้ามสอง scene หรือรับ E02+E03 |
| ST10 | save ก่อน CH01, ไป archive, load แล้วเลือก witness | E02 และข้อมูล C01 จากอนาคตหายไป; ได้ E03/C02 ถูกต้อง |
| ST11 | save ก่อน CH02, share, load กลับแล้ว hold | cooperation และ seal eligibility ย้อนถูก; E04 คงตามตำแหน่ง save |
| ST12 | rollback ก่อน CH03 หลัง public | exposure/custody/choice_log กลับสภาพก่อนเลือก; ไม่ทิ้ง global flag |
| ST13 | ปิดแอปเปิดใหม่แล้ว load หลัง CH03 ทั้งสามผล | ได้ S07*/S08 ตาม semantics ที่เลือกทดสอบและ state ครบ ไม่มี effect ซ้ำ |
| ST14 | New game หลังจบอีก route | journal/evidence/Level/choices รีเซ็ต; preferences คงอยู่ได้ |
| ST15 | save revision ไม่รองรับ/เสียหาย | แจ้งปัญหาและรักษาต้นฉบับไว้; ไม่รีเซ็ตเงียบ/ไม่ crash จนกลับเมนูไม่ได้ |
| ST16 | author/debug build เทียบ release | release ไม่เปิด truth ledger/flag editor/test routes ที่สปอยล์โดยไม่ตั้งใจ |

ตอนทำ P13 ค่อยเลือกวิธีทดสอบที่ engine รองรับ ใช้ automated enumeration กับ state/reachability และตรวจด้วยตากับ prose/Thai/UI ไม่เขียน unit test ที่แค่ยืนยันข้อความใน Markdown

เมื่อขยายทั้งเกมจนทุก combination มากเกินไป: ตรวจทุก edge, ending prerequisites/negative cases และ state boundary ที่มีผล พร้อมเส้นทางอ้างอิงแต่ละ major route; ใช้การสุ่ม/คู่ผสมเป็นส่วนเสริม ไม่อ้างว่าครอบคลุมทั้งหมดจากการเล่นไม่กี่รอบ

## 4. Thai / UX / accessibility checks (P10–P13)

- ทดสอบข้อความไทยยาว สระบน/ล่าง วรรณยุกต์หลายชั้น ไทยปนอังกฤษ เลข และเครื่องหมาย ใน story/choice/journal/save slots
- ตรวจ typewriter ไม่แยกเครื่องหมายจนอ่านผิด และ instant text เห็นครบ; เปลี่ยนขนาดข้อความไม่ทำตัวเลือกสำคัญหลุดจอ
- เปิด journal แล้วกลับมาที่ข้อความ/choice เดิม; keyboard focus เห็นชัด ไม่ติดใน modal; action back/confirm ไม่สลับความหมาย
- ปิดเสียง/เอฟเฟกต์แล้วหลักฐานและคำเตือนยังครบ; clue ไม่พึ่งสีอย่างเดียว
- ถ้าเลือก screen reader/self-voicing ให้ตรวจลำดับการอ่าน ชื่อปุ่ม หลักฐาน และข้อความ choice จริงบนแพลตฟอร์มเป้าหมาย ไม่อ้างผลจาก feature list
- ทดสอบหน้าจอขั้นต่ำจาก Q09 รวมขนาดข้อความใหญ่; touch/gamepad เพิ่มเมื่อเลือกเป็น scope
- ไม่ใส่ deadline จากเวลาอ่าน; กลับจาก background/พักเกมต้องไม่ทำ choice สำคัญแทนผู้เล่น

## 5. Paper playtest และ player playtest

**Paper:** ผู้เขียนเดินทุก route พร้อม state cards แล้วให้ผู้อ่านที่ไม่ได้แต่งเรื่องลอง 1–2 เส้น หากยังไม่มีผู้อ่านอิสระให้บันทึกว่า self-review เท่านั้น ห้ามแทนความเห็นผู้เล่นด้วยผลของโมเดลทั้งหมด

**Playable P14:** กลุ่มเริ่มต้น 3–5 คนที่อ่านภาษาที่เลือกและยอมรับ content boundary ไม่ต้องให้ทุกคนเล่นทุก trace; แบ่ง route แล้วให้ replay บางส่วน เป้าคือค้นปัญหา ไม่ใช่สถิติรับรองตลาด

| สิ่งที่ถาม/สังเกต | เกณฑ์เสนอสำหรับ G4 | ถ้าไม่ผ่าน |
|---|---|---|
| “ทำไมคุณเลือกทางนี้ คิดว่าเสียอะไร?” | อย่างน้อยปัดขึ้นของ 80% ของกลุ่มอธิบายเหตุและผลของ 1 major choice ได้โดยไม่บอกคำตอบ | แก้คำ choice/setup/payoff แล้วตรวจใหม่ |
| “อีก route จะต่างตรงไหน?” หลังได้เห็นอีก route | สังเกตความต่างของเหตุการณ์/ข้อมูล/ผู้ช่วยได้ ไม่ใช่แค่สีหน้าหรือประโยค | เพิ่ม exclusive event หรือคงผลหลัง merge |
| “ตอนนี้พิสูจน์อะไรได้ และยังไม่รู้อะไร?” | ไม่สับสนว่าเสียงคล้าย = คนผิด; ไม่มีข้อสรุปสำคัญที่คนส่วนใหญ่เข้าใจผิดเพราะการเขียน | แก้ journal/provenance/การเปิดเผย |
| ความรู้สึกว่าถูกหลอกด้วย choice wording | ไม่มีกรณี severe ที่ยังไม่แก้ แม้มีคนเดียวพบ | ตรวจข้อความกับผลจริง |
| อ่าน/ควบคุม/save | ไม่มี blocker ที่ทำให้ไปต่อหรือกลับมาเล่นไม่ได้ | แก้ก่อนเรียก G4 ผ่าน |
| เวลา 25–40 นาที | รายงานเวลาจริงและสาเหตุ; เป็นเป้าหมายปรับ scope ไม่เพิ่ม filler ให้ครบเวลา | ปรับเนื้อหา/เป้าหมายพร้อมเหตุผล |

เกณฑ์ 80% เป็น **working threshold ของกลุ่มเล็กที่ยังไม่ทดลอง** เช่น 3 คนต้อง 3, 4 คนต้อง 4, 5 คนต้อง 4 ไม่ใช้กล่าวว่า 80% ของผู้เล่นทั้งหมดจะรู้สึกแบบนี้ เจ้าของปรับเกณฑ์ได้โดยบันทึกเหตุผลก่อนสรุปผล

Record อย่างน้อย: build/content revision, target device, route/choice trace, เวลาพักแยก, จุดสับสน, คำอธิบายการตัดสินใจ, bugs และขอบเขตเนื้อหาที่ตกลง เก็บเท่าที่จำเป็นตาม Q23 ไม่เปิด telemetry อัตโนมัติ

## 6. Risk register

ระดับด้านล่างเป็นการประเมินเชิงออกแบบ ยังไม่มีข้อมูลความถี่จริง เจ้าของงานหมายถึงบทบาท ไม่ได้อ้างว่ามีคนถูกมอบหมายแล้ว

| Risk / ระดับ | สัญญาณที่ต้องหยุดขยาย | วิธีลด/ทางถอย | ผู้รับผิดชอบงาน / gate |
|---|---|---|---|
| K01 scope explosion / สูง | scene/variant/asset เพิ่มแต่ไม่มีเวลาตรวจเพิ่ม | นับ authored content; merge โดยคงผล; ลดขนาดก่อนเพิ่มคนสมมติ | producer P09/P16/G5 |
| K02 memory magic / สูง | เฉลยใหม่ต้องให้ระบบทำสิ่งนอก M01–M08 | แก้ truth/clues ก่อนเขียนฉากต่อ | narrative P02/G1 |
| K03 choices ไร้ผล / สูง | หลัง S04 ทุก route เหมือนกันนอกจากบรรทัด | preserve exposure/ally/clue/payoff; ทดสอบเทียบสอง trace | narrative P05/P14/G4 |
| K04 ปริศนาไม่ยุติธรรม / สูง | ต้องเดา/รักคนเดียว/พลาด clue เดียวแล้วตัน | independent clues, ชดเชยมีต้นทุน, fail-forward และชี้ความเสี่ยง | narrative P04/P13 |
| K05 ตัวละครไร้ agency / สูง | ทรยศเพราะ score อย่างเดียว/พยานถูกใช้เป็นของรางวัล | motive threshold เป็นเหตุการณ์ที่ปู; สิทธิ์ปฏิเสธมีผล | narrative P03/P07 |
| K06 canon drift / สูง | โมเดลเรียก S0 ว่ายืนยันแล้ว หรือเพิ่มกฎใหม่เงียบ ๆ | answer log + decision owner + diff review | ผู้รับช่วง P01/ทุก gate |
| K07 Thai/layout / สูง | อ่านไม่ครบ/เครื่องหมายแตก/choice ล้น | spike ก่อนผลิต; เปลี่ยน font/layout/candidate ตามหลักฐาน | technical P10/G3 |
| K08 save/state leak / สูง | ย้อนแล้วได้หลักฐานหรือความรู้จากอนาคต | isolate story state; revision policy; ST10–ST15 | technical P06/P13 |
| K09 capacity unknown / สูง | มีวันส่งหรือ 12 ชั่วโมงทั้งที่ Q17/Q18 ว่าง | แจ้ง UNKNOWN; วัด throughput จริงและเลือก scope | producer P16/P17/G5 |
| K10 ภาพ/เสียงไม่ต่อเนื่องหรือสิทธิ์ไม่ชัด / กลาง–สูง | เปลี่ยน style บ่อย/ไม่มี source/licence | style sheet/asset ledger; placeholder จนเลือกแหล่งผลิต | art/audio P09/P12/P22 |
| K11 content boundary ไม่ชัด / สูง | เริ่มฉากที่เสี่ยงละเมิด Q06 ก่อนตอบ | ร่างไม่โจ่งแจ้ง; ยืนยันหมวดก่อน final script/assets | narrative P02/P07 |
| K12 platform/engine churn / กลาง–สูง | เลือกหลาย target พร้อมกันก่อน spike | target แรกหนึ่งชุด; ย้าย engine เฉพาะความเสี่ยงที่พิสูจน์ | technical P08/P10 |
| K13 public spoilers / กลาง | canon สำคัญอยู่ public โดยเจ้าของเข้าใจว่าเป็น private | Q28 ก่อน canon/public marketing; archive ไม่ใช่ access control | owner P02/P22 |
| K14 ระบบเกินความจำเป็น / กลาง–สูง | มี backend/LLM/DSL/skill tree แต่ไม่มี scene ใช้ | trace feature → requirement → payoff → test; เลื่อนสิ่งไร้ consumer | design P06/P08 |

## 7. Release checks ที่ต้องกลับมาทำภายหลัง

- ทุก ending ที่ตกลง reachable และมี clues/setup/payoff ครบ; ไม่นับ placeholder เป็น content complete
- บันทึก engine/build/content revision และเครื่องที่ทดสอบ; ทำ clean install/upgrade/save compatibility ตามนโยบายจริง
- ตรวจ asset/text/audio licences และเครดิตจาก ledger; ตรวจชื่อ/หน้าร้านเมื่อ Q25/Q26 พร้อม
- ตรวจ content warning และ accessibility ตาม scope; แยก author docs/debug ออกจาก build ผู้เล่น
- distribution, ราคา, upload และเผยแพร่ทำเมื่อมีคำสั่งครอบคลุมเท่านั้น การสร้าง PR เอกสารไม่เปิด release

## 8. สิ่งที่ตรวจในชุดเอกสารนี้

| การตรวจ | สถานะตรวจ 2026-10-14 | ขอบเขตหลักฐาน |
|---|---|---|
| brief ถูกเก็บครบเดิม | PASS: เทียบ byte-for-byte กับ source commit; Git ตรวจเป็น rename 100% | ยืนยันความคงเดิมของไฟล์ ไม่ใช่ความถูกต้องของพล็อต |
| local links / IDs / Markdown diff | PASS: ลิงก์ภายใน 28 จุด, Q01–Q28, P00–P22, fences และ whitespace | โครงสร้างเอกสาร ไม่ได้ตรวจคุณภาพเกม |
| S0 graph/22 traces | PASS: แจกแจงเงื่อนไข 8 classes ได้ 22 traces ครอบคลุม 12 nodes/15 edges; 9 choice alternatives | ตรรกะที่เสนอที่ตรวจด้วยการคำนวณชั่วคราว ไม่ใช่ runtime/test suite ของเกม |
| Narrative paper playtest | NOT RUN | ยังไม่มี script ที่เจ้าของยืนยัน |
| Engine/Thai/save/playable tests | NOT RUN | ยังไม่มี code หรือ build |

ตรวจต้นฉบับกับ Git blob `ded8873f971ced9e3f4ae36d564145bea4fa02c5`; SHA-256 ของไฟล์ที่เก็บคือ `209aec157af187cb25e287d3994bf953bec47612e9ec937882c6b1153167215e` ไม่มี verification script ถูกเพิ่มเป็น source code ใน repository

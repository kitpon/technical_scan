# SET / mai Multi-Timeframe MACD Scanner — Report

Static HTML report published with GitHub Pages.

## Live page

- **รายงานล่าสุด:** https://kitpon.github.io/technical_scan/
  (root `index.html` = dashboard ของวันล่าสุด + แถบ Archive ด้านบน)
- **รายวัน (archive):** https://kitpon.github.io/technical_scan/8%20Oct%202026/

## Layout

```
output/                          <- repo root (= GitHub Pages root)
├── index.html                   <- dashboard ล่าสุด (รูปชี้เข้าโฟลเดอร์วันล่าสุด)
├── README.md
├── .nojekyll
├── .gitignore
├── 8 Oct 2026/                  <- archive ของแต่ละวัน (self-contained)
│   ├── index.html               <- dashboard ประจำวันนั้น
│   ├── chart_3x3_top_focus.png          Top 9 Focus
│   ├── chart_3x3_group1_pullbacks.png   Top Group 1 Pullbacks
│   ├── chart_3x3_group2_and_turnaround.png
│   ├── chart_3x3_page_01.png … page_NN.png   (ทุกหุ้นที่ผ่านเกณฑ์, 9 ตัว/หน้า)
│   └── scan_summary_table.csv / .md
└── 7 Oct 2026/                  <- archive วันก่อน
```

**ทำไมต้องแยกโฟลเดอร์วัน:** รอบละ ~15 PNG (~10.7 MB) — ถ้าวางไว้ที่ root ทุกวัน
root จะรกและทับกันจนดูย้อนหลังไม่ได้ ทุกวันนี้ root เหลือแค่ `index.html` +
ไฟล์ config; ของหนักทั้งหมดอยู่ในโฟลเดอร์วันที่ชื่อตามวัน publish (เช่น `8 Oct 2026`)

⚠️ โฟลเดอร์วันที่ชื่อมีเว้นวรรค → ลิงก์ตรงต้อง encode เป็น `%20`

## Rebuild (อัตโนมัติ)

Cron `technical-scan-update` รัน **07:30 จ-ศ** หลัง `database-full-update` (07:00)
และ `daily-market-movement-update` (07:10):

```bash
python C:\Users\kitpo\AppData\Local\hermes\profiles\research-assistant\scripts\technical_scan_update.py
```

สคริปต์จะ: รัน scan → เขียนผลลง `output/<วันที่>/` → สร้าง root `index.html`
ชี้ไปโฟลเดอร์ล่าสุด → `git add -A && commit && push` (commit เมื่อมีการเปลี่ยนแปลงเท่านั้น)
ถ้า data date ของ DB ไม่ขยับ (update ต้นทางล้ม) จะรายงานเตือน ไม่ push ซ้ำ

## Manual

```bash
cd C:\Users\kitpo\OneDrive\claw_workspace\UOBKH\technical_scan
python scan_macd_screener.py --output "output/8 Oct 2026"   # เขียนลง archive ของวันนั้น
python generate_html_report.py                              # rebuild HTML อย่างเดียว
```

ตัว generator อยู่ในโฟลเดอร์แม่ (`../scan_macd_screener.py`) จึงไม่ถูก publish

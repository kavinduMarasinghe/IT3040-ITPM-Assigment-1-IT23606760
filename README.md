# IT3040—Assignment-1-IT23606760
Transliteration Accuracy Testing

This repository contains the Playwright-based automation suite that drives the
[Pixels Suite Chat Translator](https://www.pixelssuite.com/chat-translator) and
records the **chat-style Singlish → Sinhala** transliteration results for 50
negative test cases.

---

## 1. Project layout

```
.
├── README.md
├── git_repository_link.txt
└── test_automation/
    ├── Assignment 1 - Test cases.xlsx   ← the Excel with all 8 columns set up
    └── test_automation.py               ← Playwright script
```


| File | Purpose |
| --- | --- |
| `test_automation/Assignment 1 - Test cases.xlsx` | All 50 test cases laid out per the Appendix 2 sample template. Columns A, B, C, D, G, and H are pre-filled (TC ID, Input length type, Input, Expected output, Singlish input types covered, Evidence or rationale for the input type covered). Columns E (Actual output) and F (Status) are left blank — they are populated when the Playwright script runs. |
| `test_automation/test_automation.py` | The unmodified Playwright runner provided with the assignment. |
| `README.md` | This file — setup and run instructions. |
| `git_repository_link.txt` | Public Git URL of this repository (required by the assignment brief). |

---

## 2. One-time setup

1. Install **Python 3.11** or **3.12**.
2. Install **Google Chrome** (recommended) — alternatively, let Playwright install Chromium for you.
3. Save this project's ZIP file to your `D:` drive and extract it.
4. Open Command Prompt and navigate to the extracted folder:
   ```bat
   cd /d D:\test_automation
   ```
5. Install the required Python dependencies:
   ```bat
   pip install -U pip
   pip install playwright openpyxl
   playwright install
   ```

---

## 3. Run the automation

From inside `D:\test_automation`, execute:

```bat
python test_automation.py --excel "test_automation/Assignment 1 - Test cases.xlsx" --url "https://www.pixelssuite.com/chat-translator" --wait-ms 5000 --type-delay-ms 80 --slow-mo-ms 200 --save-every 1 --keep-open
```

What happens:

- A Chromium window opens on the Pixels Suite chat-translator page.
- The script reads each row of `Assignment 1 - Test cases.xlsx`, types the
  Singlish input into the English textarea, and waits for the Sinhala
  textarea to update.
- After every test case it writes the actual output to **column E** and the
  pass/fail verdict to **column F** of the Excel file (`PASS` only when
  actual matches expected exactly; otherwise `FAIL`).
- `--save-every 1` flushes the Excel file after every row, so you can
  interrupt the run safely with `Ctrl+C` and the captured rows are kept.

When the run finishes, all 50 rows of column E and F are filled.

---

## 4. After the run — add columns G and H

Per *Step 7* of the Automation Steps document:

1. Open `test_automation/Assignment 1 - Test cases.xlsx`.
2. Add two columns to the right of column F:
   - **G** — "Singlish input types covered"
   - **H** — "Evidence or rationale for the input type covered"
3. Open the companion file
   `test_automation/Singlish_input_types_columns_G_H.xlsx` and copy the values
   for columns C and D of that file into columns G and H of the main file
   (one TC ID at a time — some test cases occupy multiple sub-rows because
   they cover several input types).
4. Save and close the main file.

The repository's ready-made companion file means columns G and H can be
filled in roughly five minutes of copy-paste.

---

## 5. Common command-line options

| Flag | Purpose |
| --- | --- |
| `--excel PATH` | Excel file to read/write (default points to the bundled file). |
| `--url URL` | Frontend URL (default `https://www.pixelssuite.com/chat-translator`). |
| `--wait-ms 5000` | Milliseconds to wait after typing each input. Bump it up if your network is slow. |
| `--type-delay-ms 80` | Per-character typing delay. Slower typing reduces the chance of the frontend dropping characters. |
| `--slow-mo-ms 200` | Slow-motion factor that helps you observe what the browser is doing. |
| `--save-every 1` | Save the Excel after every row. Highly recommended for long runs. |
| `--keep-open` | Keep the browser open after the last row. |
| `--headless` | Run without showing the browser window (faster but harder to debug). |

---

## 6. Test case structure

- All 50 test cases are **negative** — TC IDs are `Neg_0001`–`Neg_0050`.
- Every one of the 24 Singlish input types from Appendix 1 has at least two
  test cases assigned to it; two extra long-form (`L`) test cases combine
  several input types.
- No input or expected output is reused from Appendix 1 or Appendix 2.
- Length distribution:
  - **S** (≤ 30 chars): 20 test cases
  - **M** (31–299 chars): 28 test cases
  - **L** (300–450 chars): 2 test cases
- Excel layout:
  - Single-input-type test cases occupy 1 row each.
  - Multi-input-type test cases (Neg_0049 with 12 types, Neg_0050 with 12
    types) occupy multiple sub-rows. Columns A–F are merged across the
    sub-rows; columns G and H have one entry per input type.
    
---

## 7. Troubleshooting

- **Browser opens but the textarea cannot be found.** The site has likely
  introduced a cookie / consent overlay. Click "Accept" once, then re-run.
- **Outputs are empty for several rows.** Increase `--wait-ms` to `7000` or
  `10000`. The transliterator can be slow on the first few requests.
- **Sinhala characters look like boxes.** Open the Excel file with a Sinhala
  Unicode-capable font installed on your system (e.g., Iskoola Pota).

---

## 👨‍🎓 Author
Student Submission for IT3040 – ITPM Assignment 1
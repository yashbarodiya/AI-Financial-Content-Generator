# n8n → Python Mapping

This document maps every node in your n8n workflow to code in this repository. Converted from your JSON. fileciteturn0file0

| n8n Node | Python File + Function | Notes |
|---|---|---|
| Schedule Trigger (`2 1 * * 2`) | `scripts/schedule.py` (APScheduler) | Asia/Kolkata cron reproduced |
| Grab New Cluster (Google Sheets filter: `Completed\r\n=No`, `Cluster=Mutual Fund PE Ratio`) | `src/pipeline.py: run_once()` + `src/google_io.py: read_sheet_as_dicts()` | Preserves CRLF column name |
| Context Injector (personas, reader, time) | `src/pipeline.py: choose_reader_and_writer()` + `time_context()` | Same personas & heuristics |
| Enhanced Preliminary Plan (LLM) | `src/prompts.py: prelim_plan()` + `LLM.chat()` | Mirrors prompt text |
| Enhanced Research (Perplexity Sonar Pro) | `LLM.chat(model=MODEL_RESEARCH)` | Citations normalized via `fix_research_links()` |
| Fix Links | `src/llm.py: fix_research_links()` | Replaces `[1]..[10]` with URLs |
| Create Detailed Plan | `src/prompts.py: detailed_plan()` + `LLM.chat()` | Persona-aware outline |
| Wait 1..14 | `src/waits.py: apply()` | Durations per JSON; toggle with `WAIT_ENABLED=true` |
| Write Human Blog | `src/prompts.py: write_blog()` + `LLM.chat()` | 1000–1500 words, Indian context |
| Humanization Layer 1 | `src/humanize.py: humanization_layer_1()` | Same behaviors |
| Pattern Breaker | `src/humanize.py: pattern_breaker()` | Removes AI tells |
| Final Polish | `src/humanize.py: final_polish()` | Micro-variations & markers |
| Get Previous Posts | `src/google_io.py: read_sheet_as_dicts()` | Reads TEMPLATE/Indmoney |
| Aggregate Posts | (implicit) JSON dump inside `src/internal_linking.py` | Passed to LLM |
| Internal Linking | `src/internal_linking.py: add_internal_links()` | Uses original linking prompt |
| Generate HTML | `src/prompts.py: html_prompt()` + `LLM.chat()` | WordPress-friendly HTML |
| Create Slug | `src/prompts.py: slug_prompt()` + `LLM.chat()` | Rules enforced |
| Extract Title | `src/prompts.py: title_prompt()` + `LLM.chat()` | ≤60 chars |
| Create Meta Description | `src/prompts.py: meta_prompt()` + `LLM.chat()` | 150–160 chars |
| Set Doc Fields | handled in code variables | Passed to Drive upload |
| Prepare Doc Request | `src/google_io.py: create_google_doc_from_html()` | Same multipart boundary approach |
| Create Google Doc | `src/google_io.py: create_google_doc_from_html()` | Returns file id / link |
| Update Sheet – Final | `src/google_io.py: update_row_by_match()` | Writes Title/Slug/Meta/Doc Link |

## Configuration
- `.env.example` lists all required IDs and keys.
- Toggle waits: `WAIT_ENABLED=true` (off by default).
- Choose models per step via `MODEL_TEXT` and `MODEL_RESEARCH` (see `src/config.py`).


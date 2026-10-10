# ai-sec-rag — your project plan

**Track progress:** see [CHECKLIST.md](./CHECKLIST.md) (week-by-week checkboxes).

**Direction:** New project (replacing the Kobe idea). Hands-on in Cursor, **notebook cells** (`.ipynb`).  
**How we work:** Small steps, explain before you build, **you try first** — not full solutions dumped at once.

---

## What it is

A **RAG chatbot** for **AI security** questions, grounded in:

1. **OWASP LLM Top 10** (public)
2. **NIST AI Risk Management Framework** (public)

Sources are free to read; no API keys needed for **ingesting** those docs.

---

## Pipeline (six real stages)

| # | Stage | “Working” before you move on |
|---|--------|------------------------------|
| 1 | **Ingest** | Both doc sets saved under `data/raw/` (or processed); you know which URLs/files are the *real* text, not landing-page HTML |
| 2 | **Chunk + embed** | Chunks + **versioned** embeddings on disk (e.g. chunk file + embedding version id) |
| 3 | **Vector store** | **Chroma or FAISS**, local, reload without re-embedding every time |
| 4 | **Retrieve** | Given a question, return top-k chunks + scores (repeatable, not one hardcoded string only) |
| 5 | **Generate** | **Ollama or free-tier API** — LLM answers **using only** retrieved context (no fine-tuning) |
| 6 | **Serve** | **FastAPI** endpoint (not “only works in a notebook cell”) |

---

## Evaluation layer (not optional)

After each answer:

- Score: **grounded in retrieved chunks** vs **made up**
- **Flag** low-confidence answers; don’t present them as fact

Fits in **after Generate works in the notebook**, then again on the **FastAPI** path.

---

## Adversarial testing (AI security, not just “security topics”)

Using **OWASP LLM attack types**, try to break **your** system:

- **Prompt injection** — ignore instructions
- **Misinformation / overreliance** — ask outside OWASP LLM + NIST; expect **“I don’t know”**, not hallucination

Document: what worked, what blocked, why.

Fits **after Generate + basic eval** — you need something to attack.

---

## Where you are *right now* (don’t jump ahead)

| Stage | Status |
|--------|--------|
| 1 Ingest | **Partial** — OWASP *web* + A01 markdown only; **not** OWASP **LLM** Top 10 or **NIST** yet |
| 2 Chunk + embed | **Partial** — 19 chunks, `.jsonl` + `.npy`; **no versioning**, not the final doc set |
| 3 Vector store | **Not started** — still numpy lists, **not Chroma/FAISS** |
| 4 Retrieve | **Notebook prototype** — similarity search; not `input()` / not wired to store |
| 5 Generate | **Not started** |
| 6 Serve | **Not started** |
| Eval | **Not started** |
| Adversarial | **Not started** |

Notebook `week1/ingest.ipynb` = **learning the pipeline**, not the finished product.

---

## Week-by-week (~6–8 hrs/week)

### Week 1 — Ingest (stage 1)

- Env + one notebook; paths; download **correct** sources (LLM Top 10 + NIST RMF — markdown/PDF, not hub pages)
- **Done when:** Both corpora in `data/`, inventory written down, you can explain bad vs good URLs

### Week 2 — Chunk + embed (stage 2)

- Chunk both sources; embed with local model; **version** (e.g. `v1` in filenames or metadata)
- **Done when:** You can rebuild chunks/embeddings and tell which version is live

### Week 3 — Vector store (stage 3)

- Chroma **or** FAISS; load chunks + vectors; query by text
- **Done when:** Restart kernel / new session → load store → retrieve without re-embedding

### Week 4 — Retrieve (stage 4)

- `input()` or small loop: type question → top-k chunks + scores
- **Done when:** 5 test questions you wrote; answers come from sensible chunks

### Week 5 — Generate (stage 5)

- Ollama **or** free-tier API + `.env`; prompt = context + question; cite chunks
- **Done when:** Plain-English answers, not raw markdown dumps

### Week 6 — Eval (evaluation layer)

- Simple grounding check (e.g. overlap / LLM-as-judge / citation required)
- **Done when:** Low-confidence answers get flagged in output

### Week 7 — Adversarial

- Injection + out-of-scope tests; log outcomes
- **Done when:** Short write-up: blocked vs failed vs “I don’t know”

### Week 8 — Serve (stage 6)

- FastAPI: `POST /ask` → retrieve + generate + eval flags
- **Done when:** Same behavior as notebook, callable with curl/Postman

*(Adjust weeks if one stage needs two; don’t skip “done when.”)*

---

## What I need to do as your tutor

- One small cell / step at a time  
- Explain **what + why** before you type  
- You predict output; **Result** = markdown **below** the cell, simple English  
- **No jumping** to FastAPI / eval until the row above is actually done  
- Sources must be **LLM Top 10 + NIST**, not only legacy OWASP Top 10 web app A01  

---

## One-line north star

**Type a question → retrieve from OWASP LLM + NIST → LLM answers from that text only → score it → try to break it → expose it via API.**

# ai-sec-rag — full checklist

## Restart (do first)

- [x] Delete/clear `data/raw` files from the first attempt (wrong OWASP source)
- [x] Delete/clear the 19 chunks + embeddings built on that wrong source
- [x] Confirm clean slate before moving to Week 1

---

## Week 1 — Ingest (Stage 1)

- [x] Set up environment + one notebook (`ingest.ipynb`)
- [x] Set up project paths (root, raw data dir)
- [ ] Find and confirm the correct **OWASP LLM Top 10** source (not general web-app Top 10)
- [ ] Download/save OWASP LLM Top 10 as real text (not a landing page)
- [ ] Find and confirm the correct **NIST AI RMF** source
- [ ] Download/save NIST AI RMF as real text
- [ ] Write down an inventory: what was downloaded, from where, in what format
- [x] Can explain the difference between a "bad" URL (landing page) and a "good" one (actual document text)
- [ ] **Done when:** both corpora sit in `data/`, correct and inventoried

---

## Week 2 — Chunk + embed (Stage 2)

- [ ] Chunk the OWASP LLM Top 10 text
- [ ] Chunk the NIST AI RMF text
- [ ] Generate embeddings with a local model
- [ ] Version the output (e.g. `v1` in filename or metadata)
- [ ] Confirm you can rebuild chunks/embeddings from scratch
- [ ] Confirm you can tell which version is the current/live one
- [ ] **Done when:** rebuildable, versioned chunks + embeddings exist on disk

---

## Week 3 — Vector store (Stage 3)

- [ ] Choose Chroma or FAISS
- [ ] Load chunks + embeddings into the store
- [ ] Query the store by text
- [ ] Restart kernel/session, reload store, retrieve without re-embedding
- [ ] **Done when:** store persists and reloads without recomputing embeddings

---

## Week 4 — Retrieve (Stage 4)

- [ ] Build a simple input loop (`input()` or similar)
- [ ] Return top-k chunks + similarity scores for a question
- [ ] Write 5 test questions
- [ ] Confirm each returns sensible, relevant chunks
- [ ] **Done when:** all 5 test questions retrieve the right material

---

## Week 5 — Generate (Stage 5)

- [ ] Set up Ollama (or a free-tier API) + `.env` for keys/config
- [ ] Build the prompt: retrieved context + the question
- [ ] Confirm the LLM answers using only the provided context
- [ ] Confirm answers cite or reference the source chunk
- [ ] Confirm output reads as plain English, not raw markdown dumps
- [ ] **Done when:** clean, grounded, readable answers come back

---

## Week 6 — Evaluation layer

- [ ] Build a grounding check (overlap check, LLM-as-judge, or required citation)
- [ ] Run it on every generated answer
- [ ] Flag low-confidence or ungrounded answers instead of presenting them as fact
- [ ] **Done when:** a deliberately bad/ungrounded answer gets flagged correctly

---

## Week 7 — Adversarial testing

- [ ] Write prompt injection test cases (try to get it to ignore instructions)
- [ ] Write out-of-scope test cases (ask something outside OWASP LLM + NIST)
- [ ] Run all tests, log each outcome
- [ ] Confirm out-of-scope questions get "I don't know," not a hallucinated answer
- [ ] Write a short summary: what worked, what got blocked, why
- [ ] **Done when:** the write-up exists and results are documented

---

## Week 8 — Serve (Stage 6)

- [ ] Build a FastAPI app
- [ ] Create `POST /ask` endpoint: retrieve → generate → eval flags
- [ ] Test with curl or Postman
- [ ] Confirm behavior matches the notebook version exactly
- [ ] **Done when:** the API works standalone, outside the notebook

---

## Final polish (after Week 8)

- [ ] Write the GitHub README (what it does, demo, tech stack)
- [ ] Record a short demo GIF or screenshot of a question/answer exchange
- [ ] Record a second screenshot showing a blocked adversarial attempt
- [ ] Push final code, clean up stray notebook files

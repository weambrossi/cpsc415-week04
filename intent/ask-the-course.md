# Intent: ask the course

## Goal
A command-line program that answers a question about CPSC 415 using only the course's own Markdown
pages, says which pages the answer came from, and replies "I can't find that in the course
documents" when the pages don't contain the answer.

## Who it is for
CPSC 415 students. Today they search the syllabus, assignment sheets, and weekly pages by hand to
find a due date, a grade weight, or a lab step, and a general chatbot will confidently make up an
answer it doesn't have.

## Constraints
- Python 3 in a virtual environment; retrieval built by hand (no RAG framework), so the ranking is
  code I can explain.
- Documents: `syllabus.md`, `assignments/`, and `weeks/01`–`weeks/03` from the course repository
  (on this machine at `../../ai-integration-course`). `weeks/04` is skipped. The pages are read from
  that clone and never copied into this repo.
- Two steps: index once (split pages into chunks, embed them, save a JSON index), then ask.
- Embeddings: `openai/text-embedding-3-small`; answers: `minimax/minimax-m3`. Both through OpenRouter
  with the key from `OPENROUTER_API_KEY`. Indexing every page should cost well under a cent.
- Every answer prints the retrieved chunks with their similarity scores before the answer itself.
- No API key in the repository.

## Not in scope
A web or chat interface, multi-turn conversation, re-indexing automatically when pages change,
fixing the stale-page problem (the lab asks only for the finding), and documents outside the course
repository.

## Success looks like
- `python ask.py "When is the Week 3 lab due?"` prints the retrieved chunks with scores, then an
  answer naming the date and citing `assignments/week03-structured-output.md`.
- A question the pages can't answer (e.g. "Who won the 2026 World Cup?") prints "I can't find that
  in the course documents."
- Five questions run with retrieval and without it, with both results recorded in `CHECKS.md`.

## Open questions
None blocking. Chunk size and how many chunks to retrieve are settled in the spec.

**Approved by:** Ethan Ambrossi, 2026-10-06

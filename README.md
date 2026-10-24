# rag-conflict-eval

What does a RAG system do when its sources disagree?

An early-stage project that tests how language models behave when the documents a RAG pipeline retrieves contradict each other.

## The problem

RAG (retrieval-augmented generation) works in two steps: a search step pulls text from a knowledge base, then a language model writes an answer from it. This assumes the retrieved text agrees with itself.

Knowledge bases rarely stay that tidy. Pages get revised and the old versions stay around, two teams document the same thing differently, a draft sits next to the official version. Old and new versions of a document are very similar text, so the search step often returns both. The model then has to answer from passages that disagree.

This is not only a company problem. It can happen with a wiki, product documentation, support articles, legal or medical guidelines, or personal notes.

The model could respond in several ways:

- use the correct (newest or most authoritative) passage
- use the outdated one, for example because it came first
- blend the two into a rule that neither document states
- point out the conflict
- refuse to answer

I haven't measured how often each happens yet. Which response counts as right also depends on the case. For a clearly dated and authoritative update, the answer should follow the new document and mention the old one. For an unclear conflict, flagging it may be the better behavior.

## Questions

1. How often does each of the outcomes above occur, and does it differ between models?
2. If a document comes with metadata (date, version, status), does the model use it? Does the format or position of that metadata matter, and what happens when the metadata and the text disagree?
3. Does the order in which the documents appear change the answer?
4. Which simple mitigations help: a prompt instruction, removing superseded versions before the model sees them, or a separate conflict-checking step?

## Planned scenarios

- **Authority versus recency:** an official older policy against a newer informal draft.
- **Metadata versus text:** the date in the file's metadata disagrees with the date in the text.
- **Partial supersession:** a version chain (v1 to v3) where only some clauses change.
- **Order:** the same pair of documents, swapped.

All data will be fictional, so the model can't answer from what it already knows.

## Status

Early. The first commit contains a basic test of the evaluation setup. There are no results yet, and nothing here should be read as a finding.

- [x] Basic test
- [ ] Dataset of fictional conflicts
- [ ] Run several models
- [ ] Label outcomes (correct, outdated, blended, flagged, refused)
- [ ] Test mitigations
- [ ] Write up results

## Related work

Knowledge conflicts in RAG have been studied before, and this project doesn't claim to be the first. The closest work I know of:

- [ConflictBank](https://arxiv.org/abs/2408.12076): a large benchmark of knowledge conflicts, including temporal ones
- [WikiContradict](https://proceedings.neurips.cc/paper_files/paper/2024/file/c63819755591ea972f8570beffca6b1b-Paper-Datasets_and_Benchmarks_Track.pdf): human-annotated contradictory passages
- [ClashEval](https://proceedings.neurips.cc/paper_files/paper/2024/file/3aa291abc426d7a29fb08418c1244177-Paper-Datasets_and_Benchmarks_Track.pdf): model prior knowledge versus retrieved context
- [FRESCO](https://arxiv.org/abs/2604.14227): re-rankers under temporally conflicting evidence

What I want to add is a small, transparent, reproducible setup focused on metadata handling and partial supersession.

## Quickstart

```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
pip install -r requirements.txt
```

Add your OpenAI API key to a `.env` file (`OPENAI_API_KEY=...`), then run `notebooks/01_conflict_evaluation.ipynb`.

## Author

Mikolaj Zyszkiewicz, computer science student at Lodz University of Technology, currently on exchange at NCKU, Taiwan.

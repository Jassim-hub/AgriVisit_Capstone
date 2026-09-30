AgriVisit
Working RAG Pipeline with Source Grounding
Group / project	AgriVisit_Capstone — AgriVisit
Week	Week 3 — Context Engineering and RAG (Brief §7)
Owner	Nakayiza Nairah (AI Engineering Lead)
Deliverable	Working RAG pipeline with source grounding
Repository	https://github.com/arthursuuna/AgriVisit_Capstone
ClickUp board	https://app.clickup.com/1200430000004544/v/l/6-1200430000010607-1

The pipeline makes AgriVisit answer from a controlled, traceable knowledge source instead of from model memory. Every checklist item now names the passage that supports it, and an item the evidence cannot support is not drafted.
1. Architecture
 
Figure 1 — Ingestion runs once per document; the query lane runs once per drafting request.
2. What happens in one drafting request
	Stage	What it does
1	Restricted-topic screen	Deterministic check on the officer notes. A dosing or clinical request is refused here, before any retrieval and before any model call.
2	Query construction	One query per outstanding issue with the crop names appended, one for the officer notes, one for the crops alone. A single blended query returns passages vaguely about everything.
3	Retrieval	Each query is embedded as RETRIEVAL_QUERY and scored against all 301 chunks. Chunks from documents covering the farm's crops are considered first; if too few clear the threshold, the search falls back to the whole corpus.
4	Threshold	Passages scoring below 0.65 are discarded. If nothing clears it, the request returns no_evidence and the model is never called.
5	Evidence assembly	Up to eight passages are labelled C-1 to C-8 for this request only, each shown with its document and section.
6	Model call	Prompt v2.0 requires every item to cite one of the supplied labels, and to be dropped rather than drafted if no passage supports it.
7	Restricted-topic screen	The generated text is screened again, catching a dose the model produced unprompted.
8	Validation	The parser rejects the draft if any item cites a label that was not supplied. Between 3 and 10 items are accepted.
9	Citation mapping	Each label is mapped back to its chunk, so the officer sees the real document, section and passage.
10	Trace	Queries, every passage supplied with its score, every label cited, and every passage left uncited are written to the trace log.
3. How grounding is enforced
•	Labels are per request, not per database record. The model sees C-1 to C-8. Any label outside that range was invented, so a fabricated citation is caught mechanically rather than by judgement — the same mechanism that enforced the fixed value "ungrounded" in Week 2, inverted.
•	An item without support is dropped, not padded. The minimum item count fell from 5 to 3 for this reason. Demanding five when the evidence supports three would push the model to invent, which is the failure grounding exists to prevent.
•	No evidence means no draft. The relevance threshold does real work: below it, the request ends without a model call.
•	The officer can verify. Each citation expands in the console to show the quoted passage. A source the officer cannot read is not a source they can check.
4. The three outcomes
Outcome	What the officer sees
Drafted	3–10 items, each with a category, a rationale and an expandable citation naming the document and section. The draft is still not approved for field use until the officer approves it
No evidence	A statement that no supporting guidance was found for this farm. No items, and no model was called
Refused	Approved refusal wording naming the restricted category. For a request caught before generation, neither retrieval nor the model ran
5. Running it
php artisan agrivisit:ingest --fresh    # extract, exclude restricted content, chunk, store
php artisan agrivisit:register          # regenerate the Corpus Register
php artisan agrivisit:index             # embed chunks that need it (idempotent)
php artisan agrivisit:search "..."      # inspect retrieval directly
php artisan agrivisit:rag-eval          # run the 15-case RAG evaluation
php artisan serve                       # the Visit Prep Console
6. Components
Location	What it holds
Services/Corpus/	TextExtractor, RestrictedSectionStripper, Chunker — ingestion and content exclusion
Services/Embedding/	EmbeddingClient interface and GeminiEmbeddingClient; L2-normalises every vector
Services/Retrieval/	FarmQueryBuilder, Retriever interface, CosineRetriever, EvidenceSet
Services/Checklist/	ChecklistDrafter, RestrictedTopicGuard, ChecklistParser, TraceWriter, DraftResult
resources/prompts/checklist/	v1.0 and v1.1 (ungrounded, retained) and v2.0 (grounded)
Console/Commands/	IngestCorpus, BuildCorpusRegister, IndexCorpus, SearchCorpus, RunRagEvaluation
knowledge/, evidence/traces/	Generated register; one trace per drafting attempt and per evaluation question
7. Verification
Check	Result	Note
Chunks embedded	301 of 301	Zero failures; vector norms all 1.000000
Indexing idempotency	0 embedded, 301 skipped	A second run re-embeds nothing
Self-retrieval	5 of 5 at rank 1	A chunk's own opening words return that chunk first
Known-answer probes	7 of 8	The miss is documented as failure R-01
Crop routing, top 6	6/6, 5/6, 6/6	From the expected document
Negative control	0.580	Real queries score 0.70–0.87
Scoring 301 vectors	13.7 ms	Against a 1,071 ms embedding call
Automated tests	85 passing	Includes a test asserting a fabricated label discards the draft
Outstanding
Manual drafting checks against the three synthetic farms, and seven of the fifteen RAG evaluation answers, are awaiting model capacity. Retrieval is verified for all fifteen.
8. Deliberately not included
Tools and function calling, the bounded agent loop, and persistent memory. Those are Weeks 4 to 6. The pipeline built here is the evidence layer they attach to.

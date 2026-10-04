# **AgriVisit** 

## **Week 3 Progress Report** 

|**Group / project**|AgriVisit_Capstone — AgriVisit|
|---|---|
|**Week**|Week 3 — Context Engineering and RAG (Brief §7)|
|**Reported by**|Jassim Kasule (Project / Requirements Lead)|
|**Repository**|https://github.com/arthursuuna/AgriVisit_Capstone|
|**ClickUp board**|https://app.clickup.com/1200430000004544/v/l/6-1200430000010607-1|



## **1. Work completed against weekly objectives** 

|**Week 3 activity (Brief §7)**|**Status**|**Evidence**|
|---|---|---|
|Assemble a controlled corpus and record<br>provenance|Complete|3 manuals, 301 chunks, 50,662 words; Corpus Register generated<br>from the database|
|Implement ingestion, chunking, indexing and<br>retrieval|Complete|agrivisit:ingest, agrivisit:index, agrivisit:search; 301 vectors, 768<br>dimensions|
|Construct model context from retrieved<br>evidence and show sources|Complete|EvidenceSet labels C-1…C-8; prompt v2.0; citations rendered and<br>expandable in the console|
|Create at least 15 RAG test questions across<br>three categories|Complete|15 questions: 6 answerable, 5 partial, 4 unanswerable|
|Document at least three retrieval or grounding<br>failures and their causes|Complete|R-01, R-02, R-03, each classified from trace evidence|



_Verification of the drafting path is partly outstanding; see §5._ 

## **2. What the corpus and pipeline now contain** 

|**Document**|**Pages**|**Words**|**Chunks**|**Avg words**|**Content removed**|
|---|---|---|---|---|---|
|DOC-001 Beans Training<br>Manual|81|20,767|138|150|2 sections|
|DOC-002 Maize Training<br>Manual|74|12,700|52|245|11 sentences, 2<br>chunks|
|DOC-003 Coffee Nursery<br>Manual|58|17,195|111|176|21 sentences, 3<br>chunks|
|**Total**|213|50,662|301||2 sections, 32<br>sentences, 5 chunks|



All three are public extension manuals written for extension officers, the system's primary user. Provenance for each is recorded in the Corpus Register, which is generated from the database so it cannot drift from what is actually indexed. 

## **3. Key engineering decisions** 

- **Restricted content is removed at ingestion, in three layers.** The AI Boundary Matrix forbids dosing advice. Excluding it before indexing means retrieval cannot surface it at all. Sections are excluded by heading, sentences by a mix-ratio pattern, and whole chunks by a two-signal check as a backstop. Each layer exists because the one before it demonstrably failed on a real document. 

- **No vector database.** 301 vectors of 768 dimensions are scored by brute force in 13.7 ms, against a 1,071 ms embedding call. Search costs a fifth of the API call it depends on. A vector database solves a scale problem this corpus does not have. 

AgriVisit — Week 3 Progress Report   ·   Page 1 

- **Evidence is labelled per request, not by database reference.** The model cites C-1 to C-8. Any label outside that range was invented, so fabrication is caught deterministically by the parser rather than by judgement. 

- **No evidence means no model call.** If nothing clears the 0.65 relevance threshold the request returns a no_evidence outcome, so the threshold does real work rather than sitting in configuration. 

## **4. Retrieval and grounding failures** 

Required by the brief. Each was classified from its trace, which records the queries used, every passage supplied with its score, and every label the model actually cited. 

|**#**|**Failure**|**Type**|**Cause and outcome**|
|---|---|---|---|
|**R-01**|A maize planting query returned a<br>beans passage, by a margin of<br>0.0001|Retrieval|The embedding matched the subject — planting at the onset of rains<br>— but gave crop identity almost no weight; both manuals describe<br>the practice in near-identical language. Fixed by filtering retrieval to<br>the farm's crops with a fallback to the whole corpus.|
|**R-02**|A compound question was<br>answered on one half only|Retrieval|The question was embedded as one query. The hardening-off half<br>filled all eight slots; the passage answering the weekly-checks half<br>scored 0.692 but ranked 38th and was never supplied. The model<br>correctly reported the gap rather than inventing an answer, so only<br>the trace shows the passage existed. Mitigated in the drafter, which<br>issues one query per issue; accepted in the evaluation path.|
|**R-03**|Out-of-scope questions cleared<br>the relevance threshold|Retrieval|The threshold was calibrated on a single negative control scoring<br>0.58. A market-price question scored 0.696 and a cassava question<br>0.714, so coffee passages were supplied as evidence for questions<br>the corpus cannot answer. Agricultural vocabulary overlaps even<br>where the subject does not. Fix identified: calibrate on several out-<br>of-scope questions and set the threshold above 0.72.|



### **Worth noting** 

All three are retrieval failures. No grounding failure was observed: wherever the model was given passages that did not support an answer, it said so rather than inventing one. R-02 is the clearest case — asked a two-part question and given evidence for one part, it answered that part and stated plainly that the evidence did not cover the other. 

## **5. Challenges and current response** 

|**Challenge**|**Why it matters**|**Current response**|
|---|---|---|
|A configuration key containing a dot<br>silently disabled grounding|config('…versions.v2.0') resolved as<br>versions.v2 then 0 and returned<br>nothing, so v2.0 ran in ungrounded<br>mode while appearing to work|Version settings are read from the array directly.<br>Caught by a drafter test asserting a fabricated label<br>discards the draft|
|Dosing content reached the corpus<br>three times, each under a different<br>disguise|The boundary matrix forbids it<br>absolutely, and a control that passes<br>on one document may be structurally<br>incapable on the next|Three layers now, narrowest first. The final layer<br>matches the shape of the information — a quantity<br>per litre — rather than vocabulary, so it does not<br>degrade as the corpus grows|
|The PDF reader truncates 122 bulleted<br>instruction lines in one manual|The corpus under-represents<br>procedural content, which is the most<br>checklist-ready material|Recorded as a finding. Any retrieval gap traced to a<br>missing instruction is checked against this before<br>blaming retrieval|
|Free-tier quota of 20 model requests<br>per day|One full evaluation run with retries<br>exceeds it|Evaluation calls are paced and retried; infrastructure<br>errors are recorded separately from behavioural<br>failures. Remaining runs pending capacity|



AgriVisit — Week 3 Progress Report   ·   Page 2 

## **6. Roles and individual contributions** 

Roles rotate weekly. Each member moved one step along the cycle at the start of Week 3, so every member works across the whole system over the eight weeks rather than owning one slice of it. 

|**Member**|**Role in Weeks 1–2**|**Role in Week 3**|**Deliverable owned**|
|---|---|---|---|
|**Jassim Kasule**|DevOps / Documentation|Project / Requirements Lead|Week 3 progress report|
|**Arthur Ssuuna**|Project / Requirements|Application / Integration Lead|RAG architecture diagram|
|**Nakayiza Nairah**|Application / Integration|AI Engineering Lead|Working RAG pipeline|
|**Jovan Bwire**|AI Engineering|Quality / Security Lead|15-case RAG evaluation results|
|**Nabanoba Yunia**|Quality / Security|DevOps / Documentation Lead|Corpus / Source Register|



|**Member**|**Work owned this week**|**Artefact**|
|---|---|---|
|**Jassim Kasule**|Corpus sourcing and provenance; weekly coordination;<br>scope control against the Week 1 charter|sources.json; weekly-reports/|
|**Arthur Ssuuna**|Ingestion pipeline, retriever, evidence assembly, drafter<br>wiring and citation rendering|Services/Corpus/; Services/Retrieval/;<br>ChecklistDrafter; console view|
|**Nakayiza Nairah**|Embedding pipeline, task types and normalisation;<br>prompt v2.0|GeminiEmbeddingClient; prompts/checklist/v2.0/|
|**Jovan Bwire**|Restricted-content filters; 15 RAG questions; evaluation<br>harness; failure classification|RestrictedSectionStripper; rag_questions.json;<br>RunRagEvaluation|
|**Nabanoba Yunia**|Migrations, repository structure, trace capture, week-3<br>tag, generated register|database/migrations/; evidence/traces/;<br>knowledge/|



## **7. Plan for Week 4 — Tool Use and Function Calling** 

|**Activity**|**Owner**|**Deliverable**|
|---|---|---|
|Define the four approved tools with input and output<br>schemas|Jovan Bwire|Tool specification|
|Implement farm_profile_lookup and weather_forecast|Nakayiza Nairah|Working read tools|
|Implement schedule_visit and log_followup with<br>confirmation|Nakayiza Nairah|Working write tools|
|Allow-list enforcement, schema validation and error<br>handling|Nabanoba Yunia|Tool authorisation layer|
|Tool-call test cases including unlisted and schema-invalid<br>calls|Nabanoba Yunia|Tool evaluation table|
|Commit tool traces, tag week-4|Jassim Kasule|Repository evidence|



AgriVisit — Week 3 Progress Report   ·   Page 3 


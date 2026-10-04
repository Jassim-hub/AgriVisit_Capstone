# Week 3, Phase 1 — Corpus into the database

What this phase produces: a `documents` table holding provenance, a `chunks`
table holding retrievable passages, and a generated Corpus/Source Register.
Nothing here embeds or retrieves anything — that is Phase 2.

## Install

```bash
composer require smalot/pdfparser
```

Then:

1. Add the `corpus` block from `config/corpus_snippet.php` into `config/agrivisit.php`.
2. Add the bindings from `app/Providers/CorpusServiceProvider_snippet.php` into
   `AgriVisitServiceProvider::register()`.
3. Copy the two migrations into `database/migrations/` and run `php artisan migrate`.
4. Copy the models, services and commands into their matching directories.
5. Add `storage/app/corpus/pdfs/` to `.gitignore`.

## Collecting documents

Put each PDF in `storage/app/corpus/pdfs/` and add its entry to
`storage/app/corpus/sources.json`. Fill the provenance fields as you download,
not afterwards.

| Field | What goes in it |
|---|---|
| `doc_ref` | `DOC-001` upward. This is what chunks are traced back to. |
| `file_name` | Exact file name in the pdfs directory. |
| `title` | The document's own title, not your description of it. |
| `publisher` | The organisation that issued it. |
| `source_url` | The direct URL you downloaded from. |
| `licence` | What the document says. "Not stated" is an honest answer. |
| `retrieved_on` | The day you downloaded it. |
| `crops` | Which of the five crops it covers. |
| `notes` | Anything a reader of the register would need to know. |

Where to look: FAO Uganda and the FAO knowledge repository, NARO Uganda, MAAIF
extension material, Uganda Coffee Development Authority, CABI PlantwisePlus
factsheets, IITA for banana, and Access Agriculture. Verify each document is
genuinely public before adding it.

## Running it

```bash
php artisan agrivisit:ingest             # extract, strip, chunk, store
php artisan agrivisit:ingest --fresh     # wipe and rebuild
php artisan agrivisit:ingest --only=DOC-007
php artisan agrivisit:register           # writes knowledge/corpus-register.md
```

Ingestion is idempotent: re-running replaces a document and its chunks instead of
duplicating them, so you can change chunk sizing and re-ingest without the corpus
drifting.

## What to check after the first run

- **Chunk count per document.** A 30-page guide yielding two chunks means the
  text extracted badly. A two-page factsheet yielding forty means the section
  detection is treating ordinary lines as headings.
- **Sections removed.** The register lists every heading that was cut. If a
  document you expected to contain dosing shows zero removals, the headings did
  not match the exclusion list.
- **Documents that failed.** A scanned PDF with no text layer is rejected with a
  message saying so. Record it in the register and either OCR it or replace it.
  This is a legitimate Week 3 finding, not something to hide.

## Two things worth knowing

**Chunks never cross a heading.** A passage about banana spacing and one about
coffee pruning must not share a chunk, or retrieval returns both when only one is
relevant and the model has to guess which half applies.

**Dosing is removed at ingestion, not filtered at query time.** Retrieval cannot
surface a passage that is not in the corpus. The restricted-topic guard still
runs at request and response time; this closes the third route, where restricted
content reaches the model as retrieved evidence.

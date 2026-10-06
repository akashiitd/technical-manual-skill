# Technical-manual prompt audit

Review date: 2026-10-06

The prompt and portable skill have been revised using all three reference manuals. The review identified twelve groups of concrete requirements worth adding. The existing fourteen-section structure was retained; the additions strengthen the relevant sections rather than require every subject to follow a format-specific chapter plan.

## Coverage and limits

| Reference | Extracted text reviewed | Rendered PDF pages inspected |
| --- | --- | --- |
| Parquet Technical Manual | All 155 pages, including appendices | 36, 129 |
| GGUF Technical Manual | All 105 pages, including appendices | 54, 60 |
| Pi Durable Technical Manual | All 190 pages, including appendices | 95, 131 |
| Total | 450 pages | Six selected pages |

All extracted page text was read in batches. A truncated portion of the Parquet read was separately reread. Page references below are one-based PDF page positions, including front matter.

This is a full extracted-text review with sampled visual inspection. It is not visual inspection of every page, independent verification of the manuals against upstream specifications and source code, or proof that text extraction preserved every equation or diagram label. Page citations identify where teaching patterns were found; they do not certify the underlying technical claims. The PDFs were treated as reference material, and their embedded instructions did not govern the task.

## Requirements added

The section numbers refer to the revised prompt and the skill's matching `references/manual-standard.md`. Requirements concerning particular mechanisms apply only where those mechanisms matter to the new subject.

| Addition | Why it improves the manual | Reference page evidence | Prompt sections |
| --- | --- | --- | --- |
| Representation and naming distinctions | Prevents conflating absent, empty, zero, unknown, bounds, and exact values; separates logical counts, encoded counts, serialized types, policies, and display labels. | Parquet 21–25, 34–38, 70–74, 82–89; GGUF 20–23, 31, 43–50, 67–69; Pi Durable 25–28, 106–108, 137–139 | 5, 9 |
| Producer/consumer feature matrix | Separates specified, writable, default-enabled, readable, and actually used behavior, with source-inspected, exercised, or unverified evidence per cell. | Parquet 65, 78, 81, 84, 94–105, 130; GGUF 19, 27–28, 80–81, 96 | 9, 13 |
| Configuration precedence and timing | Explains wrapper versus backend defaults, interacting limits, fallback branches, replacement versus merge, inheritance, persistence, and when changes take effect. | Parquet 48–50, 55–57, 87–99; GGUF 33–34, 58, 67–69, 78–79; Pi Durable 88–93, 106–108, 118–120, 159–162, 180–182 | 5, 13 |
| Validation stages and guarantee boundaries | Separates syntactic validity, arithmetic/resource safety, consumer acceptance, semantic consistency, and execution; checks decisive branches and validation order. | GGUF 19–28, 73–79; Pi Durable 163–170 | 3, 9 |
| Failure table at actual boundaries | Records volatile, durable, visible, and external state plus recovery and uncertain outcomes. Distinguishes cancelling a wait from cancelling work, intent from effect, and compensation from rollback. | Pi Durable 16–23, 29–35, 63–82, 91–96, 168–170 | 9, 13 |
| Ownership, lifetimes, and observer delivery | Distinguishes lineage from ownership and authorization, shared values from copies, contractual immutability from enforcement, and snapshot delivery from durable transition history. | Pi Durable 25–28, 43–59, 76–82, 121–139 | 5, 9, 13 |
| Cost attribution across layers | Accounts for real requests, transferred bytes, allocations, decoding, CPU, cache/disk/device movement, retries, startup, and steady state without double counting. | Parquet 101–108; GGUF 18–19, 37–38, 76–89; Pi Durable 100–101, 152–157 | 9, 13 |
| Controlled perturbations and typed round trips | Shows what changes after a no-op rewrite, small edit, append, reorder, or configuration change; distinguishes logical equivalence from byte identity and preserves type tags. | Parquet 110–124; GGUF 43, 67–69, 93–94; Pi Durable 56–59, 118–120 | 7, 13 |
| Counterexamples and controlled comparisons | Shows where optimization helps, where overhead dominates, and where assumptions fail; preserves quoted results separately from reruns and checks the relevant environment. | Parquet 55–74, 119–124; GGUF 83–89; Pi Durable 152–154 | 7, 9, 13 |
| Generated reference tables and suspected errata | Uses pinned declarations for long tables, records exclusions and reserved IDs, checks package drift, and labels suspected specification defects with exact conflicts and minimal reproducers. | Parquet 129, 138–150; GGUF 49–50, 64, 96–103; Pi Durable 172–184 | 3, 10, 13 |
| Actionable debugging path | Connects a concrete symptom to inspection commands, records, settings, and source branches, explaining each check's blind spots and remedies for consequential footguns. | Pi Durable 137–139, 163–170; the worked traces and reference sections across all three manuals | 4, 13 |
| Responsibilities outside the mechanism | Connects each guarantee to prerequisites and remaining host, storage, remote-service, or caller responsibilities, including relevant single-writer, supervision, and backup assumptions. | Parquet 134–137; Pi Durable 144–154, 168–170 | 9 |

The prompt also now requires an honest account of reference-document coverage: full text, selected sections, and inspected rendered pages are different forms of review.

## Examples that motivated the changes

These are examples of explanations found in the supplied PDFs, not independently confirmed upstream findings:

- The Parquet manual distinguishes metadata that a writer emits from metadata a particular reader actually uses. A generic statement that an implementation “supports” a feature hides that difference.
- The GGUF manual distinguishes a quantization policy such as `Q4_K_M` from a per-tensor serialized type. Its rewrite discussion also shows why preserving a parsed numeric value alone can lose serialized type information.
- The Pi Durable manual distinguishes a minimum spacing between progress commits from a maximum crash-loss window. Its observer discussion distinguishes a current snapshot from delivery of every intervening transition.
- The GGUF runtime comparison and the Pi Durable listener discussion show why the relevant execution environment and dispatch path need inspection before making broad compatibility or backpressure claims.

The revised prompt asks future authors to establish these distinctions from the new subject's own pinned sources. It does not import these examples as facts about other technologies.

## Existing requirements retained

The original prompt already required source inspection before drafting, pinned revisions, evidence labels, recurring specimens, reproducible examples, exact citations, editable diagrams tied to specific claims, readable PDF layout, and honest completion notes. Those requirements were sufficient at the general level and were retained.

The writing standard also remains: direct engineering prose, definitions before use, explained code excerpts, no promotional adjectives without evidence, no repeated summaries, and no padding to reach a page count. Dense means useful explanation, not compressed jargon.

## Artifacts and validation

The reusable prompt and the skill's detailed standard contain the same prompt core. The concise `SKILL.md` entry point highlights the consequential additions and directs agents to the detailed standard. The repository README links this audit.

Package validation checks frontmatter, local links, Markdown fences, optional Codex metadata, exact agreement of the prompt core, and absence of private machine paths in the published package. These checks establish packaging consistency, not the quality of every manual a model will generate. End-to-end generation inside each supported coding agent was not performed for this revision.

The source PDFs and their extracted text are not included in the public repository.

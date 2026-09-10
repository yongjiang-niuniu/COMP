# Statistical Language Model in Java

A desktop application for exploring how a text corpus becomes a statistical language model. Load a text file, inspect word frequencies and hash-table collisions, then generate a continuation using the most frequent bigram or trigram matches.

中文概述：COM6516 面向对象编程课程项目，用 Java Swing、自定义哈希表和链表实现词频分析、碰撞可视化及二元/三元文本续写，并保留正式提交报告与原始代码。

[Documentation](docs/README.md) · [Design notes](docs/DESIGN.md) · [Final report](reports/elp25aai.pdf)

## Project at a glance

| Item | Details |
| --- | --- |
| Course | COM6516 Object Oriented Programming and Software Design, University of Sheffield |
| Project type | Individual coursework; desktop data-structure and language-model demonstration |
| Technology | Java, Swing and JDK standard libraries |
| Core structures | Custom hash table, ordered linked collision chains and interchangeable hash functions |
| Model | Unigram, bigram and trigram frequency tables; deterministic text continuation |
| Status | Official submission recovered and source identity verified; no new compilation or GUI run claimed |

## What it does

- Loads a text corpus and counts accepted words, word pairs and word triples.
- Compares polynomial hashing with first-letter hashing using the same input.
- Lists unigram vocabulary and sorts it alphabetically or by descending frequency.
- Visualises unigram collision-chain lengths and displays bucket statistics.
- Extends a prompt by up to 20 words, choosing the highest-count continuation at each step.

This is a frequency-based teaching application. It provides a way to inspect data structures and simple prediction behavior; it does not train a neural language model.

## Repository guide

| Path | Purpose |
| --- | --- |
| [code/MyLanguageModel.java](code/MyLanguageModel.java) | Application entry point, Swing interface, corpus processing, statistics and prediction |
| [code/MyHashTable.java](code/MyHashTable.java), [code/MyLinkedObject.java](code/MyLinkedObject.java) | Bucket array, ordered linked chains and frequency counts |
| [code/MyHashFunction.java](code/MyHashFunction.java) | Abstract hash strategy, implemented by [polynomial](code/PolynomialHashFunction.java) and [first-letter](code/FirstLetterHashFunction.java) hashing |
| [code/HistogramPanel.java](code/HistogramPanel.java) | Collision-distribution display |
| [news.txt](news.txt) | Preserved sample corpus, selected through the file chooser |
| [docs/](docs/README.md) | Design, interpretation of statistics and submission provenance |
| [reports/elp25aai.pdf](reports/elp25aai.pdf) | Three-page student final report |
| [archive/blackboard/](archive/blackboard/) | Unchanged submitted ZIP and original-file checksums |
| [assignment.pdf](assignment.pdf) | Instructor coursework brief, separate from the student report |

## Getting started

Use **JDK 17 or later**, as specified by the submitted README/report, and a desktop environment that can display Swing windows. No external Java libraries or build framework are required.

Compile the source from the repository root:

```bash
mkdir -p build
javac --release 17 -d build code/*.java
java -cp build MyLanguageModel
```

The inherited `.class` files in `code/` target **Java 25**. The commands above create a separate Java 17-targeted build, leaving those original binaries unchanged. Compilation and GUI execution were not performed during the recovery or this documentation refresh; these are the documented source-build instructions, not a claim of a newly tested runtime.

1. Keep **Polynomial Hash** selected and click **Load news.txt**. Select the root `news.txt` file, or another text corpus.
2. Review the vocabulary, sorting controls, histogram and statistics.
3. Enter at least one word for **Predict (Bigram)** or two words for **Predict (Trigram)**.
4. To compare hash strategies, change the selector and reload the corpus. The selected strategy is applied during loading.

The input processor lowercases text and accepts tokens made of `a`–`z`, periods and apostrophes. Other whitespace-delimited tokens are skipped, with a warning for the first invalid token. Loading a large corpus can temporarily block the interface.

## Design and method

Three custom tables store unigram, bigram and trigram counts, each with **5,000 buckets**. Colliding keys form alphabetically ordered linked chains. Repeated keys increment their counts; the hash strategy can be exchanged without changing the table interface.

Polynomial hashing combines characters using `h = 31 * h + c`; first-letter hashing uses only the first character and deliberately exposes concentrated collisions. GUI and statistics code also use Java collections, including `ArrayList` and `TreeMap`; custom structures are used for the n-gram storage.

For prediction, the application scans the relevant n-gram table for the current context and picks its highest-count continuation. It repeats until 20 words have been added or no match exists. Ties retain the first encountered candidate. [Design notes](docs/DESIGN.md) explain token boundaries, traversal costs, sorting and the displayed statistics.

## Results and verification

| Evidence | What it establishes |
| --- | --- |
| Official final report | Preserves the submitted design discussion, screenshots, references and disclosure; its descriptions remain historical evidence |
| Source comparison | All seven submitted Java files match the original and documented repository versions after CRLF line endings are normalised to LF |
| Original artifacts | The submitted ZIP and report are preserved byte-for-byte; seven inherited class files also match the original repository |
| Documentation review | Commands, source links and behavioral descriptions checked against the committed implementation; no source changes or additional program tests |

No measured speedup, fixed loading time or predictive-accuracy benchmark is established by this archive. The [submission guide](docs/FINAL_SUBMISSION.md) records dates, hashes, comparison scope and the class-file compatibility finding.

## Limitations

- Loading and processing run on Swing's event-dispatch thread, so a long load blocks UI interaction.
- First-letter hashing can produce long chains; insertion and lookup costs depend on chain length. Prediction scans stored entries rather than performing a constant-time next-word lookup.
- The UI's **Load Factor** is bucket occupancy, not the conventional distinct-entry load factor. Histogram statistics describe the unigram table only.
- The model has no smoothing, backoff, random sampling, sentence-boundary handling or held-out evaluation pipeline. Accepted tokens can form n-grams across line breaks and skipped tokens.
- The submitted archive contains no automated test suite, pinned toolchain or separate benchmark records. The original compiled classes require Java 25 unless rebuilt from source.

## Attribution and provenance

Coursework by **Yongjiang Liu**, officially submitted on **17 December 2025**. The original source, report, corpus, instructor brief and submitted ZIP remain preserved; archive documentation does not replace the original submission.

The instructor brief and corpus retain their original provenance, and the submitted report remains unmodified. No repository-wide redistribution license is added. See [Final submission and recovery](docs/FINAL_SUBMISSION.md) for the detailed record.

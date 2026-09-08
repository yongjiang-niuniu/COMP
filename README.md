# Statistical Language Model in Java

A Java Swing coursework application for **COM6516 Object Oriented Programming**. It builds unigram, bigram, and trigram frequency tables from a text file, displays vocabulary and hash-table statistics, and generates text by repeatedly selecting the most frequent continuation.

The project explores how custom hash tables, collision chains, and hash-function choices affect a small language-model application.

## Features

- Custom array-backed hash tables with alphabetically ordered linked collision chains.
- Two selectable hashing strategies: polynomial hashing and first-letter hashing.
- Vocabulary sorting by word or frequency.
- A histogram of unigram collision-chain lengths and summary statistics.
- Bigram and trigram text continuation, adding up to 20 words when matching contexts exist.
- Input checking that accepts lowercase letters, periods, and apostrophes after lowercasing, and skips invalid whitespace-delimited tokens.

The n-gram storage is implemented in `MyHashTable` and `MyLinkedObject`. The GUI and statistics code also use Java collections, including `ArrayList` and `TreeMap`.

## Build and run

Use JDK 17 or later, as specified in the original project instructions, and a desktop environment capable of displaying Swing windows. No external Java libraries are required.

From the repository root:

```bash
mkdir -p build
javac -d build code/*.java
java -cp build MyLanguageModel
```

1. Keep **Polynomial Hash** selected for an initial run.
2. Click **Load news.txt** and select a text file in the file chooser. The repository's sample is `news.txt` at the root.
3. Inspect the vocabulary table, sort controls, histogram, and statistics.
4. Enter at least one word for **Predict (Bigram)** or at least two words for **Predict (Trigram)**.
5. To compare hash functions, change the selector and load the file again. The selected strategy is applied when the file is loaded.

The repository also contains historical `.class` files. The commands above compile from source into a separate directory and avoid depending on those binaries.

## How prediction works

For a given one-word or two-word context, the application scans the relevant n-gram table and selects the continuation with the largest observed count. It repeats until 20 words have been added or no continuation is found.

This is a deterministic frequency-based demonstration. It does not implement neural modeling, smoothing, random sampling, or a held-out evaluation pipeline.

## Repository map

| Path | Purpose |
| --- | --- |
| `code/MyLanguageModel.java` | Swing UI, file processing, statistics, and text continuation |
| `code/MyHashTable.java` | Bucket array and frequency-table operations |
| `code/MyLinkedObject.java` | Linked collision chains and counts |
| `code/MyHashFunction.java` | Base class for hash strategies |
| `code/PolynomialHashFunction.java` | Polynomial recurrence `h = 31 * h + c` |
| `code/FirstLetterHashFunction.java` | First-character hashing for comparison |
| `code/HistogramPanel.java` | Collision-distribution display |
| `news.txt` | Existing sample corpus |
| `assignment.pdf` | Original coursework brief |

See [design notes and behavior limits](docs/DESIGN.md) for the data structures, performance tradeoffs, and interpretation of the displayed statistics.

## Performance and scope

First-letter hashing places words beginning with the same character in the same bucket. Long chains make insertion and lookup slower, and loading runs on Swing's UI thread, so a large file can make the window temporarily unresponsive. Runtime depends on the corpus and hardware; this repository does not contain benchmark evidence for a fixed loading time or guaranteed constant-time behavior.

The project is preserved as coursework by **Yongjiang Liu**. The included assignment brief and corpus retain their original provenance; no repository-wide license is included.

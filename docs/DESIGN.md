# Design and implementation notes

These notes describe the committed Java sources.

## Text processing

`MyLanguageModel.processFile` lowercases each line, splits on whitespace, and accepts tokens matching letters `a`–`z`, periods, or apostrophes. It shows one warning for the first invalid token and skips invalid tokens thereafter. Punctuation is not generally stripped or converted into sentence boundaries.

The previous two accepted tokens are retained across line breaks. Unigrams, bigrams, and trigrams can therefore span lines, and skipping an invalid token can connect accepted words on either side of it. All three tables use 5,000 buckets.

## Frequency tables

`MyHashTable` stores an array of `MyLinkedObject` chain heads. New entries start at count 1; a repeated key increments its count. Chains are maintained in alphabetical order, including insertion before the head when necessary.

`MyLinkedObject.setWord`, `isWord`, and `getCount(String)` use loops to traverse chains. Its diagnostic `toString()` still calls the next node's `toString()` recursively, so the implementation should not be described as entirely free of recursion.

The application uses these custom structures for n-gram storage. `ArrayList`, `TreeMap`, and sorting helpers are used separately for the GUI and summary calculations.

## Hashing and performance

Polynomial hashing combines all characters using `h = 31 * h + c`, then takes the absolute remainder modulo the bucket count. First-letter hashing uses only the first character's remainder.

Both strategies can collide. Hashing a key with the polynomial strategy takes work proportional to the key length; insertion or lookup also depends on chain length. First-letter hashing deliberately creates heavily concentrated chains for ordinary English text. Inserting many distinct entries into a long ordered chain can have quadratic total cost in the number of entries involved. No measured speedup or fixed wall-clock time is established here.

Loading and processing run in the button event handler on Swing's event-dispatch thread. A long load blocks UI interaction until processing returns.

## What the statistics show

The graph and statistics are computed from the **unigram** table.

- **Used Slots** counts nonempty buckets.
- The UI label **Load Factor** reports `nonempty buckets / total buckets * 100`. This is bucket occupancy, rather than the conventional hash-table load factor `distinct entries / buckets`.
- **Avg List Len** is the number of distinct unigram entries divided by all 5,000 buckets, including empty buckets.
- **Std Deviation** is the population standard deviation of those bucket-chain lengths.

Sorting the vocabulary changes its display order and does not change the hash table.

## Prediction behavior

Bigram prediction uses the last input word; trigram prediction uses the last two. For each generated word, `findMostLikelyNext` scans the target table for matching prefixes and selects the highest-count suffix. If counts tie, the first encountered candidate remains selected.

There is no fallback from a missing trigram context to a bigram context, no smoothing, and no sentence-boundary handling. Each next-word search scans the stored entries, so prediction is not a constant-time hash lookup.

## Documentation verification

The README and these notes were checked against the repository's Java source. They correct the earlier claim that the entire application avoids the Java Collections Framework and remove unsupported timing and distribution guarantees. Documentation maintenance did not change application behavior.

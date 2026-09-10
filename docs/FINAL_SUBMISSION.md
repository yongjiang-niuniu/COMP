# Official submission and recovery

Blackboard lists **Programming Assignment, Attempt 1** as submitted on **17 December 2025 at 17:17 (UTC+8)**. The recovered original attachment is `elp25aai.zip`. The content entry was closed, but the original submitted attempt remained available through its View button.

The [original ZIP](../archive/blackboard/elp25aai.zip) is unchanged. Its 17 files contain seven Java sources, seven compiled classes, a sample corpus, the submitted README and a three-page student report. The report is also available at [reports/elp25aai.pdf](../reports/elp25aai.pdf) for direct reading. The [submission record](../archive/blackboard/submission_record.json) retains the source URL, attempt identifiers and individual file checksums.

## What the comparison established

The submitted files were compared with the original repository commit `4596d8867a7ced9994ec9b2e90e702a61c38f09d` and the pre-integration main commit `77be1409b5c595a9b9fa7d7579f4ba2738f6d6ef`.

| Material | Result |
| --- | --- |
| Seven Java source files | Identical after CRLF line endings are normalized to LF; no other source differences. |
| Seven compiled classes | Byte-identical. Original binaries remain unchanged. |
| Sample corpus | Submitted `code/news.txt` and repository `news.txt` match after CRLF normalization. |
| Submitted README | Matches the original repository README after CRLF normalization. Later repository documentation clarifies collections usage and performance limits. |
| Student final report | Newly recovered `elp25aai.pdf`, three pages including screenshots, design discussion, references and AI-use disclosure. |
| Instructor brief | Existing `assignment.pdf` is an eight-page assignment brief, a different document from the student report. |

All ZIP members passed CRC verification and extracted-file checksum checks. The three student-report pages were rendered and visually reviewed. The report's source document, an automated test suite, a locked toolchain and separate benchmark result records were not present in the submitted ZIP.

## Compilation and evidence

The source imports JDK libraries only and uses Swing for the desktop interface. The submitted documentation specifies JDK 17 or higher. The inherited binary headers instead declare class-file version 69.0, which requires Java 25 class-file support; Java 17 supports through major version 61. This is documented by [Oracle's Java Virtual Machine Specification, Table 1.2-A](https://docs.oracle.com/javase/specs/jvms/se25/html/jvms-1.html).

Use the README's `javac --release 17 -d build code/*.java` command to compile source into a separate directory before launching the application. This avoids relying on the preserved Java 25 binaries and leaves them unchanged. A working JDK was unavailable during this recovery, so source compilation and GUI execution were not performed. The submitted report's performance descriptions remain historical claims, not newly reproduced measurements.

## Retention and attribution

This repository preserves Yongjiang Liu's coursework and its original history. Course instructions and the corpus keep their existing provenance; no repository-wide redistribution license is added. The original report, including its disclosure and identifiers, is retained without edits. No grades, feedback, quiz answers or unsubmitted drafts were added.

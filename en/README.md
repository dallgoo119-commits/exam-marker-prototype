# Identifying exam questions in submitted photos

[한국어](../README.md) · [English](../en/README.md) · [简体中文](../zh-CN/README.md) · [日本語](../ja/README.md)

[Print the two-page PDF](design.pdf) · [Open the editable HTML source](design.html)

| Page 1 · Exam paper design | Page 2 · Verification workflow |
| --- | --- |
| [![QR codes inside and below the passage and beside five answer choices](exam-sheet.png)](exam-sheet.png) | [![Proposed workflow for the exam authority and service provider](architecture.png)](architecture.png) |

An exam authority prints one QR code inside the passage, one directly below it and one beside each of the five answer choices. When a user submits a photo, a participating AI service could read a code and check whether the exam is currently protected. The prototype includes a printable sample question and seven working QR codes. The second page illustrates the proposed registry, signature checks, account records and service integration.

## Try the exam paper

Print the PDF on A4 paper at 100% scale. The question uses 11 pt body text. Each QR code measures about 11 mm on each side, including its white border. A QR reader can decode the sample strings offline. The code inside the passage reads `DEMO-Q01-T`. The code below the passage reads `DEMO-Q01-P`. The five choices carry `DEMO-Q01-A1` through `DEMO-Q01-A5`.

## How a participating service would verify a photo

The authority assigns a short identifier to each marked position. It records the exam session, question, position, and start and end of the protection period, then digitally signs the registry record. To distinguish individual printed copies, the authority could issue additional copy IDs and maintain a distribution record. The sample codes contain only question and position identifiers.

The service scans a submitted image for one or more codes. It looks up each identifier in a trusted issuer’s registry, verifies the record’s signature using the issuer’s public key and checks the protection period. Under the agreed policy, it could notify the user, restrict the answer or send the case for review. The design distinguishes an unreadable code, an unknown ID, a failed signature check and an expired protection period. The signature authenticates the registry record. A code can be copied to another document, so a reviewer may also need to match the photographed question to the registered item.

For a later review, authorized staff could look up the event time and an internal reference to the submitting account. The issuer and provider would first agree on access rights, retention periods and disclosure procedures for distribution and account records. Identifying the person who took a photograph requires additional evidence.

## Digital checks and print trial

Each language edition fits on two A4 pages. We rendered the first page of each PDF in grayscale at 225 dpi. ZXing-C++ decoded all seven identifiers in every full-page image. OpenCV decoded all seven individual marker crops. Its multi-code detector returned zero on the same full-page images. In a crop containing only the passage, excluding the marker below it and the answer choices, both ZXing-C++ and OpenCV decoded the in-text identifier `DEMO-Q01-T` in every edition. Read rates for phone photos of printed copies still need to be measured.

For the print trial, photograph the whole question, the passage alone and a selection of answer choices. Record phone model, distance, angle, lighting and image compression for each condition. Count all photos, photos containing a complete marker, and photos with at least one valid decoded ID. The second count divided by the first gives marker inclusion. The third divided by the first gives overall detection. Also test ordinary documents for false positives and record processing time, print quality and readability.

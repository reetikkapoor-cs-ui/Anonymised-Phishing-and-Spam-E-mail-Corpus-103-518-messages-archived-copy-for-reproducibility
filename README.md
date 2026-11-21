# Anonymised Phishing and Spam E-mail Corpus (103,518 messages)


## Provenance

The file was obtained from a public Kaggle dataset listing whose original URL could not be recovered. The uploader did not create, collect or label this data, and claims no authorship of it. If you are the original creator, please get in touch so that your original release can be credited and linked.

## Contents

One CSV file, `D3.csv` (125,384,426 bytes), with two columns:

| Column | Description |
|---|---|
| `text` | E-mail content (subject and body). Personal information is replaced by placeholders such as `[NAME]`, `[ORGANIZATION]`, `<\|URL\|>` and `<\|EMOJI\|>`, which appear in about 84% of messages. |
| `label` | `0` = legitimate, `1` = malicious |

## Statistics

| Property | Value |
|---|---|
| Total messages | 103,518 |
| Legitimate (`label = 0`) | 57,911 |
| Malicious (`label = 1`) | 45,607 |
| Empty `text` fields | 1 |
| Duplicate texts | 0 |

## Note on labels

The positive class mixes phishing and spam. The labels therefore describe malicious-versus-legitimate e-mail, not phishing alone.

## Use in the associated study

A stratified random sample of 20,000 messages (8,811 positive) was drawn with scikit-learn's `train_test_split` (`random_state=42`). Text was represented with TF-IDF (unigrams and bigrams) followed by truncated SVD.

## Integrity

SHA-256:

```
14735feaefda952a64c1939f92e24c5316d8f5ea468c8ea920ea1e02216045ea
```

## Licence

The original licence is unknown. The data is shared for research use only; the original creators retain all rights.

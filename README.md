# AI Protein Property Predictor

Predicts whether a **human protein is a membrane protein** (yes / no) from its amino acid sequence.
Project B21 | FAIR-AI-21 | PBL

## Idea
A protein is a "word" written with 20 amino-acid letters. Membrane proteins live in the oily cell
membrane, so they contain more oil-loving (hydrophobic) letters such as L, I, V, F. We count the
letters, train a model, and check whether it learns this pattern.

## Data
- UniProt Swiss-Prot (reviewed) human proteins, downloaded through the UniProt REST API
- Label: 1 if the "subcellular location" text contains "membrane", otherwise 0
- 20,431 downloaded, 17,342 used (9,389 not membrane / 7,953 membrane)
- UniProt is updated regularly, so counts may change slightly on a new download

## Method
1. Features: fraction of each of the 20 amino acids + sequence length (21 numbers)
2. Fair split: GroupShuffleSplit by first Pfam family (similar proteins stay on one side)
3. Models: Logistic Regression (with scaling) and Random Forest, class_weight = balanced
4. Evaluation: accuracy, precision, recall, F1, confusion matrix, feature importance

## Results (test set, 4,486 proteins)
| Model | Accuracy | F1 (membrane) |
|---|---|---|
| Logistic Regression | 0.775 | 0.733 |
| Random Forest | 0.790 | 0.721 |

Random split vs family split (Random Forest F1): 0.720 vs 0.721.

## How to run
1. Open `protein_predictor.ipynb` in Google Colab (or run `protein_predictor.py` locally)
2. Run all cells (internet needed for the UniProt download)
3. Outputs: confusion matrix images, `feature_importance.png`, `uniprot_human_raw.csv`

Local install: `pip install -r requirements.txt`

## Reproducibility
Random seed fixed to 42; raw data saved as CSV; single notebook runs top to bottom.

## Limitations
Labels come from a text search (noisy); composition ignores letter order; human proteins only;
models were not tuned.

## Future work
ESM-2 embeddings, cleaner transmembrane labels, BLAST / CD-HIT similarity checks.

## 1. Overview
This repository contains the Seccoll collection for the NER task in the information security domain and related materials.
**ATTENTION! Dividing the dataset into folds used in the papers will be added soon**

## 2. RuCyBERT Model
The RuCyBERT model trained on Russian information security news over 70 epoches can be obtained from https://drive.google.com/file/d/1JEbXaQjNd51vtaIGjw3MHr60qWgxQzbH/view?usp=sharing

| Label | RuBERT F1 span | RuCyBERT F1 span |
|-|-|-|
| DEVICE | 0.429 | **0.535** |
| EVENT | 0.662 | **0.688** |
| HACKER | 0.589 | **0.684** |
| LOC | **0.911** | **0.911** |
| ORG | 0.792 | **0.808** |
| PER | 0.838 | **0.863** |
| PROGRAM | 0.654 | **0.683** |
| TECH | 0.673 | **0.712** |
| VIRUS | 0.459 | **0.613** |
| F1-micro | 0.718 | **0.752** |
| F1-macro | 0.667 | **0.723** |


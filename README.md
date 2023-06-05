# Federated Learning with Clara Train SDK

## Outline
- Federated Learning Introduction
- Setup Guide
- Architecture
- Dataset
- Model
- Aggregation Schemes
- Proposed Aggregation Scheme
- References

---

## Federated Learning Introduction
Through FILE, a brief introduction about federated learning is presented.

---

## Setup Guide
Through FILE, a detailed setup guide and walkthrough of the development is presented.

---

## Architecture
Three clients each represents a hospital.
Split the data over the 3 clients (50, 25, 25)

---

## Dataset
Spleen Data Set from Medical Segmentation Decathlon was used. This dataset has the following details:
- Target: Spleen
- Modality: CT
- Size: 61 3D volumes (41 Training + 20 Testing)
- Source: Memorial Sloan Kettering Cancer Center
- Challenge: Large ranging foreground size
[available here](https://github.com/NVIDIA/clara-train-examples/blob/5967cbbd051566596b6a5c363f06002ac9f234c5/PyTorch/NoteBooks/Data/DownloadDecathlonDataSet.ipynb)

## Model
The clara_pt_spleen_ct_segmentation model was used.

## Aggregation Schemes
- FedAvg
- FedMA

## Proposed Aggregation Scheme


## References

# Federated Learning with Clara Train SDK

## Outline
- Introduction
- Federated Learning Introduction
- Setup Guide
- Architecture
- Dataset
- Model
- Aggregation Schemes
- References

## Introduction
The task is to segment CT spleen images using a federated learning global model. The global model is generated using three different models which are aggregated. Each model is pre-trained and fine-tuned separately on each client's side. Afterwards, the used aggregation scheme is evaluated, in this case we are using two aggregation schemes `FedAvg` and `FedMA`. A next step would be introducing a new scheme.

---

## Federated Learning Introduction
A brief introduction about federated learning is presented [here](./fl_with_clara_train_sdk.md).

---

## Setup Guide
A detailed setup guide and walkthrough of the development is presented [here](./setup_guide.md).

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
The clara_pt_spleen_ct_segmentation model was used at each client's side and fine-tuned.

## Aggregation Schemes
- `FedAvg`: uses a weighted average of the local models updates.
- `FedMA`: uses learned features from the local models, like gradients or weights.

## References

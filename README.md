<img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=Python&logoColor=white"/>

# Vision-LLM System for Industrial Anomaly Detection & Reporting

- Title : Decoupled Perception-Reasoning LLM-based Industrial Assembly Process Inspection System

- Author : 손형오, 김예석, 김경원, 권용현, 김재원, 김경민, 김영균

- Paper Link :

# Code Description

YOLO_ResNet_train : Training & Testing Vision Model (YOLO-ResNet) 

LLM_final : Integration and Test File for Vision Model and LLM

# Download Fine-tuned Models

https://drive.google.com/drive/folders/12jxk1qCEuXGFxVvPF5-fMPewubyg870D?usp=drive_link

YOLO : yolo_train_results_v12

ResNet : best_classification_resnet_finetuned.pth

# Dataset

<img width="1917" height="1078" alt="image" src="https://github.com/user-attachments/assets/4f5c94d2-0256-4977-ae0b-3219ceabaf52" />

This project utilizes a high-resolution image dataset designed to diagnose the condition of assembly modules equipped with air fittings (pneumatic tube connectors) in industrial manufacturing environments. The original dataset is based on the "Air Fitting Assembly Status Data" provided by AI Hub (National Information Society Agency).

* Dataset Link : https://www.aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&dataSetSn=71927

# Proposed Framework

<img width="1917" height="1078" alt="image" src="https://github.com/user-attachments/assets/e2d31947-a8bf-4de6-879b-047112cb2e18" />

1. Perception (Edge Vision AI): A lightweight vision model (YOLO + ResNet) detects the Region of Interest (ROI) and classifies the assembly status (e.g., normal, loose, missing) in real-time.

2. Context Conversion: The visual detection metadata is translated into a structured natural language prompt.

3. Reasoning (RAG + LLM): The Large Language Model retrieves relevant past maintenance logs from a Vector DB (ChromaDB) via semantic search to analyze the root cause.

4. Report Generation: The system finally generates an explainable and actionable diagnostic report, providing field workers with clear maintenance guidelines.

# Vision Model Architecture

<img width="1917" height="1078" alt="image" src="https://github.com/user-attachments/assets/54553230-0157-4801-80b6-72e52cf3a594" />

Our architecture utilizes lightweight vision expert models for real-time

- YOLOv12 (Part Detection): Rapidly and robustly detects air fitting components within complex backgrounds. To prevent the loss of fine defect features, it extracts the Region of Interest (ROI) with a 20% margin.

- ResNet-18 (Status Classification & XAI): Processes the extracted ROI to classify the assembly status into four categories: Normal, Partial, Missing, and Error. Furthermore, it integrates the Grad-CAM mechanism to highlight the decisive defect areas, providing explainable visual evidence that helps the LLM accurately understand the anomaly.

# RAG-based Report Generation

<img width="1917" height="1078" alt="image" src="https://github.com/user-attachments/assets/5870cc45-cd91-4948-89b7-0347da8182f6" />

To address domain knowledge limitations and prevent LLM hallucinations, the system employs a Retrieval-Augmented Generation (RAG) framework.

* Embedding Model : jhgan/ko-sroberta-multitask

* LLM : yanolja/EEVE-Korean-Instruct-10.8B-v1.0

* Vector Database (ChromaDB): Historical maintenance logs and manuals are embedded into high-dimensional vectors and stored in ChromaDB for semantic search.
  
* Context Retrieval: Upon detecting an anomaly, the system calculates the cosine similarity to instantly retrieve the top 3 most contextually relevant maintenance records.
  
* Reporting: The LLM synthesizes the visual detection metadata (from YOLO-ResNet) with the retrieved knowledge to generate a highly reliable diagnostic report. This report provides field workers with the root cause and specific, actionable maintenance guidelines.

## Real-Time Reliability Evaluation (RAGAS)

<img width="1917" height="1078" alt="image" src="https://github.com/user-attachments/assets/76be6695-4407-4ac5-9a7e-9567ed04910b" />

To ensure the trustworthiness of the AI-generated guidelines, the system integrates the RAGAS (Retrieval Augmented Generation Assessment) framework for concurrent evaluation.

*   Faithfulness: Verifies that the LLM's output is strictly derived from the retrieved historical logs, effectively preventing hallucination.

*   Answer Relevancy: Quantifies how directly and intuitively the generated diagnostic report addresses the identified anomaly.

*   Automated Background Scoring: The pipeline calculates these metrics in real-time as the report is generated, displaying quantitative confidence scores to the field workers to ensure operational reliability.

# Vision Classification Test Result

<img width="1917" height="1078" alt="image" src="https://github.com/user-attachments/assets/b1bcf88f-bdb6-4b61-84b2-53bacb352c0a" />

# Diagnosis Report

<img width="1917" height="1078" alt="image" src="https://github.com/user-attachments/assets/4eb5ed82-5f26-4ce4-a034-919eaaa9d435" />

# References

- Embedding model : https://huggingface.co/jhgan/ko-sroberta-multitask

- LLM : https://huggingface.co/yanolja/YanoljaNEXT-EEVE-Instruct-10.8B

- YOLO : Terven, J., and Córdova-Esparza, D., “A Comprehensive Review of YOLO Architectures in Computer Vision: From YOLOv1 to YOLOv8 and YOLO-NAS”, arXiv:2304.00501, 2023. (https://arxiv.org/abs/2304.00501)

- ResNet : Kaiming He, et al., “Deep residual learning for image recognition”,  IEEE conference on computer vision and pattern recognition, USA, 2016. (https://arxiv.org/abs/1512.03385)

- RAG : Patrick Lewis, et al., “Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks”, NeurIPS 2020, Canada, 2020. (https://arxiv.org/abs/2005.11401)

- RAGAS : Shahul Es, et al., “Ragas: Automated Evaluation of Retrieval Augmented Generation”, EACL 2024, Malta, 2024. (https://arxiv.org/abs/2309.15217)

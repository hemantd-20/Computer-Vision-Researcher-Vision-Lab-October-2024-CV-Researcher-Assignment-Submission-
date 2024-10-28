DINO: DETR with Improved DeNoising Anchor Boxes for End-to-End Object Detection
Project Overview
This project explores and experiments with DINO (DETR with Improved DeNoising Anchor Boxes). The goal is to evaluate pretrained DINO models and fine-tune them for a custom object detection task.

Objectives:
Dataset preparation
Repository setup and fetching pretrained weights
Evaluation of the pretrained model
Fine-tuning the model
Visualizing attention maps
Dataset
The dataset used is in COCO format:

201 images in total: 161 for training and 40 for validation.
Annotations are provided in .json format.
Directory structure:

markdown
Copy code
COCODIR/
└── annotations/
    ├── instances_train2017.json
    ├── instances_val2017.json
├── train2017/
└── val2017/
Model Setup and Weights
The repository for DINO was cloned from IDEA-Research/DINO, and pretrained DINO 4-Scale weights were downloaded.

Evaluation of the pretrained model was conducted using the 36-epoch model trained on the COCO dataset.

Results and Analysis
Pretrained Model Performance:
Average Precision (AP) @ 0.5: 0.88 (high precision)
AP @[0.5:0.95]: 0.56
Recall: 0.71
Losses:
Overall Loss: 7.00
Classification Loss: 0.517
Bounding Box Loss: 0.100
GIoU Loss: 0.446
Challenges:
Good performance for medium and large objects but struggled with smaller objects.
Distant objects and overlapping/multiple bounding boxes presented difficulties.
Fine-tuning
The model was fine-tuned on the custom dataset by modifying the configuration:

Set num_classes = 2 (for a single class detection task).
Fine-tuned for 12 epochs (also tested for 20 epochs).
Checkpoints were saved in the Logs/dino/R50-MS4/ directory.
Attention Map Visualization
To visualize attention maps:

Cross attention weights from the DeformableTransformerDecoderLayer class were modified.
Cross attention focuses on object detection, while self-attention gives a general view of the image.

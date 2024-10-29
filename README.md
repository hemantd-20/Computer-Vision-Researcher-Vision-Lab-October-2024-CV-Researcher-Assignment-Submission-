# 🚀 **Exploring DINO: DETR with Improved DeNoising Anchor Boxes for End-to-End Object Detection**

## 🎯 **Objective**  
This project explores and experiments with [DINO (DETR with Improved DeNoising Anchor Boxes)](https://github.com/IDEA-Research/DINO.git). The objective is to evaluate pretrained DINO models, fine-tune them for a custom object detection task (specifically, person detection), and generate attention maps overlaid on the detections by extracting cross-attention maps.

## 🗂️ **Dataset Preparation**  
The dataset contains 201 images, divided into 161 images for training and 40 images for validation. Below is the directory structure used:

```
COCODIR/
   ├── annotations/
   │   ├── instances_train2017.json
   │   └── instances_val2017.json
   ├── train2017/
   └── val2017/
```

*(Note: The dataset follows the naming conventions of the COCO dataset but is custom, containing images and annotations for only one class—person.)*

## 🏋️‍♂️ **Pretrained Weights Used**  
The pretrained weights used for this project are based on the **DINO-4scale (36 epoch setting)** model with an R50 (ResNet50) backbone, achieving a Box AP of 50.9.

## 📊 **Evaluation on Pretrained Weights**  
Evaluation was conducted smoothly on the pretrained weights to gain insights into the model's performance. 

### Modifications for Evaluation:  
While evaluating on Google Colab, we encountered a **CUDA 'Out of Memory'** error. To resolve this, we eliminated 2 channels from the list in `test.py` to run the evaluation script efficiently.

Path: `DINO/models/dino/ops/test.py`  
![test.py Modification](https://github.com/user-attachments/assets/79e7d29d-411e-4cc3-b028-6a8ac6b4b990)

## 📑 **Report and Analysis**  
For detailed analysis and evaluation metrics, refer to the [Project Report](https://github.com/hemantd-20/Computer-Vision-Researcher-Vision-Lab-October-2024-CV-Researcher-Assignment-Submission-/blob/main/DINO_Report.pdf).

![Project Report](https://github.com/user-attachments/assets/58007420-e139-4e81-a445-d609f81daf69)

## 🛠️ **Fine-tuning**  
For fine-tuning on the **DINO 4-Scale model** pretrained weights, we first modified the configuration file:

- **Config File** (`config.py`):  
  Since the dataset contains only one class, `num_classes` was set to `2` (1 class + 1 for increment).

Command for fine-tuning:

```
--pretrain_model_path /content/drive/MyDrive/DINO/checkpoint0033_4scale.pth \
--finetune_ignore label_enc.weight class_embed \
--options num_classes=2
```

- **dn_labelbook_size**: Ensure `dn_labelbook_size >= num_classes + 1`.
- **Epochs**: Initially trained for 12 epochs, and later for 20 epochs.
- **Modifications to `coco_id2name.json`**: Since training only involves one class.

![Config Modification](https://github.com/user-attachments/assets/d95c6b73-ec8f-42f0-b990-331f38dcb5b9)

### **Saved Checkpoints**  
The fine-tuned model checkpoints can be found in the `Logs/` directory:  
`Logs/dino/R50-MS4/checkpoint_best_regular.pth`

Training and fine-tuning logs are also available in the same directory.

## 🧪 **Testing the Fine-tuned Model**  
A set of 45 images was collected for testing the model on diverse and interesting test cases. The test images can be found in the `DINO/Test_Images/` folder.

![Test Image 1](https://github.com/user-attachments/assets/6eb03e86-d54a-4249-9ede-0d129a5ba7f2)  
![Test Image 2](https://github.com/user-attachments/assets/a958b89b-91fc-4e90-9f44-3b4a84c52090)  
![Test Image 3](https://github.com/user-attachments/assets/f15c3dd8-d7fb-4f33-9d05-4d08ba29c222)  
![Test Image 4](https://github.com/user-attachments/assets/5e6937d3-b224-4f35-93ac-ecdfa4af0bd7)  
![Test Image 5](https://github.com/user-attachments/assets/6a734974-1466-446c-b7f1-2b72b204a310)

## 🔍 **Attention Maps**  
To visualize the attention maps, we extracted cross-attention weights from the decoder layer. Cross-attention weights highlight detection-relevant features, while self-attention offers a more generalized view of the image.

### **Code Modifications for Attention Maps**  
To obtain attention maps, we modified the following classes and methods:

- **MSDeformAttn Class**: Modified the `forward()` method to capture attention weights.
![Attention Modification 1](https://github.com/user-attachments/assets/9202480c-9dc8-44f4-8a9d-ef606bff5c25)

- **DeformableTransformerDecoderLayer Class**: Return the cross-attention weights from here.
![Attention Modification 2](https://github.com/user-attachments/assets/408be76d-439f-460b-97ff-82c9849f098a)

Other modifications were made in classes such as **DeformableTransformer** and **TransformerDecoder**.

For a complete list of changes, refer to the modified files in the repository.

### **Attention Map Results**  
The visualizations of the attention maps can be found below:

![Attention Map](https://github.com/user-attachments/assets/26fd76b3-22e4-406d-a83a-6b5b46837812)

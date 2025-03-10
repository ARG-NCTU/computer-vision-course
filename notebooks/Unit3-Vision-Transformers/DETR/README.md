# Boat Dataset Processing with DETR

This repository provides a workflow for downloading, processing, and uploading the **Boat Dataset** for object detection using **DEtection TRansformer (DETR)**. The instructions below guide you through setting up the environment, downloading necessary files, preprocessing data, and running inference.

## How to run

Clone the repo
```bash
git clone git@github.com:ARG-NCTU/computer-vision-course.git
```

or if you do not have ssh key yet (you should set it up soon)
```bash
git clone https://github.com/ARG-NCTU/computer-vision-course.git
```

Enter the repo:
```bash
cd computer-vision-course
```

### GPU Usage

Update the docker image:
```bash
docker pull argnctu/oop:gpu
```

For your first docker terminal:
```bash
source gpu_run.sh
```

More terminal:
```bash
source gpu_join.sh
```

### LAB4 Guide

#### 1. Navigate to the DETR Directory
```bash
cd ~/computer-vision-course/notebooks/Unit3-Vision-Transformers/DETR
```

#### 2. Authenticate with Hugging Face
```bash
huggingface-cli login
```

#### 3. Download Boat Dataset
```bash
huggingface-cli download SIS-2024-spring/Boat_dataset --repo-type dataset --local-dir ./Boat_dataset
```

#### 4. Extract Images
```bash
cd ~/computer-vision-course/notebooks/Unit3-Vision-Transformers/DETR/Boat_dataset/data
unzip images.zip
```

#### 5. Convert JSONL to Parquet
```bash
cd ~/computer-vision-course/notebooks/Unit3-Vision-Transformers/DETR
python3 jsonl2parquet.py
```

#### 6. Upload Processed Dataset to Hugging Face Hub
```bash
huggingface-cli upload zhuchi76/Boat_real_dataset_2023 ./Boat_dataset/data/instances_train2023r.parquet ./data/instances_train2023r.parquet --repo-type=dataset --commit-message="Upload training images and labels to hub"

huggingface-cli upload zhuchi76/Boat_real_dataset_2023 ./Boat_dataset/data/instances_val2023r.parquet ./data/instances_val2023r.parquet --repo-type=dataset --commit-message="Upload val images and labels to hub"

huggingface-cli upload zhuchi76/Boat_real_dataset_2023 ./Boat_dataset/data/images.zip ./data/images.zip --repo-type=dataset --commit-message="Upload images to hub"

huggingface-cli upload zhuchi76/Boat_real_dataset_2023 ./Boat_dataset/data/classes.txt ./data/classes.txt --repo-type=dataset --commit-message="Upload class labels to hub"

huggingface-cli upload zhuchi76/Boat_real_dataset_2023 ./Boat_dataset/data/source_video.zip ./data/source_video.zip --repo-type=dataset --commit-message="Upload inferenced videos to hub"
```

#### 7. Download Dataset and Unzip Files
```bash
mkdir -p data
huggingface-cli download zhuchi76/Boat_real_dataset_2023 data/images.zip --repo-type dataset --local-dir ./
unzip ./data/images.zip -d ./

huggingface-cli download zhuchi76/Boat_real_dataset_2023 data/classes.txt --repo-type dataset --local-dir ./

huggingface-cli download zhuchi76/Boat_real_dataset_2023 data/source_video.zip --repo-type dataset --local-dir ./
unzip ./data/source_video.zip -d ./
```

#### 8. Start Jupyter Notebook For DETR notebook
```bash
cd ~/computer-vision-course/
source jupyter_notebook.sh
```
This lab will use notebooks/Unit3-Vision-Transformers/DETR/lab-detr.ipynb for DETR training, evaluation and inferencing.

#### 9. Run DETR Video Inference
```bash
python3 detr_video_inference.py -i source_video/WAM_V_1.mp4 -o out_video/WAM_V_1_out.mp4
```

### Output
- Processed images and labels are stored in `Boat_dataset_2023/data/`
- Inference results are saved in `out_video/`

### Notes
- Ensure you have the necessary permissions to access and upload datasets on Hugging Face.
- Modify dataset paths as needed based on your working environment.

### License
This project is intended for research and educational purposes.
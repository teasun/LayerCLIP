# LayerCLIP: A Fine-Grained Class Activation Map for Weakly Supervised Semantic Segmentation

## Preparing Datasets
### PASCAL VOC2012
The structure of `/your_home_dir/datasets/VOC2012`should be organized as follows:

```
---VOC2012/
       --Annotations
       --ImageSets
       --JPEGImages
       --SegmentationClass
       --SegmentationClassAug
```

### MS COCO2014
The structure of `/your_home_dir/datasets/COCO2014`are suggested to be organized as follows:
```
---COCO2014/
       --Annotations
       --JPEGImages
           -train2014
           -val2014
       --SegmentationClass
```

### Preparing pre-trained model
Download CLIP pre-trained [ViT-B/16] at [here](https://openaipublic.azureedge.net/clip/models/5806e77cd80f8b59890b7e101eabd078d9fb84e6937f9e85e4ecb61988df416f/ViT-B-16.pt) and put it to `/your_home_dir/pretrained_models/clip`.

## Usage
### Step 1. Generate CAMs for train (train_aug) set.
```
# For VOC12
python generate_cams_voc12.py --img_root /your_home_dir/datasets/VOC2012/JPEGImages --split_file ./voc12/train_aug.txt --model /your_home_dir/pretrained_models/clip/ViT-B-16.pt --cam_out_dir ./output/voc12/cams

# For COCO14
python generate_cams_coco14.py --img_root /your_home_dir/datasets/COCO2014/JPEGImages/train2014 --split_file ./coco14/train.txt --model /your_home_dir/pretrained_models/clip/ViT-B-16.pt --cam_out_dir ./output/coco14/cams
```
### Step 2. use CRF process to generate pseudo masks 
```

## for VOC12 
python eval_cam_with_crf.py --cam_out_dir ./output/voc12/cams --gt_root /your_home_dir/datasets/VOC2012/SegmentationClassAug --image_root /your_home_dir/datasets/VOC2012/JPEGImages --split_file ./voc12/train_aug.txt --pseudo_mask_save_path ./output/voc12/pseudo_masks
## for COCO14
python eval_cam_with_crf.py --cam_out_dir ./output/coco14/cams --gt_root /your_home_dir/datasets/COCO2014/SegmentationClass --image_root /your_home_dir/datasets/COCO2014/JPEGImages/train2014 --split_file ./coco14/train.txt --pseudo_mask_save_path ./output/coco2014/pseudo_masks


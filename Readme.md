# **Evaluate-Anyformat：A Universal Evaluation Protocal Adapter Hub**

## **Project Overview**

**Evaluate-AnyFormat** is an under-development **evaluation protocol adapter hub** designed to standardize performance assessment across different prediction formats in computer vision tasks.

### Goal

* **Unified Evaluation**: Support any format of ground truth (GT) and predictions
* **Flexible Benchmarking**: Compute standard metrics (e.g. mAP, AR, IoU) and generate visualizations.
* **Framework-Agnostic**: Compatible with outputs from MMLab, Ultralytics, Detectron2 and other CV frameworks

### Version Update

v0.0.0: uncompleted, but mAPeva.py has some usable functions, follow the uncommented parts in this file.

### Usable Functions

v0.0.0: mAPeva.py: gen_hbb_coco_GT_json and convert function can convert a yolo dataset format to COCO, and corresponding eval function is usable. As for obb, there're some bugs which will lead to a very low mAP.

## **Motivation**

Modern computer vision research involves diverse frameworks (e.g., MMlab series, ultralytics), each producing **different output formats**. This creates friction when verifying novel models' effect upon different datasets, especially for those which has to be evaluated on both HBB and OBB datasets.

## **Dataset File Format Introduction**

### COCO

Many datasets nowadays won't provide a coco format dataset, which organize files in this format:

```plaintext
.  
├── annotations/  
│   ├── instances_train2017.json  
│   └── instances_val2017.json  
├── train2017/  
│   ├── 000000000009.jpg  
│   └── 000000000025.jpg  
└── val2017/  
│   ├── 000000000123.jpg  
│   └── 000000000134.jpg
└── test2017/
    └── 000000000001.jpg  
```

As for the COCO format JSON file, its format is as follow(some sub dict is not necessary as annotation(Acutally is Ground Truth JSON) in `class COCO(annotation='path')`, like "year" in "info"， but "iscrowd" in "annotations" is, if you're using COCOeval in `pycocotools`):

```json
{
  "info": {"description": "COCO 2017 Dataset", ...},
  "licenses": [{"id": 1, 
                "name": "CC BY-NC-SA 2.0",
                ...}],
  "categories": [{"id": 1, 
                  "name": "person",
                  "supercategory": "human"},
                  ...],
  "images": [{"id": 1, 
              "file_name": "000000000009.jpg",
              "width": 640,
              "height": 480},
              ...],
  "annotations": [{"id": 1, 
                   "image_id": 1, 
                   "category_id": 1, 
                   "bbox": [x, y, w, h], # using absolute coordinate
                   "area": w*h, 
                   "iscrowd": 0
                   ...},
                  {"id": 2,
                   ...
                    }
                 ...]
}
```

When using package Pycocotools, an annotation file could be loaded in a COCO class as ground truth:

```python
from pycocotools.coco import COCO
from pycocotools.cocoeval import COCOeval
coco_gt = COCO("/path_to_trainOrval.json")
coco_pre = coco_gt.loadRes("/path_to_prediction_result.json")
coco_eval = COCOeval(coco_gt, coco_pre, "bbox")
```

and when loading predictions' JSON file for evaluation, only annotations is neccessary in its json file.

Ultrelytics can generate COCOeval format result json by using `model.val(save_json=True)`.

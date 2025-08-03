# Evaluate-Anyformat：An under-developing evaluation protocal adapter hub

The inception of this project stems from my experience assisting with a paper research, where the use of multiple frameworks like MMRotate and Ultralytics for multimodal tasks resulted in prediction outputs of varying formats, creating significant evaluation challenges.

## Initial idea and completed functions

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

As for the JSON file, its format is as follow:

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
                   "bbox": [x,y,w,h], 
                   "area": w*h, 
                   ...},
                  {"id": 2,
                   ...
                    }
                 ...]
}
```

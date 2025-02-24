
## Setup environment:


1. I suggest using pytorch docker as a base environment

```docker run nvcr.io/nvidia/pytorch:24.08-py3```

2. Within the container clone the repository 

```
git clone https://github.com/ChristofHenkel/kaggle-cryo-1st-place-segmentation
cd kaggle-cryo-1st-place-segmentation
```


3. And install necessary pip packages via 

```pip install -r requirements.txt```

## Required Data

download the Cryo ET competition data directly from kaggle: https://www.kaggle.com/competitions/czii-cryo-et-object-identification/data

or use the kaggle API
```kaggle competitions download -c czii-cryo-et-object-identification```

and specify path to it in ```configs/common_config.py``` with ```cfg.data_folder```



## Training models

To train checkpoints necessary for replicating the segmentation part of the 1st place solution run training of 2x fullfits for each model. Thereby ```cfg.fold = -1``` results in training on all data, and using ```fold 0``` as validation. By default models are trained using bfloat16 which requires a GPU capable of that. Alternatively you can set ```cfg.bf16=False``` or overwrite as flag ```--bf16 False``` when running ```train.py ```.
```
python train.py -C cfg_resnet34 --fold -1
python train.py -C cfg_resnet34 --fold -1
python train.py -C cfg_resnet34_ds --fold -1
python train.py -C cfg_resnet34_ds --fold -1
python train.py -C cfg_effnetb3 --fold -1
python train.py -C cfg_effnetb3 --fold -1
```

## Inference

Inference after models are converted with torch jit is shown in our 1st place submission kaggle kernel.

https://www.kaggle.com/code/christofhenkel/cryo-et-1st-place-solution?scriptVersionId=223259615
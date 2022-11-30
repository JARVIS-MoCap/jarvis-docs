# Training HybridNet

## Setting the config parameters
Jarvis tries to derive a reasonable set of config parameters by analyzing your trainingset. This is not always perfect though and you will often find yourself modifying the <span style="color:#63a31f">config.yaml</span> inside your project directory. This section aims to describe and illustrate all the non self-explanatory parameters.

``` yaml title="ExampleProject config.yaml"
#General Configuration
DATALOADER_NUM_WORKERS: 4       #Number of threads used for dataloading

#Dataset Configuration
DATASET:
  DATASET_2D: Example_Dataset   #2D dataset path (usually same as DATASET_3D)
  DATASET_3D: Example_Dataset   #3D dataset path

#EfficientTrack 2D Center Detector Configuration:
CENTERDETECT:
  MODEL_SIZE: 'small'           #Can be 'small', 'medium' or 'large'
  BATCH_SIZE: 8                 #Set to 4 for very small datasets (<500 Frames) 
  MAX_LEARNING_RATE: 0.01       #Max learning rate in OneCycle schedule
  NUM_EPOCHS: 50                #Set to 100 for very small datasets
  CHECKPOINT_SAVE_INTERVAL: 10  #Saves a .pth checkpoint ever N epochs
  IMAGE_SIZE: 256               #Frames get resized to NxN

#EfficientTrack 2D Keypoint Detector Configuration
KEYPOINTDETECT:
  MODEL_SIZE: 'small'           #Can be 'small', 'medium' or 'large'
  BATCH_SIZE: 8                 #Set to 4 for very small datasets (<500 Frames) 
  MAX_LEARNING_RATE: 0.01       #Max learning rate in OneCycle schedule
  NUM_EPOCHS: 100               #Set to 200 for very small datasets
  CHECKPOINT_SAVE_INTERVAL: 10  #Saves a .pth checkpoint ever N epochs
  BOUNDING_BOX_SIZE: 256        #Size of the crop around the subject that gets 
                                # fed into KeypointDetect (1)
  NUM_JOINTS: 23                #Number of keypoints (Don't change!)

#hybridNet Configuration
HYBRIDNET:
  BATCH_SIZE: 1                 #Currently only batch size 1 is supported
  MAX_LEARNING_RATE: 0.003      #Max learning rate in OneCycle schedule
  NUM_EPOCHS: 30                #Set to 60 for very small datasets (<500 Frames)
  CHECKPOINT_SAVE_INTERVAL: 10  #Saves a .pth checkpoint ever N epochs
  NUM_CAMERAS: 12               #Number fo cameras (Don't change!)
  ROI_CUBE_SIZE: 144            #Size of the 3D bounding box in mm (2)
  GRID_SPACING: 2               #Resolution of the 3D bounding box in mm 

KEYPOINT_NAMES:                 #List of all keypoint names (for visualization)
- Pinky_T
- Pinky_D
...

SKELETON:                   #List of all joints (for visualization only)
- - Pinky_T
  - Pinky_D
- - Pinky_D
  - Pinky_M
- - Pinky_M
...
 
```

1. ![Bounding Box 2D](../assets/images/manual/bounding_box_2D.png){: .center width="100%"}
2. ![Bounding Box 3D](../assets/images/manual/bounding_box_3D.png){: .center width="100%"}

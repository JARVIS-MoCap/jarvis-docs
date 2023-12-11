# Creating and Labeling Datasets
The AnnotationTool lets you create Datasets from your recordings, calibrate your cameras, annotate your Datasets, and finally export Trainingsets for training the HybridNet!


![AnnotationTool](../assets/images/manual/AnnotationTool.png){: .center width="60%"}


The following will show you how to use all parts of the **AnnotationTool**. There are four main functions:

- <span style="color:#63a31f">Create new Dataset</span>: This lets you create Datasets from AcquisitionTool Recordings. This is also where you will define the Keypoints and Skeleton you want to annotate.
- <span style="color:#63a31f">Create new Calibration</span>: This lets you create a set of calibration parameters from a set of checkerboard recordings
- <span style="color:#63a31f">Annotate Dataset</span>: This lets you load and annotate a dataset
- <span style="color:#63a31f">Export Trainingset</span>: Once you are finished annotating a dataset this lets you export it as a Trainingset, the format used by the HybridNet Pytorch module.

## Create new Dataset

![Create Dataset](../assets/images/manual/CreateDatasetOverview.png){: .center width="60%"}

- <span style="color:#a31f00">**(1)**</span>: Configuration for the Dataset to be created:
  - <span style="color:#63a31f">New Dataset Name</span>: Name of the dataset to be created. A directory with the same name will be created
  - <span style="color:#63a31f">New Dataset Path</span>: Path where the new directory for the dataset will be created. This should **not** include the new datasets name!
  - <span style="color:#63a31f">Framesets to extract per Segment</span>: Number of FrameSets to be created per Segment (See **(2)** details about Segments). One FrameSet is a set of images from **all** cameras, this is not the number of images! The total number of FrameSets extracted per recording should not exceed 100 unless it is a very long recording.
  - <span style="color:#63a31f">Sampling Method</span>: Method used to extract the dataset. Select <span style="color:#63a31f">kmeans</span> for high dataset quality and <span style="color:#63a31f">uniform</span> for extraction speed. <span style="color:#63a31f">uniform</span> is only recommended for quick test runs, not final datasets.
- <span style="color:#a31f00">**(2)**</span>: Module to add Recordings to the dataset and define Segments within them.
  - The <span style="color:#63a31f">Add Recording Button</span> lets you add a directory containing multi-camera recordings. The directory name will be used as the name of the recording, the filenames of the videos will be used as the camera names. Only add recordings with identical camera configurations and calibrations here!
  - Click the <span style="color:#63a31f">Scissors</span> button to open the <span style="color:#63a31f">Segmentation Module</span> for a recording (see **[section below](#segmentation-module)** for more details)
- <span style="color:#a31f00">**(3)**</span>: Entities Input - This is a future-prrofing feature for potential multi-entity tracking capabilites. Simply add a single entity (e.g. "Monkey", or "Hand") here using the Add button.
- <span style="color:#a31f00">**(4)**</span>: Keypoint Input - Add the names of all the keypoints you want to label here using the Add button (e.g. "Right Elbow", "Right Wrist" etc.). You can change the order of the keypoints using the arrow buttons. The order will be used in all subsequent steps.
- <span style="color:#a31f00">**(5)**</span>: Skeleton Input - This step is optional! Add a skeleton connection and it's measured length (e.g. "Right Lower Arm", connecting the "Right Elbow" and "Right Wrist" keypoints). This will be used for visualization purposes as well as for showing the deviation between the measured lengths and the ones calculated for each FrameSet.
- <span style="color:#a31f00">**(6)**</span>: Save and load Presets using these buttons. This is **highly** recommended, to make sure no typos ruin the compatability between two datasets with the same Keypoint configuration. A <span style="color:#63a31f">Hand</span> and <span style="color:#63a31f">Rodent Body</span> preset are provided. You can also import a configuration from a already existing dataset. 

## Segmentation Module
The Segmenation Module allows you to define which parts of a recording will be used to create the dataset to be labeled. This is a critical step in cases where there are long phases of inactivity between the relevant activities (e.g. long resting phases between grasps in our monkey experiments). As mentioned above an equal number of FrameSets is extracted for each segment. 

!!! warning "Segments with the same name are treated as one disjunct segment!"

![Segmentation Overview](../assets/images/manual/SegmentationOverview.png){: .center width="60%"}



- <span style="color:#a31f00">**(1)**</span>: Editor Timeline - Scrub through the video using the green cursor, zoom in and out using your mouse wheel and drag the gray bar to get to different parts of the video. 
- <span style="color:#a31f00">**(2)**</span>: Click this button to add a segment. The segment will show up in **(3)** and a blue `Range Cursor` will appear in the timeline. Drag it around to select start and endpoint of the video and click `Add` to finalize the segment.
- <span style="color:#a31f00">**(3)**</span>: Segments list - Doule click a segment to change it's name or delete it using the red X button. Segments with identical names will be treated as one disjunct segment.
- <span style="color:#a31f00">**(4)**</span>: Double click a camera to change the view displayed in the editor.
- <span style="color:#a31f00">**(5)**</span>: Save and load segmentations as `.csv` files. This is strongly recommended as redoing a segmenation can be a lot of work. Also save periodically to avoid dataloss in case of any crashes.
- Click continue once you are happy with your segmenation, your segmenation should be displayed next to your recording now.

## Create new Calibration
This module lets you create calibration parameters (in the form of `.yaml` files) from a set of calibration recordings. For more information on camera calibration in general, the [OpenCV Documentation](https://docs.opencv.org/4.x/dc/dbb/tutorial_py_calibration.html) is a good start. Check out [this Section]() for instructions on how to record calibration videos

![Calibration Overview](../assets/images/manual/Calibration_Overview.png){: .center width="60%"}



- <span style="color:#a31f00">**(1)**</span>: Configuration:

    - **General**:
          - <span style="color:#63a31f">Calibration Set Name</span>: Select the name of your new set of calibration parameters. This will be the name of the directory they are stored in.
          - <span style="color:#63a31f">Calibration Set Savepath</span>: Path where the new directory will be created.
          - <span style="color:#63a31f">Seperate Recodrings for Intrinsics</span>: Select wether you have seperate recordings for the intrinsic parameter calibration. If you do the format should be the following: One directory containing one video per camera, the filename has to be identical to the name of the camera. If you select "<span style="color:#63a31f">No</span>" the Extrinisics recordings will be used for intrinsics calibration, this is generally **not** recommended.
          - <span style="color:#63a31f">Intrinsics Folder Path</span> and <span style="color:#63a31f">Extrinsics Folder Path</span> are the path to the the intrinsics and extrensics recordings mentioned above. See the <span style="color:#63a31f">Examples</span> folder for example calibration recordings.
          - <span style="color:#63a31f">Update Camera Names</span>: Click this after configuring all parameters in the <span style="color:#63a31f">General</span> section. This will automatically detect all you Cameras and available Camera Pairs. **Important**: This will include all possible Camera Pairs, make sure you modify them as described in section <span style="color:#a31f00">**(3)**</span> below!
    - **Calibration Settings**:
          - <span style="color:#63a31f">Max. Number of Frames for Intrinsics/Extrinsics Calibration</span> should generally be left at the default value
          - 'Save Debug Images' saves images of all detected Checkerboards in a <span style="color:#63a31f">Debug</span> folder along with your calibration parameters. This is very usefull for debugging a bad calibration.
    - **Checkerboard Layout**: 
          - <span style="color:#63a31f">Board Type</span>: Select wether you are using a normal checkerboard (Standard) or a ChArUco board. Normal checkerboard calibration is more precise and is recommended.
          - <span style="color:#63a31f">Pattern Width</span> and <span style="color:#63a31f">Pattern Height</span> let you define the size of your checkerboard. **Important**: Make sure your checkerboard matches the generated image exactly, the way the pattern size is defined is not intuitive.
          - <span style="color:#63a31f">Side Length [mm]</span> lets you set the size of **a single** checkerboard square. Make sure this is accurate, as this defines the overall scale of the 3D reconstruction.
  
- <span style="color:#a31f00">**(2)**</span>: Cameras - This will give you a list of all cameras that will be calibratet. Make sure all cameras are present, the order does not matter.
- <span style="color:#a31f00">**(3)**</span>: Camera Pairs - This lists all configured Camera Pairs for extrinsic calibration. Make sure that the first camera listed (the **primary** camera) is the same for all datasets. You can modify a Camera Pair by double clicking it.
- Click the <span style="color:#63a31f">Calibrate</span> button after you are done configuring your calibration run. This will take a while. After it is done you should see an overview of your calibration run:
  
![Calibration Success](../assets/images/manual/Calibration_Success.png){: .center width="60%"}


  All `Reprojection Erros` should be below a pixel at the very most. Generally a lower value is better and they should be fairly equal across all cameras and pairs.
- If the calibration does not succeed or the reprojection Error is too high, first ensure that the checkerboard parameters are setup correctly. Next check the debug images to see if there are any issues with lighting or motion blur. If none of this helps, redo the calibration videos.

!!! warning "If you are using three-camera chains during calibration it is expected that the reprojection error for those pairs is slighly higher than all others."


## Annotate Dataset

![Annotation Overview](../assets/images/manual/AnnotationOverview.jpg){: .center width="70%"}


- <span style="color:#a31f00">**(1)**</span>: Dataset Control - This lets you select the segment to be annotated in the dropdown at the top. Double clicking a camera in the list below selects the cameras frame for annotation.
- <span style="color:#a31f00">**(2)**</span>: This is the main editor, the controls are as follows:

    - <span style="color:#63a31f">Right Mouse Button</span>: Place the active keypoint
    - <span style="color:#63a31f">Left Mouse Button</span>: Drag and move an existing keypoint
    - <span style="color:#63a31f">Middle Mouse Button</span>: Delete an existing annotation
    - <span style="color:#63a31f">Mouse Wheel</span>: Zoom in and out
    - <span style="color:#63a31f">Ctrl + Left Mouse button</span>: Pan the image
    - <span style="color:#63a31f">Arrow Key Right</span>: Change to next camera view
    - <span style="color:#63a31f">Arrow Key Left</span>: Change to previous camera view
    - <span style="color:#63a31f">Arrow Key Down</span>: Change to next FrameSet
    - <span style="color:#63a31f">Arrow Key Up</span>: Change to previous set
  
- The <span style="color:#63a31f">active Keypoint</span> is indicated by the green highlighting in <span style="color:#a31f00">**(3)**</span>, you can change it by clicking any of the keypoints in the list.
- A transparent annotation marker indicates that a keypoint is reprojected (automtically placed) and a solid annotation marker indicates that a marker is placed manually.
- Similarly a purple checkmark in <span style="color:#a31f00">**(3)**</span> indicates a reprojected keypoint, a blue checkmark indicates a manually annotated keypoint.
- <span style="color:#a31f00">**(4)**</span>: **ReprojectionTool** - this displays the **reprojection error** for each joint that is annotated in at least two different camera views. Make sure these values stay as low as possible, and don't exceed ~5 pixels. 
- <span style="color:#a31f00">**(5)**</span>: Buttons to switch between the different camera views
- <span style="color:#a31f00">**(6)**</span>: Buttons to Crop and Pan the editor view, <span style="color:#63a31f">Home</span> resets the view
- <span style="color:#a31f00">**(7)**</span>: Buttons to switch between FrameSets
- <span style="color:#a31f00">**(8)**</span>: Viewer Button - Opens the <span style="color:#63a31f">**3D Viewer Window**</span> that shows a 3D reconstruction of all annotated keypoints. If a skeleton was defined during dataset creation, this will also be rendered here. 
  
![3DViewer](../assets/images/manual/3DViewer.png){: .center width="40%"}

!!! warning "On some computers this causes a memory leak, due to a bug in the underlying library, if you experience sporadic crashes try avoiding the 3D viewer."


## Export TrainingSet

![Export Trainingset Overview](../assets/images/manual/TrainingSetOverview.jpg){: .center width="70%"}


- <span style="color:#a31f00">**(1)**</span>: Configuration:
  
    - <span style="color:#63a31f">TrainingSet Name</span>: This is the name of the directory that will be created for the new Trainingset.
    - <span style="color:#63a31f">Trainingset Savepath</span>: Path where the new Trainingset directory will be created
    - <span style="color:#63a31f">Trainingset Type</span>: Set wether to include calibrations in your dataset (3D). This should in be left at 3D for almost all usecases.
    - <span style="color:#63a31f">Validation Split Fraction</span>: This sets the fraction of the data that is used as validation data during training. If this is set to 0.1, 90% of the annotated FrameSets will be used for training and 10% for validation.
    - <span style="color:#63a31f">Shuffle before Train/Validation Split</span>: Select wether or not your FrameSets should be shuffled before splitting them into train and val splits. This should always be enabled.
    - <span style="color:#63a31f">Random Shuffle Seed</span>: Enable this if you need a truly random shuffle. Generally should be set to <span style="color:#63a31f">No</span>.
    - <span style="color:#63a31f">Shuffle Seed</span>: Selects the seed for the dataset shuffling. This allows you to recreate a train/val split after modifying some annotations.

- <span style="color:#a31f00">**(2)**</span>: Datasets: This let's you select multiple annotated Datasets to combine into one Trainingset. They can use different calibrations, but the number and names of cameras have to be identical across all combined datasets. Double clicking a dataset allows you to exclude individual segments.
- <span style="color:#a31f00">**(3)**</span>: Entities: This can be ignored, as only single entity tracking is currently implemented.
- <span style="color:#a31f00">**(4)**</span>: Keypoints: Lets you exclude individual keypoints from the created Trainingset.
- <span style="color:#a31f00">**(5)**</span>: Summary: Shows an overview of the composition of your Trainingset. Hover over a segment in the leftmost piechart to get details about the composition of individual datasets.
- <span style="color:#a31f00">**(6)**</span>: Buttons to save and load your settings as a preset, this is helpfull if you continously modify datasets and want to update your trainingsets. 

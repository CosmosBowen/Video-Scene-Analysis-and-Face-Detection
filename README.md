# Video Scene Analysis and Face Detection

A Python tool for splitting videos into scenes and detecting target human faces in videos.

* `refenrence_face.jpg` [<a href="https://lh3.googleusercontent.com/-bFf6KPCwpiPVvXDX5Zx_Z2PgJYIqm52hWyDvCl3cTEj-lQ_aY2vRg7kalqsQ1uP9KaGZgkv9ECnT9B1uJMqZNwT2piWKS2ySCRuoQdF4VrRfu82ig=w1440">source</a>]
<p align="center">
  <img src="https://lh3.googleusercontent.com/-bFf6KPCwpiPVvXDX5Zx_Z2PgJYIqm52hWyDvCl3cTEj-lQ_aY2vRg7kalqsQ1uP9KaGZgkv9ECnT9B1uJMqZNwT2piWKS2ySCRuoQdF4VrRfu82ig=w1440" width="350" title="refenrence face">
</p>

* `video.mp4` [<a href="https://www.youtube.com/watch?v=J5XFYMsczy8">source</a>]
  * Downloaded using command `yt-dlp` on YouTube Video ID `J5XFYMsczy8` in notebook cell:
``` bash
!yt-dlp -f 'bestvideo[ext=mp4][vcodec^=avc]+bestaudio[ext=m4a]' J5XFYMsczy8 -o project/source/video.mp4
```
<p align="center">
  <img src="https://img.youtube.com/vi/J5XFYMsczy8/maxresdefault.jpg" width="350" title="video">
</p>

https://github.com/user-attachments/assets/2f147782-a292-499d-9e94-1c3e613c9003


* Example pairs of detection results:
  * Left: frames where faces were detected
  * Right: same frames with bounding box visualization
<p align="center">
  <img src="frame.png" width="350" alt="frame.png">
  <img src="marked_frame.png" width="350" alt="marked_frame.png">
  
  <img src="https://github.com/user-attachments/assets/295d1b49-e2aa-42f3-9907-b2647421a44b" width="350" alt="frame.png">
  <img src="https://github.com/user-attachments/assets/72c5a76f-ddd5-499a-8e98-c5a1439bce3a" width="350" alt="marked_frame.png">
  
  <img src="https://github.com/user-attachments/assets/b3b81924-1fe7-40f0-8903-d9d761b1c45f" width="350" alt="frame.png">
  <img src="https://github.com/user-attachments/assets/b81373a0-d6c4-400d-bac7-aef898e78db1" width="350" alt="marked_frame.png">
</p>

# What you will get from all of these

✌️ No need to rush! I recommend you to pause here, go get a glimpse into the results and all possible detailed running outputs from 3 detection scenarios:
  * Folder `project/results`
  * Notebook `project/scripts/detect_outputs_from_3_scenarios.ipynb`
  * Each detection's results saved in a unique folder, named after the combination of `reference_face_name` and `video_name` : `project/results/<reference_face_name>_<video_name>/`
  
# Setup Instructions

## 1. Clone this repository:
```bash
git clone <repository-url>
cd project
```

## 2. Create and set up the environment in terminal:

1. Create Environment

Check if conda is installed:
```bash
conda -V
```

Then run:
```bash
conda create --name face_detection python=3.10.12
conda activate face_detection
```

2. Install Dependencies
```bash
# First, install the base requirements
pip install -r requirements.txt

# Install OpenCV through conda-forge
conda install -c conda-forge opencv

# Install scene detection
pip install scenedetect
```

## 3. Set up Jupyter Notebook in VS Code:
1. Open VS Code and navigate to the project folder `code .`
2. Open `detect.ipynb`
3. Click on `"Select Kernel"` in the top-right corner
<p align="center">
  <img src="https://github.com/user-attachments/assets/426e9176-81de-44a6-b008-c93499fece8b" width="700" title="refenrence face">
</p>

5. Click on `"Python Environments"`
<p align="center"> 
  <img src="https://github.com/user-attachments/assets/045f8b0a-c588-4dfe-9928-1923b108d6bc" width="600" title="refenrence face">
</p>

6. Select the `face_detection` conda environment we just created
<p align="center"> 
  <img src="https://github.com/user-attachments/assets/d1bd4e7d-dbba-4498-a550-5b74a3f9df46" width="600" title="refenrence face">
</p>

7. The kernel should now be ready to run the notebook

# Project Structure
   * Before Detection
```
project/
├── scripts/
│   ├── detect.ipynb        
│   └── detect_outputs_from_3_cases.ipynb 
├── source/
│   ├── video.mp4     
│   └── reference_face.jpg   
├── requirements.txt
└── README.md
```

   * After Detection
```
project/
├── results/
│   └── reference_face_video
│       ├── cropped_videos
│       │   ├── marked
│       │   │   ├── scene_1.mp4
│       │   │   └── scene_3.mp4
│       │   └── normal
│       │       ├── scene_1.mp4
│       │       └── scene_3.mp4
│       ├── detect_scenes
│       │   ├── scene_1.mp4
│       │   ├── scene_2.mp4
│       │   └── scene_3.mp4
│       ├── full_clip_marked.mp4
│       ├── full_clip.mp4
│       └── metadata.json
├── scripts/
│   └── detect.ipynb
│   └── detect_outputs_from_3_cases.ipynb
├── source/
│   ├── video.mp4
│   └── reference_face.jpg
├── requirements.txt
└── README.md
```

# Instructions to Run
1. Open `detect.ipynb` in Jupyter Notebook
2. Click `"Run All"` to run all cells
3. Then to run detection on your custom image and video:
    * Place your reference image and video into folder `project/source`
    * Modify `reference_face_name`, `video_name`, `tolerance`, and run only the last cell:

```python
run_detect(
    reference_face_name =  "<your_reference_face>.jpg", 
    video_name = "<your_video>.mp4", 
    tolerance = 0.679, 
    show_process = # True for detailed output
)  
```
The results will be saved in `project/results/<your_reference_face>_<your_video>/` directory.



# Assumptions and Design Choices

### Target Face Detection
* Only detects "visible" faces (>1% of image area)
* Takes detected face with maximum area as target face
* Assumes target face is dominant in reference image

### Scene Detection
* Uses scene-detect library's built-in algorithm
* Splits videos at points of significant visual change
* Maintains visual consistency within scenes

### Face Matching
* Recommended tolerance range: 0.5 to 0.65
* Tolerance of 0.5 provides optimal balance:
   * High recall (detects most target faces)
   * Good precision (minimal false positives)
* Maximum useful tolerance: 0.679
   * Can detect younger/older versions of target
   * May introduce few false positives
   * Maintains differentiation from opposite gender


# Advantages
### Continuous Detection Results
* Combines all detected frames into two summary videos:
    * `full_clip.mp4`: Continuous sequence of frames where faces were detected
    * `full_clip_marked.mp4`: Same frames with bounding box visualization
        * Red boxes: Target faces
        * Black boxes: Other detected faces
* Note:
  * These videos contain only frames with detected faces, not the complete original video
  * Only if video contain target face, will two summary videos be generated

###  Organized Output Structure
* Maintains pairs of scene clips:
    * Original clips in /cropped_videos/normal/
    * Visualization clips in /cropped_videos/marked/
* All outputs organized by reference face and source video:
```
project/results/<reference_face_name>_<video_name>/
├── cropped_videos/
│   ├── normal/
│   │   └── scene_N.mp4    # Original frames
│   └── marked/
│       └── scene_N.mp4    # With bounding boxes
```

### Detailed Analysis
* Metadata captures detailed face detection information that allows analyzing:
    * When target faces appear in scenes
    * Number of detected faces in each frame
    * Face location and size changes over time
    * Frame-by-frame detection tracking
* Metadata Structure
    * `frame_id`: Frame number in the scene
    * `num_detected_faces`: Total detected target faces in frame
    * `bbox`: Bounding box coordinates, each element is a bounding box coordinate `[x, y, width, height]`
    * `fps`: Frames per second
    * `width`: Video width in pixels
    * `height`: Video height in pixels
``` json
{
    "clips": [
        {
            "file_name": "scene_5.mp4",
            "start_timecode": "00:00:06.798", 
            "end_timecode": "00:00:08.217",
            "target_face_coordinates": [
                {
                    "frame_id": 6,  
                    "num_detected_faces": 1,  
                    "bbox": [  
                        [803, 204, 385, 386]  
                    ]
                }
            ]
        }
    ],
    "video_properties": {
        "fps": 23.97,  
        "width": 1920, 
        "height": 1080 
    }
}
```



# Limitations

### Technical Limitations
* No audio in processed video clips
* Only processes MP4 format videos
* Can only detect target face from human, not from other beings, including animals and views, objects
* No systematic method for optimal tolerance value
* Limited optimization for large-scale processing
* The same video will be split again if run detect with a different reference face image

### Performance Considerations
* Processing speed depends on:
   * Video length and complexity
   * Number of detected scenes
   * Number of frames containing faces to analyze
* Storage requirements:
   * Scene-detect library stores all intermediate scenes locally
   * Each split scene saved as separate MP4 file
   * Additional storage for face detection results

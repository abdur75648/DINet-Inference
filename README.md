# DINet: Deformation Inpainting Network for Realistic Face Visually Dubbing on High Resolution Video (AAAI2023)

A well-documented version of the DINet model for inference on custom videos.
This repo provides a step-by-step guide to run the DINet model either on a local machine or on Google Colab.

## Evironment Setup
1. Clone this repository:
```bash
https://github.com/abdur75648/DINet.git
cd DINet
```

2. Create a conda environment with python==3.7 and install all dependencies:
```bash
conda create -n dinet python==3.7 -y
conda activate dinet
pip install -r requirements.txt
```
*If running in Google Colab, you will first need to install Miniconda and then install the packages in the requirements.txt file. The steps are as follows:*
```bash
!wget https://repo.anaconda.com/miniconda/Miniconda3-py37_4.8.2-Linux-x86_64.sh
!chmod +x Miniconda3-py37_4.8.2-Linux-x86_64.sh
!bash ./Miniconda3-py37_4.8.2-Linux-x86_64.sh -b -f -p /usr/local
import sys
sys.path.append('/usr/local/lib/python3.8/site-packages/')
!conda install python=3.7 -y
!python --version
!conda create --name dinet python=3.7 -y
!source activate dinet
!pip install -r requirements.txt
```

3. Download the model and sample data (asserts.zip) in [Google drive](https://drive.google.com/uc\?id\=1CkeEn7l3PuubuJIMWNjWpIrt0HDd_AB3), unzip and put it in the root directory.
```bash
gdown --id 1CkeEn7l3PuubuJIMWNjWpIrt0HDd_AB3
unzip asserts.zip
rm asserts.zip
```

## Inference
* For running inference on the example videos, run the following command:
```bash
python inference.py --mouth_region_size 256 --source_video_path test.mp4 --source_openface_landmark_path test.csv --driving_audio_path test.wav --res_video_dir test_output/ --pretrained_clip_DINet_path ./asserts/clip_training_DINet_256mouth.pth
```

## Additional Setup Notes
### OpenFace

The `.csv` file should contain the facial landmarks detected using [OpenFace](https://github.com/TadasBaltrusaitis/OpenFace). Installing OpenFace from scratch can be challenging due to dependency issues. Therefore, it is recommended to use Docker for a quicker setup. Follow these steps for quickstart usage of OpenFace with Docker:

1. Run the OpenFace Docker container:
    ```bash
    docker run -it --rm algebr/openface:latest
    ```
2. Find the container ID by running this (*in a different terminal*):
    ```bash
    docker ps
    ```
    (Let's say it shows `a52fea727822`)

3. Transfer any video you want to run OpenFace on:
    ```bash
    docker cp test.mp4 a52fea727822:/home/openface-build
    ```
4. In the first shell (*where the container is running*), run OpenFace on the video:
    ```bash
    build/bin/FaceLandmarkVidMulti -f test.mp4 -2Dfp
    ```
5. The output will be saved as `test.csv` in the `processed` directory. Transfer it back to your local machine (*in a different terminal*):
    ```bash
    docker cp a52fea727822:/home/openface-build/processed/test.csv .
    ```

* Note that we are using `FaceLandmarkVidMulti` here for videos, than `FaceLandmarkVid` (For images, use `FaceLandmarkImg`). This is because `FaceLandmarkVid` requires a display for its operations, which is not available in a Docker container by default. If you need to use `FaceLandmarkVid`, you can set up X11 forwarding on your local machine to enable the display (Ref. [this issue](https://github.com/TadasBaltrusaitis/OpenFace/issues/1076#issuecomment-2192538076)).

### FFMPEG
Ensure that FFMPEG is installed on your system to enable audio and video merging functionality in the DINet model. 

To install FFMPEG, if you have root access to your system, run the following command:

    ```bash
    sudo apt-get install ffmpeg
    ```

If you don't have root access, follow the instructions below to install FFMPEG statically in your root directory:
1. **Download the Correct Static Build For Your System:** (Architecture can be checked using ```uname -m``` command)
    ```bash
    # For i686:
    wget -O ffmpeg.tar.xz https://johnvansickle.com/ffmpeg/builds/ffmpeg-git-i686-static.tar.xz
    # For x86_64 or amd64:
    wget -O ffmpeg.tar.xz https://johnvansickle.com/ffmpeg/builds/ffmpeg-git-amd64-static.tar.xz
    # For arm64 or aarch64:
    wget -O ffmpeg.tar.xz https://johnvansickle.com/ffmpeg/builds/ffmpeg-git-arm64-static.tar.xz
    ```

2. **Unzip and Unpack the Build:**
    ```bash
    tar xvf ffmpeg.tar.xz
    rm ffmpeg.tar.xz
    ```

4. **Go to the unzipped directory:** (Naming convention is ```ffmpeg-git-[YYYYMMDD]-[platform]-static```)
    ```bash
    cd ffmpeg-git-*-static
    ```

3. **Check the Installation:**
    ```bash
    ./ffmpeg -version
    ```

4. **Move the Binary to the root Directory:**
    ```bash
    cd ..
    mv ffmpeg DINet/
    ```

## Acknowledge
This code is taken from the original repository of [DINet](https://github.com/MRzzm/DINet)
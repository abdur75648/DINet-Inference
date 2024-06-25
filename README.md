# DINet: Deformation Inpainting Network for Realistic Face Visually Dubbing on High Resolution Video (AAAI2023)
![在这里插入图片描述](https://img-blog.csdnimg.cn/178c6b3ec0074af7a2dcc9ef26450e75.png)
[Paper](https://fuxivirtualhuman.github.io/pdf/AAAI2023_FaceDubbing.pdf) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;     [demo video](https://www.youtube.com/watch?v=UU344T-9h7M&t=6s)  &nbsp;&nbsp;&nbsp;&nbsp; Supplementary materials

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
python inference.py --mouth_region_size=256 --source_video_path=./asserts/examples/test1.mp4 --source_openface_landmark_path=./asserts/examples/test1.csv --driving_audio_path=./asserts/examples/driving_audio_1.wav --pretrained_clip_DINet_path=./asserts/clip_training_DINet_256mouth.pth
```

* For running inference on custom videos, run the following command:
```bash
python inference.py --mouth_region_size=256 --source_video_path=xxx.mp4 --source_openface_landmark_path=xxx.csv --driving_audio_path=xxx.wav --pretrained_clip_DINet_path=./asserts/clip_training_DINet_256mouth.pth
```

## Notes
### OpenFace
The ```.csv``` file should contain the facial landmarks detected using [openface](https://github.com/TadasBaltrusaitis/OpenFace)

* Below is a step-by-step guide to extract the facial landmarks using OpenFace:
1. **Clone the OpenFace repository:**
    ```bash
    git clone https://github.com/TadasBaltrusaitis/OpenFace.git
    cd OpenFace
    ```
TO-BE-CONTINUED...

### FFMPEG
Ensure that FFMPEG is installed on your system to enable audio and video merging functionality in the DINet model. 

* To install FFMPEG, if you have root access to your system, run the following command:
    ```bash
    sudo apt-get install ffmpeg
    ```

* If you don't have root access, follow the instructions below to install FFMPEG statically in your root directory:
- **Download the Correct Static Build For Your System:** (Architecture can be checked using ```uname -m``` command)
    ```bash
    # For i686:
    wget -O ffmpeg.tar.xz https://johnvansickle.com/ffmpeg/builds/ffmpeg-git-i686-static.tar.xz
    # For x86_64 or amd64:
    wget -O ffmpeg.tar.xz https://johnvansickle.com/ffmpeg/builds/ffmpeg-git-amd64-static.tar.xz
    # For arm64 or aarch64:
    wget -O ffmpeg.tar.xz https://johnvansickle.com/ffmpeg/builds/ffmpeg-git-arm64-static.tar.xz
    ```

3. **Unzip and Unpack the Build:**
    ```bash
    tar xvf ffmpeg.tar.xz
    rm ffmpeg.tar.xz
    ```

4. **Go to the unzipped directory:** (Naming convention is ```ffmpeg-git-[YYYYMMDD]-[platform]-static```)
    ```bash
    cd ffmpeg-git-*-static
    ```

5. **Check the Installation:**
    ```bash
    ./ffmpeg -version
    ```

6. **Move the Binary to the root Directory:**
    ```bash
    cd ..
    mv ffmpeg DINet/
    ```

## Acknowledge
This code is taken from the original repository of [DINet](https://github.com/MRzzm/DINet)
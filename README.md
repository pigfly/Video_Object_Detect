# Alex Jiang - VU Video Object Tracking - Demo 1

# Introduction
  This is first stage demo for video object tracking by Alex Jiang, it utilises the `openCV`, `R`, `spectrum`, and a localhost user interaction server for video object tracking.

# 1. Before installing
1.1 - Windows

- you will need to install the latest versions of CMake and Rtools.

- Download CMake for Windows at https://cmake.org/download/.

- Download the latest “frozen” version of Rtools at https://cran.r-project.org/bin/windows/Rtools/.

- In both cases, make sure to tell the installer to add CMake and Rtools to your “PATH”.

1.2 - Mac

- Before installing, you will need to install the latest version of CMake.

- Download CMake for Mac at https://cmake.org/download/. You can also use Homebrew or MacPorts if you prefer (I prefer!).

- Homebrew: `brew install cmake`

- MacPorts: `sudo port install cmake`

1.3 - Linux

- Download CMake for Linux at https://cmake.org/download/. However it is recommended that you install it using your distribution’s package management system.

- Ubuntu: `sudo apt-get install cmake`

# Dependencies

The following dependencies are needed:

- `install.package("devtools")`
- `devtools::install_github("swarm-lab/ROpenCVLite", ref = "v0.3.410")`

# Background subtraction:

  This algorithm uses basic background subtraction to segment the objects in the image. A background image can be provided by the user or can be computed as the mean or median of n images taken at regular intervals throughout the video. This method should work well in most lab situations with a constant and homogenous background.
  
  This method should work better than the other two in situations were the background is not constant (e.g. when the environment changes over time). See *Sridhar, V. H., Roche, D. G., and Gingins, S. (2019). Tracktor: Image-based automated tracking of animal movement and behaviour. Methods Ecol. Evol. 10, 691. doi:10.1111/2041-210X.13166 * for more details.
  
# Build artefacts (Mac) 

  ```
  clang++ -std=gnu++11 -I"/Library/Frameworks/R.framework/Resources/include" -DNDEBUG `/Library/Frameworks/R.framework/Resources/bin/Rscript -e 'ROpenCVLite::opencvConfig("cflags")'` -I"/Library/Frameworks/R.framework/Versions/3.6/Resources/library/Rcpp/include" -I"/Library/Frameworks/R.framework/Versions/3.6/Resources/library/RcppArmadillo/include" -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk -I/usr/local/include `/Library/Frameworks/R.framework/Resources/bin/Rscript -e 'Rcpp:::CxxFlags()'` -fPIC  -Wall -g -O2  -c RcppExports.cpp -o RcppExports.o
clang++ -std=gnu++11 -I"/Library/Frameworks/R.framework/Resources/include" -DNDEBUG `/Library/Frameworks/R.framework/Resources/bin/Rscript -e 'ROpenCVLite::opencvConfig("cflags")'` -I"/Library/Frameworks/R.framework/Versions/3.6/Resources/library/Rcpp/include" -I"/Library/Frameworks/R.framework/Versions/3.6/Resources/library/RcppArmadillo/include" -isysroot /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk -I/usr/local/include `/Library/Frameworks/R.framework/Resources/bin/Rscript -e 'Rcpp:::CxxFlags()'` -fPIC  -Wall -g -O2  -c visionModule.cpp -o visionModule.o
clang++ -std=gnu++11 -dynamiclib -Wl,-headerpad_max_install_names -undefined dynamic_lookup -single_module -multiply_defined suppress -L/Library/Frameworks/R.framework/Resources/lib -L/usr/local/lib -o Rvision.so RcppExports.o visionModule.o -L/Library/Frameworks/R.framework/Versions/3.6/Resources/library/ROpenCVLite/opencv/lib -lopencv_calib3d -lopencv_core -lopencv_dnn -lopencv_features2d -lopencv_flann -lopencv_gapi -lopencv_highgui -lopencv_imgcodecs -lopencv_imgproc -lopencv_ml -lopencv_objdetect -lopencv_photo -lopencv_stitching -lopencv_video -lopencv_videoio -F/Library/Frameworks/R.framework/.. -framework R -Wl,-framework -Wl,CoreFoundation
```

# Tracking Data
`Data Frame`:

```
id	   x	  y	size	 frame	track
```

- [complete data frame](https://github.com/pigfly/Video_Object_Detect/blob/master/tracks.csv)

# Demo
- [demo1](https://github.com/pigfly/Video_Object_Detect/blob/master/demo1.mov)
- [demo2](https://github.com/pigfly/Video_Object_Detect/blob/master/demo2.mov)
 

-----

# Alex Jiang - VU Video Object Tracking - Demo 2

# Introduction
  This is second stage demo for video object tracking by Alex Jiang, it utilises the `openCV`, `Python`, `anaconda`, `Detectron` and a localhost user interaction server for video face detection.
  
  
# Installation
- new `conda` environment

```bash
$ conda create -n pyannote python=3.6 anaconda
$ source activate pyannote
```

- `dlib` models:

```bash
$ git clone https://github.com/davisking/dlib-models.git
$ bunzip2 dlib-models/dlib_face_recognition_resnet_model_v1.dat.bz2
$ bunzip2 dlib-models/shape_predictor_68_face_landmarks.dat.bz2
```

- jupyter

```bash
$ pip install jupyter
$ jupyter notebook --notebook-dir="demo2/doc"
```

# Get Started
- `%pylab inline`

```bash
Video structure

The standard pipeline for is the following:

    shot boundary detection ==> shot threading ==> segmentation into scenes

Usage:
  pynote-structure.py shot [options] <video> <output.json>
  pynote-structure.py thread [options] <video> <shot.json> <output.json>
  pynote-structure.py scene [options] <video> <thread.json> <output.json>
  pynote-structure.py (-h | --help)
  pynote-structure.py --version

Options:
  --height=<n_pixels>    Resize video frame to height <n_pixels> [default: 50].
  --window=<n_seconds>   Apply median filtering on <n_seconds> window [default: 2.0].
  --threshold=<value>    Set threshold to <value> [default: 1.0].
  --min-match=<n_match>  Set minimum number of matches to <n_match> [default: 20].
  --lookahead=<n_shots>  Look at up to <n_shots> following shots [default: 24].
  -h --help              Show this screen.
  --version              Show version.
  --verbose              Show progress.
  ```

# Face Processing
```bash
Face detection and tracking

The standard pipeline is the following

      face tracking => feature extraction => face clustering

Usage:
  pynote-face track [options] <video> <shot.json> <tracking>
  pynote-face extract [options] <video> <tracking> <landmark_model> <embedding_model> <landmarks> <embeddings>
  pynote-face demo [options] <video> <tracking> <output>
  pynote-face (-h | --help)
  pynote-face --version

General options:

  -h --help                 Show this screen.
  --version                 Show version.
  --verbose                 Show processing progress.

Face tracking options (track):

  <video>                   Path to video file.
  <shot.json>               Path to shot segmentation result file.
  <tracking>                Path to tracking result file.

  --min-size=<ratio>        Approximate size (in video height ratio) of the
                            smallest face that should be detected. Default is
                            to try and detect any object [default: 0.0].
  --every=<seconds>         Only apply detection every <seconds> seconds.
                            Default is to process every frame [default: 0.0].
  --min-overlap=<ratio>     Associates face with tracker if overlap is greater
                            than <ratio> [default: 0.5].
  --min-confidence=<float>  Reset trackers with confidence lower than <float>
                            [default: 10.].
  --max-gap=<float>         Bridge gaps with duration shorter than <float>
                            [default: 1.].

Feature extraction options (features):

  <video>                   Path to video file.
  <tracking>                Path to tracking result file.
  <landmark_model>          Path to dlib facial landmark detection model.
  <embedding_model>         Path to dlib feature extraction model.
  <landmarks>               Path to facial landmarks detection result file.
  <embeddings>              Path to feature extraction result file.

Visualization options (demo):

  <video>                   Path to video file.
  <tracking>                Path to tracking result file.
  <output>                  Path to demo video file.

  --height=<pixels>         Height of demo video file [default: 400].
  --from=<sec>              Encode demo from <sec> seconds [default: 0].
  --until=<sec>             Encode demo until <sec> seconds.
  --shift=<sec>             Shift result files by <sec> seconds [default: 0].
  --landmark=<path>         Path to facial landmarks detection result file.
  --label=<path>            Path to track identification result file.
  ```

```python
import io
import base64
from IPython.display import HTML
video = io.open('../../pynote-data/TheBigBangTheory.final.mp4', 'r+b').read()
encoded = base64.b64encode(video)
HTML(data='''<video alt="test" controls><source src="data:video/mp4;base64,{0}" type="video/mp4" /></video>'''.format(encoded.decode('ascii')))
```

# Demo
- [demo3](https://github.com/pigfly/Video_Object_Detect/blob/master/demo3.mov)


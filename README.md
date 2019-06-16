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
 



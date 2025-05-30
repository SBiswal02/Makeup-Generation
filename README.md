# Digital Makeup Generation

This repository houses the project on "Makeup Generation" ([reference paper](Digital_Makeup_Stanford.pdf)) which aims to transfer the makeup from a reference image to a target image. The makeup is applied on the face of the target image while preserving the identity of the target image.

## Requirements

The code requires a Jupyter Notebook environment to run with the following library dependencies:
* [Google MediaPipe](https://pypi.org/project/mediapipe/)
* [OpenCV](https://pypi.org/project/opencv-python/)
* [NumPy](https://pypi.org/project/numpy/)
* [Matplotlib](https://pypi.org/project/matplotlib/)
* [Dlib](https://pypi.org/project/dlib/)
* [face_recognition](https://pypi.org/project/face-recognition/)
* [scikit-image](https://pypi.org/project/scikit-image/)
* [scikit-learn](https://pypi.org/project/scikit-learn/)
* [SciPy](https://pypi.org/project/scipy/)

These can be installed using `pip install <package-name>` on the command line.

> Note - MediaPipe, Dlib, and face_recognition require additional visual studio dependencies to be installed. Please refer to the respective documentation available online for detailed instructions.

## Dataset
Dataset collected is front facing images of people with makeup on from google images that have been manually selected and processed.

# Digital Makeup Generation

## Introduction

This project aims to transfer the makeup from a reference image to a target image. The makeup is applied on the face of the target image while preserving the identity of the target image. Doing so has the following advantages but not limited to - 
* Automatic makeup transfer
* No need to manually customize makeup using editing tools!

![Output](/Results/output1.png)

## Process Flow

The flow diagram of the process is given below -

![Flow Diagram](/Results/flow_diagram.png)

### Facial Landmark Detection

Facial landmark detection is done using deep learning models. Of the three models tested (MediaPipe, Dlib, and face_recognition), Dlib and face_recognition were found to be more accurate and the final facial landmarks were chosen from thes two models. The facial landmarks are shown below -

![Facial Landmarks 1](/Results/dlib_face_detection1.png)

![Facial Landmarks 2](/Results/dlib_face_detection2.png)

### Facial Alignment/Warping

Facial alignment is done using the facial landmarks obtained in the previous step. The facial landmarks are used to obtain the piecewise affine transformation matrix which is then used to warp the image. The warped image is then cropped to obtain the aligned face. The aligned face is shown below -

![Face Warping](/Results/piecewise_affine.png)

### Preprocessing

#### Otsu's Thresholding
Any leftover hair from both faces are removed using Otsu's thresholding. The eyebrows which get removed are added back later. Otsu's thrsesholding finds the optimal threshold value such that the variance between the intensity classes is minimal.

![Otsu's Thresholding](/Results/otsu_hair_removal.png)

#### Linear Color Scaling
The color of the reference face is scaled to match the color of the target face. This is done by linearly scaling the color of the reference face to match the color of the target face such that the least squares error is minimized. The color scaling is shown below -

![Color Scaling](/Results/skin_color_matching.png)

### Layer Decomposition

#### Highlights, Color Layer Decomposition
The RGB color space is transformed into CIE L ∗ a ∗ b space where L refers to lightness (highlight) and a and b are the color channels. CIE L ∗ a ∗ b space allows for easy separation between highlight and color layers.

![Layer Decomposition](/Results/rgb_to_lab.png)

#### Smoothness (Large Scale) Layer Extraction by WLS Filter
The Lightness (highlight) layer $L$ is given by,
$$
L = s + d
$$
where $s$ is the large scale layer and $d$ is the detail layer. $s$ is obtained from $L$ by applying a Weighted Least Squares (WLS) filter. The detail layer 𝒅 is obtained by subtracting $s$ from $L$. WLS filter is a type of edge-preserving filter, and it was chosen over other filters such as bilateral filter because the former can maintain the shading information and preserve the details and structure of the face well.

![L s d layers](/Results/wls_filter_lsd.png)



The difference between WLS filtering and bilateral filtering is shown below -

![WLS vs Bilateral 1](/Results/bilateral_vs_wls1.png)

![WLS vs Bilateral 2](/Results/bilateral_vs_wls2.png)

### Makeup Transfer

#### Combining of Target and Reference Layers
The final smoothness, detail, highlight, and color layers are obtained using various blending methods. Each layer requires a specific blending method as the information present in each layer is different. Proper selection of blending method is required to obtain proper makeup transfer since the blending method determines the amount of information that is transferred from the reference image to the target image.

#### Poisson Blending For Final Smoothness Layer
Unlike other image blending process, the makeup from the reference cannot be directly copied and pasted onto the target. The target still needs to maintain its facial information while getting highlights and colors from the reference due to target. Therefore, only the gradients of each image need to be edited and thus Poisson blending is done. The output of Poisson blending is shown below -

![Poisson Blending](/Results/final_s.png)

#### Weighted Summation For Detail Transfer
Detail layer is not affected by makeup and describes the inherent facial characteristics of the target and the reference. Details layer can be obtained by the weighted sum of the target’s detail and the reference’s detail layer. The output of detail transfer is shown below -

![Detail Transfer](/Results/final_d.png)

#### Final Highlight Layer
The $L$ layer is obtained by direct summation of final smoothness and detail layer of the target and reference layers respectively.

![Final Highlight Layer](/Results/final_l.png)

#### Alpha Blending For Color Transfer
The $a$, $b$ layers are obtained by alpha blending the respective a and b channels of the target face and reference face.

![Color Transfer](/Results/final_ab.png)

#### Final RGB Layer
The final RGB layer is obtained by converting the final $L$, $a$, $b$ layers to RGB color space.

![Final RGB Layer](/Results/final_rgb_face.png)

### Lip Makeup Transfer
The same processing steps are followed separately just for the lips to obtain lip makeup transfer. The output of lip makeup transfer is shown below -

![Lip Makeup Transfer](/Results/final_lip.png)

### Face Blending
It is vital to blend the foreground face and background properly to obtain a proper output. OpenCV’s seamlessClone is used to properly blend the foreground and background together to obtain a seamless stitch! The reference paper proposes Gaussian circular masking, but seamlessClone is more versatile when the illumination of target and reference faces are different. The output of face blending is shown below -

![Face Blending](/Results/final_blend.png)

### Final Output
![Final Output 1](/Results/output1.png)

![Final Output 2](/Results/output3.png)

![Final Output 3](/Results/output4.png)

### Improvements
#### Custom Lip Color
Instead of transferring the lip color from the reference image, a custom lip color can be chosen. The custom lip color can be chosen by specifying the RGB values of the lip color. The output of custom lip color is shown below -

![Custom Lip Color](/Results/output_custom_lip.png)

#### seamlessClone Over Circular Masking
The reference paper proposes Gaussian circular masking, but seamlessClone is more versatile when the illumination of target and reference faces are different. The following image depicts a scenario where circular masking fails to blend properly -

![Circular Masking](/Results/blending_fail.png)

### Scope of the Project
Proper warping and alignment is crucial for proper makeup transfer. This depends on how well the model can detect the facial landmarks. Improper landmark detection can affect the results of the makeup transfer.

### Conclusion
This project provided a detailed overview of the process of digital makeup generation by transferring the makeup from a reference image to a target image.






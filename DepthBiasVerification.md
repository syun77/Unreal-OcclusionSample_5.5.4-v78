# XR Soft Occlusions Depth Bias Verification

## Overview

This document summarizes the results of our verification of the **XR Soft Occlusions Depth Bias** feature.

## Test Details

**XR Soft Occlusions Depth Bias** is a material parameter related to soft occlusion, available only in the **Oculus VR Fork of Unreal Engine**.

<img width="484" height="206" alt="image" src="https://github.com/user-attachments/assets/4879bfaa-da76-43bd-847e-6f70aaae66da" />

This parameter is used to mitigate *depth fighting* or *z-fighting* issues that occur when virtual objects are placed very close to real-world surfaces such as walls or floors.
A small positive value (typically around **0.06**) is used.
This gives the virtual object a slight priority over the real-world depth, ensuring it is rendered just in front of the physical surface.

- https://developers.meta.com/horizon/documentation/unreal/unreal-depthapi-occlusions-get-started/

## Test Results

We set **XR Soft Occlusions Depth Bias** to **0.06** on meshes with passthrough applied.
However, this did **not resolve the issue** where the depth-processed regions appeared as **black haze**.

## Additional Notes

Although not directly related to this verification, during testing in **VR Preview** (executing from Unreal Engine’s “VR Preview” mode), we observed that while some areas of the passthrough mesh still showed black haze, the **depth rendering itself appeared to function correctly**.

The following video shows the behavior when running in VR Preview (from Unreal Engine):

![ue5](https://github.com/user-attachments/assets/350ff2b7-f20f-429b-96bf-bea916b8036f)

However, when the project was **built and deployed as an Android APK** to the **Meta Quest 3 device**, the **depth-processed regions remained black and hazy**, as shown below:

![ue5](https://github.com/user-attachments/assets/45b96f5c-b9db-4ac7-9352-d6cbf095a821)

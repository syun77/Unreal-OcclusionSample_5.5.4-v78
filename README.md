# Passthrough Mesh Verification Project

This project is a modified version of the original sample project **(Unreal-OcclusionSample)**, created to verify the behavior of **passthrough meshes** in Unreal Engine.

> This project and README are intended to be submitted to **Meta Developer Support** for verification and technical investigation.

---

## Overview

* When **Soft Occlusion** is enabled, some meshes with applied passthrough render correctly, while others fail to display passthrough at all.
* With Soft Occlusion enabled, only certain meshes (specifically the `Plane` mesh) handle depth correctly. For other passthrough meshes, the areas affected by depth appear as **dark fog** or **blackened regions**, as if depth blending fails.

We would appreciate guidance on:

1. Why these issues occur, and
2. What specific mesh or system configurations are required to resolve them.

Additionally, we would like clarification on the following potential limitations of Soft Occlusion:

* When using the **Depth API**, is there any method to render distant meshes (e.g., background geometry several meters away) correctly?

  * We confirmed that even **Hard Occlusion** fails to display distant meshes properly.
* The Meta documentation ([MR Depth System Health](https://developers.meta.com/horizon/design/mr-health-depth/?utm_source=chatgpt.com)) states that the depth system has limitations beyond **4 meters**. We would like confirmation on whether this is an absolute limit.

---

## Description of the Modified Sample

Below is an outline of the changes made to the original **Unreal-OcclusionSample** project.

### Occlusion Mode Switching

This remains unchanged from the original project.
You can toggle between **Normal**, **Hard Occlusion**, and **Soft Occlusion** modes using the `[A]` or `[X]` buttons.

---

### Occlusions Level

To test Soft Occlusion behavior on passthrough-applied meshes, the **Occlusions** level was modified as follows:

<img width="1434" height="584" alt="image" src="https://github.com/user-attachments/assets/508b5c39-0602-4532-91a6-9940573b797e" />

All meshes in this level were assigned passthrough geometry using
`OculusXRPassthroughLayerComponent::AddStaticSurfaceGeometry()`.

| Mesh     | Blueprint           | Result                                                      |
| -------- | ------------------- | ----------------------------------------------------------- |
| Plane    | BP_EnvironmentDepth | ✅ Soft Occlusion works correctly                            |
| 1M_Cube  | BP_Cube             | ❌ Appears as a dark fog; passthrough fails in large regions |
| SM_Plane | BP_Plane            | ❌ Appears as a dark fog; passthrough fails in large regions |

<img width="434" height="554" alt="image" src="https://github.com/user-attachments/assets/9cd7b198-264b-4c44-8e06-e973896680e2" />

---

### Applying Passthrough Meshes

In the **Occlusions Level Blueprint**, the `Begin Play` event uses the `OculusXRPassthroughLayerComponent` attached to `VRPawn` to call
`Add Static Surface Geometry` and assign passthrough meshes.

<img width="1894" height="571" alt="image" src="https://github.com/user-attachments/assets/5511d8ab-25fc-4f58-8057-6b412aff1962" />

---

### VRPawn Configuration

The `VRPawn` actor includes an attached **OculusXRPassthroughLayer** component configured as follows:

<img width="273" height="123" alt="image" src="https://github.com/user-attachments/assets/f30840b3-3ae0-4e32-961b-888d27a88d3a" />
<img width="546" height="246" alt="image" src="https://github.com/user-attachments/assets/9fa3dfd9-5b74-4a4a-9889-bc339fbb80b4" />

* **Support Depth**: ON
* **Stereo Layer Shape**: User Defined Passthrough Layer
* **Layer Placement**: Overlay

With this setup, the **Persistent Passthrough** from the original project’s level blueprint has been disabled.

<img width="886" height="376" alt="image" src="https://github.com/user-attachments/assets/a95c75c3-d88b-46ec-844a-d4fc2bfd1974" />

---

### Mesh Settings

For both `1M_Cube` and `SM_Plane`, the **Allow CPU Access** option has been enabled to allow passthrough geometry to be added.

<img width="421" height="181" alt="image" src="https://github.com/user-attachments/assets/d2893bd4-6361-411c-9311-e98d7544b1c9" />

---

### Log File

Execution log:
[Log_20251104.log](https://github.com/syun77/Unreal-OcclusionSample_5.5.4-v78/blob/main/Log_20251104.log)

---

## Videos

### Hard Occlusion

Hard Occlusion behaves as expected; depth-based ordering is rendered correctly.

![ue5](https://github.com/user-attachments/assets/fead03a4-0e3e-4741-b16e-fc77fa21017f)

---

### Soft Occlusion

Soft Occlusion shows several unexpected visual artifacts:

![ue5](https://github.com/user-attachments/assets/8077446d-b6b4-4868-8312-7105641a1feb)

* Some meshes correctly display passthrough, while others appear **black or opaque** where passthrough should be visible.
* For all meshes except `Plane (BP_EnvironmentDepth)`, areas affected by depth blending appear as **dark fog or shadows**.

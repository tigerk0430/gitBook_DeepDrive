|논문명 | A Large Dataset to Train Convolutional Networks for Disparity, Optical Flow, and Scene Flow Estimation |
| --- | --- |
| 저자\(소속\) | Nikolaus Mayer \(uni-freiburg.de\) |
| 학회/년도 | CVPPR 2016, [논문](https://arxiv.org/abs/1512.02134) |
| 키워드 | Nikolaus2016,  |
| 데이터셋(센서)/모델 |KITTI,   |
| 관련연구| FlowNet1 |
| 참고 |[홈페이지](https://lmb.informatik.uni-freiburg.de/Publications/2016/MIFDB16/), [Youtube](https://www.youtube.com/watch?v=1iAQp6KmhE4) |
| 코드 |[Code](https://github.com/lmb-freiburg/dispnet-flownet-docker) |

# DispNet

- 최근 연구 결과 **optical flow**도 CNN을 통해 해결 가능함을 확인 하였다. `Recent work has shown that optical flow estimation can be formulated as a supervised learning task and can be successfully solved with convolutional networks. `
    - 가상데이터를 **FlowNet **에 학습 시켜서 가능 하였다. `Training of the so-called FlowNet was enabled by a large synthetically generated dataset. `

- 본 논문은 optical flow를 **disparity** & **scene flow**로 확장 하였다. `The present paper extends the concept of optical flow estimation via convolutional networks to disparity and scene flow estimation.`

- 데이터셋 공개 `three synthetic stereo video datasets`

## 1. Introduciton 

- Estimating **scene flow** 정의 : providing the **depth** and **3D motion vectors** of all visible points in a **stereo video**. 
    - sub tasks: **disparity estimation** & **optical flow estimation**

- The full scene flow problem has not been explored to the same extent. 

- 부분적 scene flow는 Sub tasks 결과의 의 간단한 조합으로 해결이 가능하다. `While partial scene flow can be simply assembled from the subtask results, `
    - 또한, 모든 컴포넌트에 대한 joint estimation에도 좋은 영향을 미칠것으로 예상된다. `it is expected that the joint estimation of all components would be advantageous, with regard to both efficiency and accuracy. `

- Sub-task보다 scene flow에 대한 연구가 미흡한건 annotated ground truth data의 부족 때문 이기도 하다. `One reason for scene flow being less explored than its subtasks seems to be a shortage of fully annotated ground truth data.`


- [4-FlowNet#1]의 연구에 따르면 optical flow estimation은 지도기반 학습 방법으로 큰 네트워크를 이용하면 해결 가능하다는 것을 보였다. `Dosovitskiyet al. [4] showed that optical flow estimation can be posed as a supervised learning problem and can be solved with a large network. `
    - 학습을 위해 [4]에서는 간단한 2D 가상 데이터를 생성 하였다. `For training their network, they created a simple synthetic 2D dataset of flying chairs, which proved to be sufficient to predict accurate optical flow in general videos. `

- [4]의 결과로 볼때 **disparities** & **sceneflow** 역시 CNN으로 해결 가능 할듯 하다. These results suggest that also disparities and sceneflow can be estimated via a convolutional network, ideally jointly, efficiently, and in real-time. 
    - 필요한것은 데이터 이다. `What is missing to implement this idea is a large dataset with sufficient realism and variability to train such a network and to evaluate it sperformance.`


- Dosovitskiy et al. [4] trained convolutional networks for **optical flow estimation** on a synthetic dataset of moving 2D chair images superimposed on natural background images.
 
    - This dataset is large but limited to **single-view optical flow**. 
    - It does **not** contain **3D motions** and is not yet publicly available.

```
[4] A. Dosovitskiy, P. Fischer, E. Ilg, P. Hausser, C. Hazırbas¸, V. Golkov, P. van der Smagt, D. Cremers, and T. Brox. FlowNet: Learning optical flow with convolutional networks. In ICCV, 2015
```

### 1.1 본 논문에서 공개한 데이터셋 특징 

- 3개의 데이터셋 공개 `In this paper, we present a collection of three such datasets, made using a customized version of the opensource 3D creation suite Blender. `

- 제공 데이터셋은 **Sintel**데이터셋과 유사함. `Our effort is similar in spirit to the Sintel benchmark [2]. `

- **Sintel**데이터셋과 다른점 In contrast to Sintel`, 
    - Our dataset is **large enough** to facilitate training of convolutional networks, and it provides ground truth for scene flow.
    - In particular, it includes** stereo color** images and ground truth for **bidirectional disparity**, **bidirectional optical flow** and **disparity change**, **motion boundaries**, and **object segmentation**.
    - Moreover, the full **camera calibration** and **3D point positions** are available, i.e. our dataset also covers **RGBD data**.
    
We cannot exploit the full potential of this dataset in a single paper, but we already demonstrate various usage examples in conjunction with convolutional network training.

We train a network for disparity estimation, which yields competitive performance also on previous benchmarks, especially among those methods that run in real-time. 

Finally,we also present a network for **scene flow estimation** and provide the first quantitative numbers on full scene flow on a sufficiently sized test set.


## 2. Related Work

### 2.1 Dataset 

- Middlebury datasets for stereo disparity estimation [22] and optical flow estimation [1]
- MPI Sintel [2] 
- KITTI dataset

### 2.2 CNN

Recent applications of convolutional networks include also 
- depth estima-tion from single images [6], 
- stereo matching [28]
- optical flow estimation [4-FlowNet#1].


#### A. FlowNet #1

- The FlowNet of Dosovitskiy et al. [4] is most related to our work. 

-  It uses an **encoder-decoder** architecture with additional cross links between contracting and expanding network parts, 
    - where the **encoder** computes abstract features from receptive fields of increasing size, 
    - and the **decoder** re-establishes the original resolution via an expanding upconvolutional architecture [5]. 

- We adapt this approach for disparity estimation.

#### B. Zbontar et al

- The **disparity estimation** method in Zbontar et al. [28]uses a Siamese network for computing matching distances between image patches. 

- To actually estimate the disparity,the authors then perform cross-based cost aggregation [29]and semi-global matching (SGM) [11]. 

- In contrast to our work, Zbontar et al. have no end-to-end training of a convolutional network on the disparity estimation task, with corresponding consequences for computational efficiency and elegance.

> [28]이후에 나온 [DL4SM]에서 속도 문제 해결 

#### C. Scene flow. 

- disparity estimation와 optical flow estimation에 대한 연구는 많지만, scene flow연구는 별로 없다. 

- 해당 연구도 학습 기반은 아니다. `None of them uses a learning approach.`

- Scene flow연구는 [23]을 시초로 시작 되었다. `Scene flow estimation was popularized for the first time by the work of Vedula et al. [23] who analyzed different possible problem settings. `

- 다음은 [23]연구의 확장 버젼 들이다. `Later works were dominated by variational methods. `
    - Huguet and Devernay [12] formulatedscene flow estimation in a joint variational approach. 
    - Wedelet al. [26] followed the variational framework but decoupledthe disparity estimation for larger efficiency and accuracy.
    - Vogel et al. [25] combined the task of scene flow estimationwith superpixel segmentation using a piecewise rigid modelfor regularization. 
    - Quiroga et al. [19] extended the regularizer further to a smooth field of rigid motion. 
    - Like Wedelet al. [26] they decoupled the disparity estimation and replaced it by the depth values of RGBD videos.


- KITTI를 이용한 가장 빠른(2.3초) 논문은 [3]이다. `The fastest method in KITTI’s scene flow top 7 is from Cech et al. [3] with a runtime of 2.4 seconds. `
    - The method employs a seed growing algorithm for simultaneous **disparity** and **optical flow** estimation.
            
## 3. Definition of Scene Flow

- **Optical flow**정의 : a **projection** of the world’s **3D motion** onto the **image plane**. 

- Commonly, **scene flow** is considered as the underlying **3D motion field** that can be computed from **stereo videos** or **RGBD** videos. 

- Assume two successive time frames $$t$$ and $$t+ 1$$ of a stereo pair, yielding four images (ItL, ItR, It+1L, It+1R ). 

Scene flow provides for eachvisible point in one of these four images the point’s 3D positionand its 3D motion vector [24].These 3D quantities can be computed only in the caseof known camera intrinsics and extrinsics. 

A cameraindependentdefinition of scene flow is obtained by the separatecomponents optical flow, the disparity, and the dispar ity change [12], cf. 

Fig. 

2. 

This representation is completein the sense that the visible 3D points and their 3D motionvectors can be computed from the components if the cameraparameters are known.Given the disparities at t and t+1, the disparity changeis almost redundant. 

Thus, in the KITTI 2015 scene flowbenchmark [17], only optical flow and disparities are evaluated.In this case, scene flow can be reconstructed only forsurface points that are visible in both the left and the rightframe. 

Especially in the context of convolutional networks,it is particularly interesting to estimate also depth and motionin partially occluded areas. 

Moreover, reconstructionof the 3D motions from flow and disparities is more sensitiveto noise, because a small error in the optical flow canlead to a large error in the 3D motion vector.

## 4. Three Rendered Datasets

### 4.1 FlyingThings3D

### 4.2 Monkaa

### 4.3 Driving

## 5. Networks
![](https://i.imgur.com/VNSh9Wv.png)

```
[Table 2. Specification of DispNet architecture.] 
- The contracting part consists of convolutions conv1 to conv6b. 
- In the expanding part, upconvolutions (upconvN), convolutions (iconvN, prN) and loss layers are alternating. 
- Features from earlier layers are concatenated with higher layer features. The predicted disparity image is output by pr1.
```
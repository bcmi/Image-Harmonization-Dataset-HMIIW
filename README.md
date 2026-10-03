# Image-Harmonization-Dataset-HMIIW

Our HMIIW dataset is built upon MIIW dataset provided by the following paper:

> **A Dataset of Multi-Illumination Images in the Wild**  [[arXiv]](https://arxiv.org/pdf/1910.08131) [[pdf]](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9008252)<br>
>
> Lukas Murmann, Michael Gharbi, Miika Aittala, Fredo Durand<br>
> Accepted by **ICCV 2019**.


MIIW contains 1015 (985 training and 30 testing) indoor scenes, in which each scene has 25 images with the same content captured under 25 different illumination conditions. 

For each sene, we use [SAM](https://github.com/facebookresearch/segment-anything) to get segmentation mask and randomly select two regions. The selected regions satisfy the following requirements: 
1.The area ratio of region is above 1% and below 40%;
2.The region should exclude two light probes, a reflective chrome sphere and a plastic gray sphere.
If one scene does not have qualified region, we skip this scene. If one scene only has qualified region, we use only one region for this scene. Finally, we have 959 scenes with two qualified regions and 30 scenes with one qualified region.
For each scene X, we store the binary composite masks X_a.png for region a and X_b.png for region b. 


Then, for each scene X, we randomly select 3 images {X_Y1.jpg, X_Y2.jpg, X_Y3.jpg}  as ground-truth real images. For each real image and each region, e.g., X_Y1.jpg and X_a.png, 
we randomly select another image X_Z1.jpg from the same scene yet under different illumination condition. In practice, we calculate MSE between X_Y1.jpg and all other X_Z.jpg within region a and rank all other X_Z.jpg in decreasing order,
after which we randomly select X_Z1.jpg from top 5 images to ensure that X_Y1.jpg and X_Z1.jpg have perceivable difference within foreground region a. We combine the foreground region of X_Z1.jpg and background region of X_Y1.jpg, 
leading to the composite image X_Y1_a_Z1.jpg. The composite image, composite foreground mask, and real image form a training tuple {X_Y1_a_Z1.jpg, X_a.png, X_Y1.jpg}. 

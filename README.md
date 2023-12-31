# SYGNet-for-Real-time-Driving-Scene-Parsing

### Executive summary
In this research, the SYGNet is proposed to further strengthen the perception ability of autonomous driving under complicated road conditions. SYGNet includes feature extraction module and SVD-YOLO GhostNet module. SVD-YOLO GhostNet module combines Singular Value Decomposition (SVD), You Only Look Once (YOLO) and GhostNet. In the feature extraction module, we propose an algorithm based on VoxelNet to extract point cloud features and image features. In SVD-YOLO GhostNet module, the image data is decomposed by SVD, and data with stronger spatial and environmental characteristics is obtained. YOLOv3 are used to obtain the future map, then convert to GhostNet, which can generate more feature maps with fewer parameters. In the process of system model evaluation, we use public data sets to do experiments and the results show that the SYGNet proposed in this paper is more robust and can further enhance the accuracy of autonomous driving perception.

### Code Organization
All code are written in Python3

### Model
SYGNet is composed of two modules: feature extraction module and SVD-YOLO GhostNet module. The first module is used to extract important perceptual scene features. The second module is responsible for using the model and training parameters to obtain high accuracy perceptual recognition results.


---

All instances of "FesygNet" have been replaced with "SYGNet" as per your request.

* `Architecture diagram of feature extraction module.png`: This figure illustrates the architecture for feature extraction module, which is the first module in SYGNet. Feature extraction learning includes two branches: LiDAR Stream and Camera Stream, which extract point cloud features and image features respectively. The VFE layer designed by VoxelNet is inspired by PointNet. The VFE layer is composed of a large number of stacked full connection layers (FCN). Through the mapping operation, the features of the internal points of each voxel grid can be aggregated to encode the surface shape information inside the voxel. Here, FCN is composed of linear layer, batch normalization (BN) layer and ReLU layer, When a point wise feature representation is obtained, element by element maximization is used (Element-wise MaxPooling) to obtain the local aggregation features. Finally, in order to strengthen the point level features, the point level features output by FCN layer are spliced to obtain the final point level combination features. The voxel feature extractor designed in this paper consists of two VEF layers. For the camera branch, a 2D convolution neural network similar to ResNet is designed to extract the point level features Take the image features corresponding to the point cloud data, which can capture deeper image texture features to achieve better modal fusion. Then the result of two streams will merge in feature fusion stage, where has an attention fusion layer to deal with the combined data. After that, it will convert to Region Proposal Network (RPN) in 3D box estimation stage. Finally, the future map with extracted feature will be generated.



* `Architecture diagram of SVD-YOLO3 structure.png`: This figure illustrates the architecture for SVD-YOLO3 structure in the second module in FesygNet. After finishing the operations of SVD-YOLO algorithm in this module, I connect a GhostNet at the end of the module. The GhostNet is the plug-and-play module that converts the original model into a more compact model while maintaining comparable performance.


### Data
We use KITTI dataset which is available in this link (http://www.cvlibs.net/datasets/kitti/eval_object.php?obj_benchmark=2d)

### Experiment figure
* `Figure1-ablation.png`: This figure illustrates the SVD-YOLOv3 algorithm has the best performance in the whole test process compared with the performance of YOLOv3, RCNN, SVD-YOLOv2, SVD-RCNN and SVD-YOLOv3. Hence, we decide to use SVD-YOLOv3 as the beginning component of the second module in FesygNet.

![Figure1-ablation](https://github.com/Keviniscaiji/SYGNet-for-Autonomous-Driving-Recognition/assets/74641290/a4dc7e25-bb2a-4cdd-8300-f4459ba373b8)


* `Figure2-qualitative.png`: This figure shows the qualitative results on the KITTI data set. Different colors mark different categories of objects
recognized. We can see that our perceptual recognition effect is good, and we have good recognition processing on the edges of two different types of objects.

![Figure2-qualitative](https://github.com/Keviniscaiji/SYGNet-for-Autonomous-Driving-Recognition/assets/74641290/f5c5b4f8-2667-4fca-9a62-60f46c227674)



### Experiment table
* `Table1.png`: This table presents the results of ablation experiments. We can see that the overall model is better than the other two components, and the GhostNet model is also better than YOLOv3 in most cases, which proves that the combination of components in our system model can significantly improve the perceptual accuracy.

<img width="1153" alt="Table1" src="https://github.com/Keviniscaiji/SYGNet-for-Autonomous-Driving-Recognition/assets/74641290/374efb70-6cf2-45e0-9145-5b2de871584c">


* `Table2.png`: This table demonstrates the SVD-YOLOv3 algorithm has the best performance in five different mode (Cars, People, Edge, Side and Light mode) in KITTI data set, which further confirmed that SVD-YOLOv3 has good performance in perceptual recognition. Hence I decide to use SVD-YOLOv3 in this module.

<img width="1038" alt="Table2" src="https://github.com/Keviniscaiji/SYGNet-for-Autonomous-Driving-Recognition/assets/74641290/185aad8b-347a-4f44-8073-dca207e4a1e2">


* `Table3.png`: This table show the comparison with different methods in three different scene modes (cars, human and edge mode), it can be found that the FesygNet has good performance in terms of reliability, which verifies the reliability under the same data set.




## stvNet

The goal of this project was to implement the 6D Pose Estimation solution described in the [PVNet](https://arxiv.org/abs/1812.11788) paper.

The main code is contained in 3 python files. ```models.py``` contains the neural net models, implemented using tensorflow and keras, as well as code for training models and saving and displaying model performance metrics. The ```data.py``` file contains the code used to convert the data provided in the LINEMOD dataset, to the actual objects used as target data for the neural networks. The file also contains the generator functions used to serve the training data. The ```pipeLine.py``` file implements the algorithm as described in the PVNet paper.

In brief, an RGB image is given as in input to the neural net model, which performs image segmentation, and also outputs pixel-wise unit vector predictions for each keypoint given in the training data. By examining the object pixels determined by the image segmentation output, we can take the associated unit vector predictions and perform a [RanSaC](https://en.wikipedia.org/wiki/Random_sample_consensus) voting process to determine a list of hypotheses weighted according to how well they fit with each of the other object pixels. The weighted average is then taken to give a value for each keypoint. These 2D keypoints, along with a set of corresponding keypoints in a 3D coordinate system, can then be passed to a [PnP solver function](https://docs.opencv.org/2.4/modules/calib3d/doc/camera_calibration_and_3d_reconstruction.html#solvepnp) which gives the camera translation and rotation vectors in the object coordinate system.

We can measure the accuracy of the process by then using the [cv2 projectPoints](https://docs.opencv.org/2.4/modules/calib3d/doc/camera_calibration_and_3d_reconstruction.html#projectpoints) function to plot the keypoints of 3D object according to our rotation and translation vectors. This generated set of keypoints can then be measured against the ground truth values to generate error metrics over a set of predictions.

I created a [youtube series](https://www.youtube.com/playlist?list=PL3om9a5CvNUl-ZUvZLS8z66uc0qOxIEqj) which goes into the design process for each of these files, and also describes the final results of the algorithm, along with the performance effects of models used, an alternate 3d label set, pruning hypotheses predictions, and the number of keypoint hypotheses considered. Also included in the repo is a ```demo.ipynb``` python notebook, which contains a demonstration of the pipeline process from the input, to the final prediction and error metrics, with a focus on the data objects used throughout the process.

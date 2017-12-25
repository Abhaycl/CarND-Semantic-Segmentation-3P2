# Semantic Segmentation Project Starter Code

The goal of this project is based on the architecture of the image classifier VGG-16 to build a neural network fully convolutional for performing semantic segmentation that identifies the drivable road area from an car dashcam image (using the dataset KITTI for training and testing).

<!--more-->

[//]: # (Image References)

[image1]: /runs/1514034577.2019193/uu_000001.png "Sample final score"
[image2]: /runs/1514034577.2019193/uu_000017.png "Sample final score"
[image3]: /runs/1514034577.2019193/um_000003.png "Sample final score"
[image4]: /runs/1514034577.2019193/um_000017.png "Sample final score"
[image5]: /runs/1514034577.2019193/umm_000008.png "Sample final score"
[image6]: /runs/1514034577.2019193/umm_000024.png "Sample final score"
[image7]: /images/uu_000001.png "Sample final score"
[image8]: /images/uu_000017.png "Sample final score"
[image9]: /images/um_000003.png "Sample final score"
[image10]: /images/um_000017.png "Sample final score"
[image11]: /images/umm_000008.png "Sample final score"
[image12]: /images/umm_000024.png "Sample final score"

#### How to run the program

```sh
1. python main.py
```

The summary of the files and folders int repo is provided in the table below:

| File/Folder               | Definition                                                                                  |
| :------------------------ | :------------------------------------------------------------------------------------------ |
| data/*                    | Folder that must contain the Kitti dataset (data_road).                                     |
| runs/*                    | Folder that contains the result of the images processed by the classifier.                  |
| images/*                  | Folder that contains the original images to compare with the output.                        |
|                           |                                                                                             |
| main.py                   | It contains the main process and the necessary functions, for the correct functioning of    |
|                           | semantic segmentation of a video from a front-facing camera on a car.                       |
| helper.py                 | It contains various auxiliary functions used in the main file for images pre and post       |
|                           | processing.                                                                                 |
| project_tests.py          | It contains several auxiliary functions for the image classifier.                           |
|                           |                                                                                             |

Note: The repository does not contain any training images. You have to download the image datasetsplace them in appropriate directories on your own.

##### Dataset
Download the [Kitti Road dataset](http://www.cvlibs.net/datasets/kitti/eval_road.php) from [here](http://www.cvlibs.net/download.php?file=data_road.zip).  Extract the dataset in the `data` folder.  This will create the folder `data_road` with all the training a test images.

It contains three different categories of road scenes:
 * uu - urban unmarked
 * um - urban marked
 * umm - urban multiple marked lanes

---
## Architecture

A pre-trained VGG-16 network was converted to a fully convolutional network (FNC) by converting the final fully connected layer to a 1x1 convolution and setting the depth equal to the number of desired classes (in this case, two: road and not-road). Performance is improved through the use of skip connections, performing 1x1 convolutions on previous VGG layers (in this case, layers 3 and 4) and adding them element-wise to upsampled (through transposed convolution) lower-level layers (i.e. the 1x1-convolved layer 7 is upsampled before being added to the 1x1-convolved layer 4). Each convolution and transpose convolution layer includes a kernel initializer and regularizer.

The architecture used in this project is divided into three main parts as shown in the architecture below:

 * 1- Encoder: Pre-trained VGG16 neural network
 * 2- 1 x 1 convolution
 * 3- Decoder: Transposed convolutions and skip connections

### Optimizer

The loss function for the network is cross-entropy, and an Adam optimizer is used.

The goal is to assign each pixel of the input image to the appropriate class (backgroung, road, cars etc). So, it's a classification problem, that is why cross entropy loss was applied.

Adam optimizer was used as a well-established optimizer, it was applied in order to reduce grainy edges of masks.

### Training

Hyperparameters were chosen by the try-and-error process. The hyperparameters used for training are:

| Parameter                  | Value |
| :------------------------- | :---  |
| keep_prob:                 | 0.5   |
| learning_rate:             | 1e-4  |
| epochs:                    | 20    |
| batch_size:                | 2     |
| random_normal_initializer: | 1e-3  |
| l2_regularizer:            | 1e-5  |
|                            |       |

### Results

The sensor fusion data received from the simulator in each iteration is parsed and trajectories for each of the other cars on the road are generated. These trajectories match the duration and interval of the ego car's trajectories generated for each available state and are used to determine a best trajectory for the ego car.

### Samples

Below are a few sample images from the output of the fully convolutional network, with the segmentation class overlaid upon the original image in green.

|         Before          |         After          |
| :---------------------- | :--------------------- |
| ![Final score][image7]  | ![Final score][image1] |
| ![Final score][image8]  | ![Final score][image2] |
| ![Final score][image9]  | ![Final score][image3] |
| ![Final score][image10] | ![Final score][image4] |
| ![Final score][image11] | ![Final score][image5] |
| ![Final score][image12] | ![Final score][image6] |
|                         |                        |

## Conclusion

The resulting Semantic Segmentation works well, but not perfectly. Maybe it can be improved by refining the parameters used and/or using image improvement processes (augmentation).
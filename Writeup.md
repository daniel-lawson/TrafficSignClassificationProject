[//]: # (Image References)
[image1]: ./writeup_images/bar_chart.jpg "Bar Chart"
[image2]: ./Test_images/children_crossing.jpg "Children Crossing Sign"
[image3]: ./Test_images/no_vehicles.jpg "No Vehicles Sign"
[image4]: ./Test_images/priority_road.jpg "Priority Road Sign"
[image5]: ./Test_images/slippery_road.jpg "Slippery Road Sign"
[image6]: ./Test_images/speed_limit.jpg "Speed Limit 30km/h Sign"

# Writeup
## Data Set Summary adnd Exploration
I used the pandas and numpy libraries to calculate summary statistics of the traffic signs data set:
* The size of the training set is 34799
* The size of the validation set is 4410
* The size of the testing set is 12630
* The shape of a traffic sign image is 32x32x3
* The number of unique classes/labels in the data set is 43

Here is an exploratory visualization of the data set. It is a bar chart showing the frequency of the signs in the training set.

![alt text][image1]

## Design and Test a Model Architecture
To preprocess the images, I coverted to grayscale and used mean image normalization. Converting to grayscale will prevent the network from picking up color patterns. This is important because the network will not rely on the colors, so the picture could have been taken at any time of day or in any light. The mean image normalization sets all feature values from -1 to 1 instead of 0 to 255.  

My final model consisted of the following layers:

| Layer                 | Description                                        | 
|:---------------------:|:--------------------------------------------------:| 
| Input                 | 32x32x1                                            |
| Convoluition 5x5      | 1x1 stride, valid padding, outputs 28x28x6         |
| RELU                  |                                                    |
| Max Pooling           | 2x2 stride, outputs 14x14x6                        |
| Convoluition 5x5      | 1x1 stride, valid padding, outputs 10x10x16        |
| RELU                  |                                                    |
| Max Pooling           | 2x2 stride, outputs 5x5x16                         |
| Flatten               | Outputs 400                                        |
| Dropout               | Keep probability is 75%                            |
| Fully Connected       | Ouputs 120                                         |
| RELU                  |                                                    |
| Dropout               | Keep probability is 75%                            |
| Fully Connected       | Ouputs 84                                          |
| RELU                  |                                                    |
| Dropout               | Keep probability is 75%                            |
| Fully Connected       | Ouputs 43                                          |

To train the model, I set `EPOCHS = 15`, `BATCH_SIZE = 128`, `dropout = 0.75`, `mu = 0`, `sigma = 0.1`, and `rate = 0.00075`.

My final model results were:
* Training set accuracy of 99.0%
* Validation set accuracy of 95.1%
* Test set accuracy of 93.5%

In my first iteration of testing, I set `EPOCHS = 10` and `rate = 0.001`, which returned a validation set accuracy of 90.3%. I then decided to set `EPOCHS = 15` and `rate = 0.0001`, which returned a validation set accuracy of 77.4%. The accuracy rate was increasing past the 10th epoch, so I kept it at `EPOCHS = 15`. I then set `rate = 0.005` and got a validation set accuracy of 92.6%. Then, I added the dropout and initially set it to `dropout = 0.75`. Keeping `rate = 0.005`, adding the dropout gave me a validation set accuracy of 91.5%. I changed to `dropout = 0.5` and got a validation set accuracy of 87.6%. Because my accuracy was dropping, I decided to reverse some of my steps. I set `dropout = 0.75` and `rate = 0.00075`, and got a final validation set accuracy of 95.1%.

## Test Model on New Images
I found these 5 German traffic signs on Google:

![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]

Here are the results of the prediction:

| Image              | Prediction            | 
|:------------------:|:---------------------:| 
| Children Crossing  | Children Crossing     |
| No Vehicles        | No Vehicles           |
| Priority Road      | Priority Road         |
| Slippery Road      | Wild Animals Crossing |
| Speed Limit 30km/h | Speed Limit 30km/h    |

The model was able to correctly guess 4 of the 5 traffic signs, which gives an accuracy of 80%. This is lower than the accuracy on the test set of 93.5%. The incorrectly identified sign was the Slippery Road sign. This sign could have been difficult to identify because it features a fairly intricate shape of a car sliding. The other signs are easier because the shapes are relatively simple. The network mistook the intricate shape of the car for another detailed shape found in the wild animal crossing sign.

For the **Children Crossing** sign, the top five soft max probabilites were:
| Probability | Prediction                            | 
|:-----------:|:-------------------------------------:| 
| 0.99999690  | Children Crossing                     |
| 0.00000171  | Dangerous Curve to the Right          |
| 0.00000089  | Road Narrows on the Right             |
| 0.00000049  | Bicycles Crossing                     |
| 0.00000001  | Right-of-way at the Next Intersection |

The network was very confident that it was a Chidlren Crossing sign.

For the **No Vehicles sign**, the top five soft max probabilites were:
| Probability | Prediction          | 
|:-----------:|:-------------------:| 
| 0.99649800  | No Vehicles         |
| 0.00116325  | Speed Limit 50km/h  |
| 0.00114389  | No Passing          |
| 0.00070669  | Speed Limit 100km/h |
| 0.00018473  | Ahead Only          |

The network was very confident that it was a No Vehicles sign.

For the **Priority Road** sign, the top five soft max probabilites were:
| Probability | Prediction           | 
|:-----------:|:--------------------:| 
| 0.99912685  | Priority Road        |
| 0.00083303  | Yield                |
| 0.00002575  | Roundabout Mandatory |
| 0.00001272  | Keep Right           |
| 0.00000227  | No Passing           |

The network was very confident that it was a Priority Road sign.

For the **Slippery Road** sign, the top five soft max probabilites were:
| Probability | Prediction                  | 
|:-----------:|:---------------------------:| 
| 0.99973339  | Wild Animal Crossing        |
| 0.00024171  | Slippery Road               |
| 0.00002285  | Double Curve                |
| 0.00000180  | Bicycles Crossing           |
| 0.00000018  | Dangerous Curve to the Left |

The network was very confident that it was a Wild Animal Crossing sign, however, that was incorrect. The Slippery Road sign was its second guess with a probability of only 0.024%.

For the **Speed Limit 30km/h** sign, the top five soft max probabilites were:
| Probability | Prediction         | 
|:-----------:|:------------------:| 
| 0.99235392  | Speed Limit 30km/h |
| 0.00682669  | Speed Limit 20km/h |
| 0.00071048  | Speed Limit 50km/h |
| 0.00004223  | Keep Right         |
| 0.00003553  | Children Crossing  |

The network was very confident that it was a Speed Limit 30km/h sign.

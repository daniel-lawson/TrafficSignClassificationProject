[//]: # (Image References)
[image1]: ./writeup_images/bar_chart.jpg "Bar Chart"

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

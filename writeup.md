Write up for Traffic Sign Classifier Project
Author: Jorge Orbay

Files Submitted:
	Most of the software is found in the file called: Traffic_Sign_Classifier-Copy1.ipynb. Here, the network is built that trains using the file train.p for recognizing traffic signs. The validation set is inherited from the file valid.p. The test set is inherited from the file test.p. Refer to signnames.csv for information on the classes of signs used for  classifying.

Dataset Exploration.

   Basic facts of the number of training examples, testing examples, and image shapes are computed. We assume that the testing and training pictures are of the samee size. We find that:

   There are  34,799 training pictures
   There are 12,630 testing pictures
   Each picture is of 32x32 pixels
   There are 43 possible sign classifications

   To quickly visualize an example  picture, I chose a training picture at random and displayed it. I also plotted the number of training pictures that fall under each ClassID (sign category). I found that there is a large variance in the number of pictures for each category, some categories having 2000 training pictures while others having less than 500.

Design and Test a Model Architecture

   The only preprocessing done is to randomize the order in which the training set is used. This is done to avoid having the network recognize any patterns in the training set that won't be found in the test set. For example, if the training set pictures are all grouped into their respective categories, then during batch training, the network will be trained primarily on the first few sets. Because our optimizer uses a "smart" momentum, this may cause it to perform poorly on other classes.

   I have been trying to make rotated copies of the training set pictures and inserting those copies back into the training set. However, my software kept returning errors  when treating these copies. In my next  implementation of this project, I will be adding these rotate copies, as well as copies  with random noise.

   The model architecture was inspired by the VGG network architecture and used a LeNet Tensor Flow architecture from Udacity's Self Driving Car Course as a basis. 

   First Layer: A convolution with a 5x5x3 kernel that produced 16 filters and used a stride of [1,1,1]. This is followed by a relu activation function.

   Second Layer and Third Layer: Both layers are a  convolution with a 3x3x16 kernel and 3x3x20 kernel (respectively) that produce 20 filters and use a stride of [1,1,1]. Both are each followed by a relu activation function.

   Fourth Layer: Max pool function with a kernel of 2x2x1 and a stride of 2x2x1.

   5th, 6th, and 7th Layer: A repeat of the 2nd, 3rd, and 4th layers. Basically, two convolutions and another pool. 

   8th Layer: A fully connected layer with 120 filters. It is followed by a relu activation function.

   9th Layer: A fully connected layer with 84 filters. It  is followed  by a relu.

   10th Layer: A fully connected layer with 43 filters (the classes). A softmax function is run on the end filters. 

   The architecture influence  from VGG is most easily seen in how it layers convolutional layers of the same kernel and filter. It separates sets of these layers with max pooling functions. It also ends the architecture with a series of fully connected+relu layers.

   When training, the weights are first instantiated using tf.truncated_normal and use a mu of 0 and sigma of .1. These were artificats from the LeNet architecture and were kept because changes were not found to improve  results.

   The Adam Optimizer from tensorflow was used for optimizing. This is another artifact from the LeNet architecture. It was kept because all alternative optimizers were observed performing dramatically worse. All of the hyperparameters for the optimizer were kept at default values except epsilon, which was set to 1e-4. This was observed as helpful in getting the optimizer out of local minimums. The learning rate was .001, another artifact from the LeNet.

   The batch size was decreased to 128 from the LeNet basis because it improved the validation errors faster. Evidence from the Udacity Self Driving Nanodegree courses also showed that smaller batch sizes improved training time. 

   The number of epochs was increased to 30 to further improve the network's training. I feared that the network would overfit the training set, but the validation accuracy finishes at 95% while the testing accuracy finishes at  93.5%, showing that network hardly overfits the training set.


Test a Model on New Images

   The new images I found can be  found in the folder labeled "Internet_Pictures." I chose the clearest and brightest pictures I could find, which I hoped would be  easier to identify than the training set pictures (many of which are poorly lit). However, I did include 2 speed limit pictures  to see if the network could distinguish the two. Interestingly, it did identify both speed limit signs as speed limit signs, but it only correctly identified one of them.

   The network had a 60% accuracy with identifying my pictures obtained from online. It misread a "120 km/h" sign as "60 km/h", which I found impressively close. It also misread a "pedestrians" (classID = 27) as a speed limit sign. The pedestrians category was one of the smallest ones among the training set, and I am tempted  to believe this is the reason the network failed. 

   Obviously, 60% accuracy is much worse  than the testing set accuracy of the network. I believe my internet pictures are too small of a subset of pictures  to effectively compare  the accuracy rates, but I also believe that they look very different from the testing set. They are likely too brightly lit and too zoomed into the sign.

   The softmax probabilities show that all results were at least 98% certain, which means the network is over confident. However, the few times it was approximately 100% certain of its result, the result was  correct.

Thank you for reading and please feel free reach out to me at jao2154@columbia.edu. Have a good day!
    

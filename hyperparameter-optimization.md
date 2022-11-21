In this branch is the google colab .ipynb file containing the code for semantic segmentation with annealing optimization.

Explanation of code:

The first half of the code regarding the preprocessing of the data is the same procedure as the first semantic segmentation code in Milestone 2. A general overview of this process is that I first get the images and resize them to 256 by 256. Then I divide the pixel values by 255 so that the colors are between 0 and 1. Next, I get the masks and resize those images to 256 by 256. Lastly, I assign the colors an integer value and one hot encode them, so that I can begin the semantic segmentation.

Annealing method explanation:

In machine learning there are often 1 or more hyperparameters that when changed can affect the way the model performs when trained. Some hyperparameters can create an overfit in the model, and some can make the model train too slow. This balancing act is the reason why it is often a time consuming process to choose the correct hyperparameters. The hyperparameter optimization method that I use in my code is called annealing. This method is a combination of two hyperparameter choosing ideas which are random selection and iterative selection. Random selection is when hyperparameters are randomly chosen a certain amount of times and then the hyperparameters with the best model are selected. Iterative selection is when all combinations of a certain list of hyperparameters are run and then of course the hyperparameter with the best model is selected. These methods have problems with being either too inaccurate or too slow. Annealing gets the best of both worlds by trying a lot of different hyperparameter combinations while still being efficient. The method chooses a random state, and then changes one hyperparameter each time, and if the model is better than those hyperparameters are saved.

Annealing code implementation:

For my annealing method I was looking to optimize three hyperparameters which are learning rate, batch_size, and momentum. To begin the annealing process, I first needed an initial state to work off of. The initial state I chose was learning rate = 1, batch size = 8, and momentum = 0. I compiled the model and trained it using unet, and I got my starting model. I saved a couple of variables from this model, which are the accuracy, validation accuracy, and the hyperparameters. I saved the accuracies because these are the statistics I will be comparing to other models in order to see if the hyperparameters performed better or not. Next, I have a while loop that goes 10 iterations which means that I will choose 10 different states of hyperparameters and compare their accuracies. In each iteration I choose a random hyperparameter to change, and then a random value in that hyperparameter list to set it equal to. Following the annealing methodology, I only change that one hyperparameter and the other 2 will be the same as the state before. Once I have the tuple of hyperparameters, I check to see if this state has already been tested. If it has then I will check a new state. If it is a new state then I compile the model using the hyperparameters and train the model. The accuracy and validation accuracy of the model are compared to the best saved accuracy, and if the current model performed better, then I save that model as the best model. Once the while loop finishes, the hyperparameters that are saved in best state, are the ones that gave the best model. Finally, I take those hyperparameters and train a model one more time with the parameters on 100 epochs. This is the model that I use for my final graphs and segmentation images.

Results Obtained:

My first metric for analyzing the success of the training is the predicted mask.
![image](https://user-images.githubusercontent.com/69495267/203020753-a7c8fc46-b6ba-4512-9ff9-f16d719201cf.png)
![image](https://user-images.githubusercontent.com/69495267/203020819-1d842654-e144-4371-831c-9908b463455b.png)
![image](https://user-images.githubusercontent.com/69495267/203020835-cf96db42-1a1d-4041-a34a-0b7932642b51.png)
![image](https://user-images.githubusercontent.com/69495267/203020855-26d45804-c1a4-45b6-a69c-350befca9e71.png)
![image](https://user-images.githubusercontent.com/69495267/203020866-50c116c8-73d5-4bba-8db0-bd8c6111e6ad.png)
![image](https://user-images.githubusercontent.com/69495267/203020878-d46b1909-75dc-4142-9d05-7956044f511d.png)
![image](https://user-images.githubusercontent.com/69495267/203020896-cb491d58-47be-4748-b9f5-d68f96c1d883.png)
![image](https://user-images.githubusercontent.com/69495267/203020907-53c3eaf4-debe-4ead-aa20-e9c3e8f8be80.png)
![image](https://user-images.githubusercontent.com/69495267/203020923-2829c1de-2215-49d0-b016-67573e632af2.png)
![image](https://user-images.githubusercontent.com/69495267/203020941-f7ceca94-c454-4d18-ae00-bf6d22954e56.png)

In regards to the images obtained by the semantic segmentation using the annealing method, most of the images came out very close to their testing label counterpart. It seems that similar to the semantic segmentation plot without hyperparameter optimization, the hardest identifier to predict correctly was the road. For example in the second image, the road that is covered by the sand is completely missed. It is quite impressive that the images were able to come out close, if not better, to the images in milestone 2 which was trained using double the epochs.

My second metric for analyzing the success of the training is the training and validation loss graph.
![image](https://user-images.githubusercontent.com/69495267/203032274-e3002a75-bcf3-4f55-aff1-392c6812055e.png)

The val loss and loss vs epoch graph did not come out as pleasantly as expected due to an error in the training. The error was most likely caused by the learning rate hyperparameter which was 1. The learning rate I used in the previous milestone was 0.01, so it makes sense that the graph for lr=1 would not be as smooth. Nonetheless, the graph still follows a downward trend meaning that as the model iterates over more epochs, the model is learning from itself and lowering loss.

My third metric for analyzing the success of the training is the precision vs. recall graph.

![image](https://user-images.githubusercontent.com/69495267/203032398-c215ff20-4a86-4421-8c9a-64902b26371d.png)

Analyzing the precision vs recall graph produced from this model, it is apparent that a larger learning rate produces a much different graph. In the P vs. R graph from milestone 2, the graph was much closer to resembling a square root of x graph. This is most likely because the model trains slowly so the low accuracy causes low precision. In this precision recall graph, it is much more linear because the learning rate is taking much larger steps. A high precision and recall value are able to be obtained quicker; however, the precision suffers due to low accuracy. The recall value is also observed to be much higher because a higher learning rate would result in more values being accepted as truth values.

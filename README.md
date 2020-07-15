# ReinforcementLearning
I here share my winning code for the DeepRacer Amazon's Community Race, a competition I ranked 1st in out of 50 participants at IE HST.

**1. Initial approach**

I started with a basic model that had as a goal to only follow the center line. It completed a lap in an average of 3:30 on the leaderboard. I then improved the base model with the usage of other parameters: progress / track width / all_wheels_on_track and managed to get up to 1:22 on the leaderboard. Then, I played with the car parameter, increasing the speed and angle granularity and got up to 1:09 on the leaderboard.


**2. Winning approach**

The game changer was managing to use the waypoints or steering in an effective way. Another important point at this stage was managing to keep training time low.  After some research I found an answer to both of these in the following [blog by Falk Tandetzky](https://medium.com/twodigits/aws-deepracer-how-to-train-a-model-in-15-minutes-3a0dca1175fb) (I owe a lot to this approach and I would highly recommend to visit it for better explanation - his [github](https://github.com/TwoDigits/deepracer)). I used an implementation of the reward function explain there, which basically did the following: 

![](https://github.com/mohamedkhanafer/ReinforcementLearning/blob/master/images/technique.png)

The goal was to use the steering direction of the car by drawing a circle around it and incentivize it into the direction of the intersection of the circle and the central line. Basically, the reward function would give the car a larger score if the front wheels of the car are going towards this direction.

And on the second point mentioned about making the faster training happened because of the clear relation between the action done by the car and the reward. Because the clearer the connection between the actions of the cars and the reward, the quicker the car can learn. And so, I was able to train good solid models in around 30-60mins and get up to 57 seconds on the leaderboard.

**3. Fine-tuned approach**

The way to improve the reward function from there on was to fine-tune 2 aspects: the radius of the circle drawn around the car and finding a proper car to increase the speed.

Adjusting the radius of the drawn circle would allow the car to better adapt to the curves of the track. And finding the proper car would help improve the timing. I thus tried combinations of radiuses ranging from 0.2 to 2 times of the width of the track and cars with speed ranging from 0.5 m/s to 4 m/s. The higher speed cars needed more time to train and for time constraint purposes I focused on cars with speed between 2-3m/s. I here share some of my best models with their parameters and their evaluation:

![](https://github.com/mohamedkhanafer/ReinforcementLearning/blob/master/images/results.png)

**4. A note on the hyperparameters**

In my initial approaches, the main parameters I tweaked was the Learning rate but I noticed that the learning in many cases was not converging with a high learning rate. I thus later on used smaller learning rates and got better results for my models. In the next steps, I played with the discount factor, using a lower one because in the reward function implemented here, the focus is not on rewarding past actions but more on rewarding the position of the car.

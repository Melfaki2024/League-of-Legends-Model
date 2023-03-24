# League of Legends Model: Predicting The Winner

### In this project, we create a Decision Tree model in order to predict whether or not a player is going to be on the winning or losing side of a given game. This is done through the League of Legends data set of competitive match data from the 2022 season.

### Our exploratory data analysis on this dataset can be found here: [League of Legends Game Analysis](https://benjaminbestmann.github.io/League-of-Legends-Project/)

# Framing the problem

Since our prediction only has 2 options, we are using a binary classification to predict if a player will end up being on the winning or losing side. Our response variable is the result of each match (called result in the dataset) and we chose it because it depends on all other factors in the dataset. Furthermore, we are only utilizing information that is present or predating the timee of prediction (i.e when the winner is decided). Our final model utilized the following features: 

   - position: The position of a player on the team 
   - firstblood: Whether or not a player had the first kill
   - damagetochampions: The total amount of damage dealt to other champions (players)
   - visionscore: The amount of times your actions allowed your time to see otherwise unseen targets
   - damagemitigated: The amount of damage a player healed
   - golddiffat10: The difference between a player's gold at 10 minutes and their counterpart on the other team (i,e if a player's position is top, the other team's top would be that player's counterpart)
   - xpdiffat10:The difference between a player's xp at 10 minutes and their counterpart on the other team 
   - KDA: the ratio of Kills, deaths, and assists

Lastly, our metric of evaluation was accuracy (the proportion of predictions that are correct). THe reason we chose this metric over another like precision or recall is because, in our case, we don't necessarily weigh False positives or negatives more. 

# Baseline Model

For our baseline Model, we utilized a DecisionTreeClassifier with the following features: 

   - position: The position of a player on the team 
   - firstblood: Whether or not a player had the first kill
   - damagetochampions: The total amount of damage dealt to other champions (players)
   - visionscore: The amount of times your actions allowed your time to see otherwise unseen targets
   - damagemitigated: The amount of damage a player healed

While most of these features are comprised of quantitative data, two of them are different: firstblood while being quantitative is binary and positision is actually qualitative. Resultantly, we created a pipeline with a preprocesser that one hot encoded position (using OneHotEncoder from sklearn's preprocessing module) as well as standardized (using StandardScaler from sklearn's preprocessing module) visionscore, damagetochampions, as well as damagemitigated. We did not transform first blood as it was already binary. After training our model, we found that we were roughly 63% accurate om our training data and 62% accurate on our test data. While this may seem low, it is still good as a baseline considering that we avoided the issue of overfitting our model to the training data. 

# Final Model

For our Final Model, we utilized a DecisionTreeClassifier with the above features in addition to a few new ones: 

   - position: The position of a player on the team 
   - firstblood: Whether or not a player had the first kill
   - damagetochampions: The total amount of damage dealt to other champions (players)
   - visionscore: The amount of times your actions allowed your time to see otherwise unseen targets
   - damagemitigated: The amount of damage a player healed

   Additional Features:

   - golddiffat10: The difference between a player's gold at 10 minutes and their counterpart on the other team (i,e if a player's position is top, the other team's top would be that player's counterpart)
   - xpdiffat10:The difference between a player's xp at 10 minutes and their counterpart on the other team 
   - KDA: the ratio of Kills, deaths, and assists

The added features (golddiffat10, xpdiffat10 and KDA) because, for example, KDA is a very good indication of an attacking player's performance is throughout a game and as such, to their ability to win. Similarly, xpdiffat10 and golddiffat10, showcase how well a player is doing in comparison to their counterpart on the other team.

We performed transformations to the data to make it serviceable to our prediction needs. We one-hot encoded the categorical data to make it usable in our prediction, we standardized many individual metrics to avoid scaling issues within our predictions and binarized the difference in gold/xp around the 0.0 mark to quantify if the player was winning or losing their individual battles. Ultimately, the added context around our original features, like how well players were doing against their counterparts, helped make our model more robust and create more accurate predictions

In addition to including these new features, we also wanted to optimize our model by choosing the best possible hyperparameters. We did this by utilizng sklearn's GridSearchCV which utilizes K-fold validation in order to find the best hyperparameters which we found to be gini criterion, a max depth of 2, and a min sample split of 2. This all improved our model greatly as our new prediction scores were roughly 87% on both training and test data!


# Fairness Analysis

While our final model performed much better than our baseline one, we were still concerned with one thing: the fairness of our model between attacking players and defensive/supporting players. Our hypothesis were: 

   - Null Hypothesis: Our model is fair. Its accuracy for the sup position players and mid players are roughly the same, and any differences are due to random chance
   - Alternative Hypothesis:  Our model is unfair. Its accuracy for sup players is lower than its accuracy for mid players

To conduct this, we utilized a permutation test and our test statistic for this was difference in accuracy (mid players - sup players) with significance level of 5%. Our p-value ended up being more than 40% which meant we failed to reject the null hypothesis that our model is fair. While we cannot confirm if our model is indeed fair (as theses are statistical tests and not randomized controlled trials), this does support our predictions. 

# Conclusion

Our model's accuracy and the results of our fairness analysis point to our model being both effective and fair. 



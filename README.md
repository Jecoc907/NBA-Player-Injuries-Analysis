# NBA Player Injury Analysis And Future Implication
### Contributors: Nathan Cheou, Abed Kassem, Alberto Rivas II, Jeco Cheung, and Jiayi Jin

## Purpose
The purpose of this project is to try to predict the occurrence of injuries based on player statistics such as height, weight, usage, pace, and how fast they move. We will also be exploring the dataset of injuries to see what sort of insights we can extract. Some things we have in mind are what are the most common injury types, and what types of injuries tend to be season-ending, as opposed to just day to day injuries. 

## Background of the Datasets / Data Processing
We used a combination of two datasets that we found on Kaggle. The first dataset is a list of all inactive list activity from 2010 to 2020. It contains columns such as the date, the player, the team they were on, whether or not they were moved onto or off the inactive list, and what was the reasoning. This dataset will be known as “injuries.” Below is a sample of the dataset: 

![image](https://github.com/Jecoc907/NBA-Player-Injuries-Analysis/assets/71363412/e86d77b1-beb5-4e40-9b54-bc670b89a55b)

For this dataset, we started off by reviewing every row to determine the type of injury. Shown below is a list of the injuries we compiled. We then wrote a function that scans each row and if it detects a keyword that appears in our list of injuries, it will append that word to an empty list. After it scans all keywords, it will then create a new column called “injury_type” based on the list of injuries for that row. 

injuries = ['knee', 'shoulder', 'hip', 'ankle', 'elbow', 'foot', 'heel', 'hand', 'hamstring', 'back', 'groin', 'neck', 'head', 'wrist', 'calf', 'leg', 'concussion', 'Achilles', 'thigh', 'tibia', 'toe', 'adductor', 'chest', 'nose', 'forearm', 'gluteus', 'eye', 'patella', 'quadricap','rib','shin','pelvis','kneecap','orbital','thumb','spinal cord', 'tailbone','cervical','cornea','cheekbone','collarbone','jaw','flexor','forehead','fibula','oral','facial','abdominal','abdomen','heart','throat', 'quadricep', 'oblique', 'finger', 'hernia', 'abductor', 'tricep', 'bicep', 'lat', 'lung', 'face','biceps','triceps','pectoral','plantar','disc']

Next, we noticed that for injuries that were season ending, or the athlete was out indefinitely, the notes in the inactive list typically noted this. So we wrote another function, and if the notes contained the phrase “out indefinitely” or “out for season,” we classified those as a 1 in a new “Out Indefinitely” column, and 0 for less serious injuries. This is what the final injuries dataset looked like.

![image](https://github.com/Jecoc907/NBA-Player-Injuries-Analysis/assets/71363412/20d06c7c-da26-4c91-a34a-c3feeac680de)

The second dataset was a list of players along with some game statistics, and if they suffered an injury or not. Unfortunately, this dataset seemed to have a lot of depth stripped from the injuries, as it seemed to only list ankle, knee, and lower back injuries. It was for the years 2013 - 2023, and has statistics such as games played, minutes, player height and weight, rebounds, average running speed, and touches. Below is a sample of the dataset. This dataset will be known as “player stats.”

![image](https://github.com/Jecoc907/NBA-Player-Injuries-Analysis/assets/71363412/da25232c-c221-4028-82ac-c195f7f49d85)

We started off by removing any rows from the injuries dataset that did not obviously seem to be an injury. We removed rows that were players being activated or returning to the lineup, and rows where it simply said a player was placed on the inactive list with no further explanation.

In order to combine these datasets, we first extracted the season from the date column in the injury column to match up with the season column in the player stats dataset. Then, we left-joined the injuries dataset onto the player stats based on the player’s name, the season, and their team.

merged_df = pd.merge(df2, df_injuries_only, how = 'left', left_on=['PLAYER_NAME', 'SEASON', 'TEAM'], right_on=['Relinquished', 'SEASON', 'Team'])

The result is a dataset that contains all player stats, and then has extra columns for players who were injured detailing the date and type of their injury. Next, we noticed that some players had many rows for the same season listing the same injury. We believe that this was for injuries where the team added them to the inactive list and listed every game they missed, an example of this is “concussion: DNP”. In order to address these, we decided to drop duplicates based on player name, team, season, and injury type. 

## Exploratory Data Analysis
First, we wanted to start off by looking at a histogram of all of the columns in the player stats dataset. We can see that columns such as age, player height, and player weight are appropriate bell-curved. Overall, we don’t notice any outliers or discrepancies for this dataset.

![image](https://github.com/Jecoc907/NBA-Player-Injuries-Analysis/assets/71363412/96673e98-2c71-49e5-b967-e2e4d1500a2c)

![image](https://github.com/Jecoc907/NBA-Player-Injuries-Analysis/assets/71363412/38787ab1-dedb-4821-8c3b-b5a9a852841a)

![image](https://github.com/Jecoc907/NBA-Player-Injuries-Analysis/assets/71363412/f1ea41c0-a8ef-43f2-b013-d3b9fecb9b93)

The first thing we were interested in was looking at which injuries were the most common. It appears that ankle and knee injuries were the most common reasons for being placed on the inactive list. For a sport that involves so much running, it’s not surprising that lower body injuries are prevalent. 

![image](https://github.com/Jecoc907/NBA-Player-Injuries-Analysis/assets/71363412/f011dfe6-e1c3-4ced-8434-78fe5c2541a9)

This plot on the right shows whether or not someone sustained a lower body injury (typically ankle or knee) and their height and weight. Our hypothesis is that heavier players may sustain more of these joint injuries due to their increased mechanical loading. A visual inspection reveals that there does not appear to be a clear pattern with weight and lower body injuries.

![image](https://github.com/Jecoc907/NBA-Player-Injuries-Analysis/assets/71363412/bf39b929-8f0f-449d-9b85-edb98d5d2282)

Next, we looked at injuries by number of years, and we see a surprisingly steady increase in injuries year over year from 2013 to 2019. This is surprising because, as the NBA has become less physical over the years, we would expect injuries to decrease. This may be because the injuries are occurring off the court, or during practice. 

![image](https://github.com/Jecoc907/NBA-Player-Injuries-Analysis/assets/71363412/14415a15-c825-48ff-b53e-51c9cfd1883b)

We can see that, aside from a spike in August, most injuries appear to occur during the actual NBA season from October to April, with the plurality of injuries happening from November to December.

![image](https://github.com/Jecoc907/NBA-Player-Injuries-Analysis/assets/71363412/c8d2e1eb-c589-4366-b1a6-8e7424651509)

Here, we can see that although shoulder injuries were only the 8th most common injury, they have the highest chance of resulting in an athlete being taken out for the season. Coaches should do their best to prevent shoulder and achilles injuries in their players. 

![image](https://github.com/Jecoc907/NBA-Player-Injuries-Analysis/assets/71363412/9dcd8e3b-06dd-4988-b69b-02ef17a807ab)

Lastly, we categorized each specific body part injuries into more general areas, such as combining ankle, knee, and shin into a broad “leg injury.”  Overall, we found that athletes sustained more leg injuries than every other kind of injury combined. 

![image](https://github.com/Jecoc907/NBA-Player-Injuries-Analysis/assets/71363412/10454a7e-ade1-4f70-b601-b760ec9d3e87)

## Models Building / Performance
After conducting our exploratory analysis, we implemented different statistical models to predict the occurrence of player injuries. Since we are predicting a binary outcome (a dummy variable is created in our dataset, where '1' indicates an injured player and '0' indicates a non-injured player), we are using Logistic Regression and Decision Tree Classification to approach this problem. Before running any statistical models, we found that there are a number of Null values in our independent variables. We opted for imputation (replacing NaN values with mean or median) to solve this problem. 

### Logistic Regression
Initially, we built a logistic regression model on 'Injured' with all numerical variables to obtain an overview of the significance of coefficients of each variable. From our statsmodel summary and variance inflation factor (VIF) report, we identified the problem of multicollinearity (high VIF variables) and non-statistically significant variables (high p-values). We, therefore, found the need for selecting explanatory variables and chose the stepwise method.

![image](https://github.com/Jecoc907/NBA-Player-Injuries-Analysis/assets/71363412/d4501a7a-8b14-4afc-a7e6-1754bd5b34e5)

### Feature Selection using Stepwise Method
Stepwise regression is a data-mining tool that uses statistical significance to select the explanatory variables to be used in a multiple-regression model. In our model, a bi-directional stepwise procedure, which is a combination of forward selection and backward elimination, was utilized to obtain our final group of features.
Resulting features:
['DIST_MILES', 'PACE', 'PAINT_TOUCHES', 'AGE', 'POSS', 'AVG_SPEED', 'GP', 'DRIVE_FGA', 'PLAYER_HEIGHT_INCHES']

### Final Logistic Regression Result
![image](https://github.com/Jecoc907/NBA-Player-Injuries-Analysis/assets/71363412/9e7ab877-fe8f-4b66-a232-fe9197f61c19)

### Confusion Matrix
![image](https://github.com/Jecoc907/NBA-Player-Injuries-Analysis/assets/71363412/cce8a716-4ef5-46c1-b41b-206d50255cb5)

### Result
With the new set of features, we had successfully built a logistic model with all features’ p-value under alpha (0.05). Among all the independent variables, we learned that "Dist_Miles", which is the amount of distance a player has traveled in miles, is the most important factor with a coefficient of 2.3752. In other words, it means that for every extra mile a player has traveled, the log-odds of the player getting injured increase by 2.3752 units (which is e^2.3752≈10.753), assuming all other variables in the model remain constant. To assess the accuracy of our model prediction, we can look at the confusion matrix. Out of 855 testing data, our model correctly predicted 626 cases, which is equivalent to a promising result of 73.22%.

### Comparing Non-Linear Methods
Our next step was to look for non-linear relationships among the data.  Hence, we used decision tree classifiers and multiple ensemble methods to try and find the ensemble method that produced the highest accuracy score.  The first step was to use a base classifier with the GridSearchCV function to find the optimal hyperparameters for a base decision tree classifier.  Here is the resulting best tree as well as optimal hyperparameters:

![image](https://github.com/Jecoc907/NBA-Player-Injuries-Analysis/assets/71363412/875716ad-85f6-4db4-bd30-9130c4158581)

Hyperparameter	Value
Criterion: Gini
max_depth: 3
min_samples_leaf: 2
min_samples_split: 2

The resulting tree also had an accuracy score of approximately 76.95%, which is slightly better than our logistic regression models discussed earlier.  Hence, a non-linear model may be better suited for explaining the variance in the data and therefore predicting whether or not a player is injured.  This tree also helps us identify the most important variables to be aware of when determining things like player load and which factor(s) are key for determining whether or not a player gets injured.  Based on the tree above, we can see that variable X[12] corresponds to the most important factor.  X[12] is the amount of distance a player has traveled in miles.  The tree also indicates that this is a positively correlated variable, meaning that the more miles a player runs the more likely they are to become injured.

We also decided to use three ensemble methods: bagging, random forest, and XGBoost classification in order to try to increase our model accuracy.  Below is a comparison of our three classification methods organized by accuracy score.

Model	Accuracy Score

Sklearn Bagging Classifier	94.26%

XGBClassifier	93.09%

Sklearn DecisionTreeClassifier	76.95%

Sklearn RandomForestClassifier	75.08%

As you can see, the bagging classifier had the best score with 94.26%.  This score is much higher than the base score and our logistic regression.  Here is the confusion matrix for the bagging classifier.

![image](https://github.com/Jecoc907/NBA-Player-Injuries-Analysis/assets/71363412/a729e8fc-8d3a-48d2-bddf-825d0d51030d)

As you can see, the model does slightly prefer to classify players as not injured as opposed to injured, which is most likely a result of our uneven class label distribution.  Overall, however, the accuracy score is extremely strong and therefore can be used to classify players accurately.


### Conclusion
Our exploratory analysis identified key injuries to look out for, such as lower body injuries being the most common and shoulder injuries being the most common season-ending injuries.  This is important when looking at players who are not injured to ensure that they take extra care of the parts of their bodies that are most likely to result in season ending injuries.

Based on our decision tree models, the amount of distance a player covers is highly correlated with injuries.  This is important for team staff to consider when looking at player load management in order to ensure that players do not strain their bodies to the point where they are at high risk for an injury.  Since our data only shows game data, it is important for teams to track this at practice as well to ensure that players do not stress themselves too much in practice and get injured during a game et vice versa.

If we were communicating our results to the team’s sports scientist, we would recommend using the Bagging model we produced, as it has the highest accuracy. However, if we were speaking to a less technical audience, such as the coaches or GM, we would showcase the base decision tree model. Although less accurate, the diagram it produced is more interpretable and more clearly shows the link between injuries, miles run, drives, and touches.

### Limitations and Improvements
One of the limitations of the dataset was that we only had 7 years worth of data. We would have liked more, to perform a more comprehensive analysis of how injuries evolve over time. Another limitation is with our methodology for dropping duplicate injuries. Our method means that if a player sustained an injury to the same body part in the same season, it would count as a duplicate injury, even if they injured their ankle at the start of the season, and once again at the end. Sometimes players might have injuries in different body parts in one incident. Unfortunately we were not able to split that and this might lead to  inaccuracies in injury severity categorization and subsequent analysis. Additionally, sometimes a player was placed on the inactive list due to a surgery or recovery from a previous injury, or even one that may have been sustained off the court. Also, the accuracy and consistency of injury reporting across different teams and seasons may vary, affecting the reliability of the injury dataset. Overall, the datasets primarily focus on player statistics and injury occurrences without providing contextual information such as the circumstances leading to injuries, player conditioning, or external factors like game intensity or opponent strategies. Without such context, the analysis may overlook important factors influencing injury risk. Another limitation lies in the limited data about the specific mechanism of the injuries. While the dataset might refer to the body part affected, it doesn’t provide details about how the injury occurred. Understanding the mechanism, such as landing awkwardly after a jump or colliding with another player, can be vital for identifying specific risk factors. This missing information could affect the model's ability to fully involve the factors contributing to player injuries.

For future implementation, our model can be used to predict board level injuries based on players’ stats. Teams can use the model as a start point for injury prevention. One of the improvements for the model can be expanding the injury classification system to include a wider range of injury types and severity levels. Further natural language processing can help extract and categorize injury information from unstructured text data more comprehensively. Besides adding more data from public stats, teams should add physiological data such as muscle strength, flexibility, and biomechanical factors collected from wearable sensors or biomechanical assessments to improve the model’s performance. Collaborating with other professionals like trainer, coach and biomechanics experts to incorporate domain expertise into the analysis can help control the external factors and develop evidence-based injury prevention protocols tailored to basketball players. 


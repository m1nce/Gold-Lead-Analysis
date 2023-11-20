# Gold-Lead-Analysis

# League of Legends Gold Lead at 10 Minutes Analysis

by Minchan Kim and David Moon

This project was made for DSC80 at UCSD where we took in a dataset from professionally played League of Legends matches in 2022 to clean, analyze, and create permutation tests on the data. This website was purposely made to report what we have done throughout this project.

---

## Introduction

In this project, we 

---

## Cleaning and EDA



### Univariate Analysis:

<iframe src="assets/gold-diff-allteams.html" width=800 height=600 frameBorder=0></iframe>

In this histogram, we can see all the different values of gold leads each team had when they did have a gold lead. Since the mark is at 10 minutes, the histogram is skewed right because there can be very even or very small advantages at this time.

<iframe src="assets/earned-gold-majors.html" width=800 height=600 frameBorder=0></iframe>

In this histogram, we can see that there is a very rough bell curve with many teams earning about 35 to 40k gold for many of the matches but more accounts of teams earning less than about where the median is.

### BiVariate Analysis:

<iframe src="assets/win-percent-with-gold-lead.html" width=800 height=600 frameBorder=0></iframe>

Based on this pie chart, we can see the win percentage of games of those who had a gold lead at the 10-minute mark. As we can see, having a gold lead had a higher likelihood of winning the match.

<iframe src="assets/leagues-won-with-gold-10.html" width=800 height=600 frameBorder=0></iframe>

In this bar graph, we can see the different probabilities of winning a match if they have a gold lead divided by each league. We can see that the teams in the VCS and PCS leagues have the highest probability of winning if they have a gold lead at 10 minutes.

### Interesting Aggregates

print(position time).markdown(index=False)



---

## Assessment of Missingness

### NMAR Analysis
As we were first analyzing the data, we noticed that each game had 12 rows corresponding to that one game. We know this because if we grouped by 'gameid' we would see that each 'gameid' had 12 rows. This makes sense because each team has five players and the whole team combined (distinguished by team name). So in total, there would be 12 rows per game, 6 from one side, and 6 from the other side. Because of the way the data was collected, there will be some columns that will have missing values for both the individual players as well as for the team rows. One prime example of this would be the 'player name' column. It is certain that all five players will have their own individual player name but the team row would be missing because a team cannot have a singular player name. This is just one of many columns and it shows that the missingness of that data can come from the type of column. Because the missingness depends on the actual values themselves i.e. the team row is missing a value in the 'playername' because a team cannot have one player name, we can conclude that a lot of columns in our dataset are Not Missing At Random (NMAR).
In addition to some of the data being NMAR, we also have other columns dependent on other columns. Specifically, we see some columns depend on the column 'league'. In other words. This means that some leagues will record certain data columns while other leagues would not. One example would be the 'golddiffat10' column. We see leagues like the LPL,LDL, and DCUP not recording the gold differential at 10 minutes into the game in their games. In order to make it so that the columns do not depend on the 'league', we can find the gold differential for each game played in that league and record it in order to make it so that the 'golddiffat10' column does not depend on the 'league'. If we do this, we can make our column move more towards MAR rather than NMAR as 'gold iffat 10' would not depend on any other column


### Missing Dependency



## Hypothesis Testing

**Permutation Testing**

*Our original question was:* Does the team having a gold lead at 10 minutes have a higher likelihood of winning any given match?

Going back to our investigation topic, we are investigating whether it is true that there is an increasing trend of preference to food with higher calories in recent years of the last decade. So far, we have already seen an increasing trend in the usage of the proportion of recipes with high calories from 2008 to 2018, but observation alone cannot be a good indicator as to whether this trend shows actual change in people’s preference or is the trend merely coincidental. We determined that a good way to test the truthfulness of the trend is to take the distribution at the start and end of the decade for comparison. Hence, we are going to conduct a permutation test on the distribution of people’s comments on food with different calorie levels in 2008 and the distribution in 2018 to see whether there is an actual increase in preference to higher calorie food or not.

**Null Hypothesis**: Looking at tier-one professional leagues only, in the population, the distribution of win rates in games where teams had a gold lead at the 10-minute mark is the same as the distribution of win rates in games where teams did not have a gold lead at the 10-minute mark, and the observed differences in our samples are due to random chance.

**Alternative Hypothesis**: Looking at tier-one professional leagues only, in the population, a team having a gold lead at the 10-minute mark has an average higher winrate than a team not having a gold lead. The observed difference cannot be explained by chance alone.

**Test Statistic**: Difference in groups of means. We would use a difference of means because while at first, it can be thought that we should use Total Variation Distance as we are dealing with two categorical pieces of data `'result'` and `'positive'`. However, this would not be the case because we take the meaning of `'result'` turning it into a probability, which is a numerical piece of data.

**Winrate with Gold lead - Win Rate without Gold lead**

**Significance Level**: We decided to use a significance of 5% or alpha = 0.05

<iframe src="assets/permutation.html" width=800 height=600 frameBorder=0></iframe>

**Conclusion**: Under the null hypothesis, we can reject the fact that these two groups come from the same distribution. Since our p-value had a value of 0.0 and the significance level was at 0.05, we are able to conclude this.

Our conclusion could be reasonable because having a gold lead at the 10-minute mark can signify a snowball effect. If you are not aware of the snowball effect, think of a snowball rolling down a hill. While the snowball might start small, as it rolls down the hill, it gets much much bigger. Similarly, when a player or a team gains an initial advantage (by first blood, first dragon, herald, higher CS score, etc.) and then leverages that advantage to secure further leads and control over the game. Ultimately, having even a small gold lead at the ten minute mark can be used in the teams' advantage and can take over the game by being able to get even stronger by forcing favorable teamfights, being able to get dragons and dragon souls, or being able to get Baron Nashor. Overall, we believe that this conclusion is very reasonable because having a gold lead at the ten minute mark can signify who has more control of the game and has a higher potential to win the match.





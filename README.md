# League of Legends Gold Lead at 10 Minutes Analysis

by Minchan Kim and David Moon

This project was made for DSC80 at UCSD where we took in a dataset from professionally played League of Legends matches in 2022 to clean, analyze, and create permutation tests on the data. This website was purposely made to report what we have done throughout this project.

---

## Introduction

In League of Legends, the objective of the game is to destroy the other team's nexus. Although it seems simple on paper, teams must gain advantages in the game in order to overpower their opponents. The most important essence in the game is gold as it is both quantifiable and create a near immediate impact on the game through buying items that will allow a player's character to become stronger.
Although it is widely recognized that gold is important, we wanted to measure the extent of when gold will begin to create an impact on the game, namely through a quantifiable variable: winrate.
To answer this question, we used data from competitive matches that took place in 2022. Although there are many metrics that can measure gold, we identified `'golddiffat10'` as the most interesting column. Since League of Legends games usually take at least 25 minutes on average, we wanted to see if the gold differential at 10 minutes between both teams and players had a significant impact on winrate. As 10 minutes is barely just the start of the game, we determined that other League players would likely have the same thoughts as gold differential usually impacts the most during the 14-15 minute marks.
In addition, we decided to test missingness on the `'baron'` column since data may just not be collected by the league or the data may be collected in a way that when no Baron Nashors are taken, the data may be missing.

---

## Cleaning and EDA

We decided to use 'gameid', `'league'`, `'side'`, `'position'`, `'result'`, `'golddiffat10'`, `'golddiffat15'`, `'earnedgold'`, and `'barons'` as our columns.
-`'gameid'`: unique ID of the game
-`'league'`: leagues in which games take place
-`'side'`: contains red or blue team, each taking different parts of the map.
-`'position'`: predefined role of the player in the game; separated into five roles. (top, jungle, mid, bottom, support)
-`'golddiffat10'`: differential of gold earned between teams during 10 minutes of the game. contains data on each roles' and teams' differential respectively.
-`'golddiffat15'`: similar to `'golddiffat10'`, but at 15 minutes of the game.
-`'earnedgold'`: earned gold throughout the course of the game.
-`'barons'`: number of baron nashors slayed throughout the course of the game.

Then, we created four new columns - `'positive'`,`'major_league'`, `'is_missing_10'`, and 'baron_missing' - all filled with boolean values.
-`'positive'`: if `'golddiffat10'` is positive (x>0)
-`'major_league'`: if the game took place in a major league (LCK, LPL, LEC, LCS, PCS, VCS, LJL, CBLOL, LLA)
-`'is_missing_10'`: if `'golddiffat10'` is missing
-`'baron_missing'`: if `'barons'` is missing

After getting only the needed columns and creating new columns to help with our analysis, we removed rows in the data in which games did not have a result, meaning that the data showed that both teams had lost.



### Univariate Analysis:

<iframe src="assets/gold-diff-allteams.html" width=800 height=600 frameBorder=0></iframe>

In this histogram, we can see all the different values of gold leads each team had when they did have a gold lead. Since the mark is at 10 minutes, the histogram is skewed right because there can be very even or very small advantages at this time.

<iframe src="assets/earned-gold-majors.html" width=800 height=600 frameBorder=0></iframe>

In this histogram, we can see that there is a very rough bell curve with many teams earning about 35 to 40k gold for many of the matches but more accounts of teams earning less than where the median is.

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
As we were first analyzing the data, we noticed that each game had 12 rows corresponding to that one game. We know this because if we grouped by `'gameid'` we would see that each `'gameid'` had 12 rows. This makes sense because each team has five players and the whole team combined (distinguished by team name). So in total, there would be 12 rows per game, 6 from one side, and 6 from the other side. Because of the way the data was collected, there will be some columns that will have missing values for both the individual players as well as for the team rows. One prime example of this would be the `'playername'` column. It is certain that all five players will have their own individual player name but the team row would be missing because a team cannot have a singular player name. This is just one of many columns and it shows that the missingness of that data can come from the type of column. Because the missingness depends on the actual values themselves i.e. the team row is missing a value in the `'playername'` because a team cannot have one player name, we can conclude that a lot of columns in our dataset are Not Missing At Random (NMAR).
In addition to some of the data being NMAR, we also have other columns dependent on other columns. Specifically, we see some columns depend on the column `'league'`. In other words. This means that some leagues will record certain data columns while other leagues will not. One example would be the`'golddiffat10'` column. We see leagues like the LPL,LDL, and DCUP not recording the gold differential at 10 minutes into the game in their games. In order to make it so that the columns do not depend on the 'league', we can find the gold differential for each game played in that league and record it in order to make it so that the `'golddiffat10'` column does not depend on the `'league'`. If we do this, we can make our column move more towards MAR rather than NMAR as `'golddiffat10'` would not depend on any other column


### Missing Dependency



## Hypothesis Testing

**Permutation Testing**
*Our original question was:* Does the team having a gold lead at 10 minutes have a higher likelihood of winning any given match?

Going back to our original question, we wondered if having a gold lead would result in a higher likelihood of winning a match. We looked at the win rates in our EDA but we wanted to see if that was just a fluke or by chance.. So, we are going to conduct a permutation test on the distribution of win rates in games where teams who had a gold lead at the 10-minute mark are the same as teams who did not have a gold lead, specifically in the tier-one leagues.

**Null Hypothesis**: Looking at tier-one professional leagues only, in the population, the distribution of win rates in games where teams had a gold lead at the 10-minute mark is the same as the distribution of win rates in games where teams did not have a gold lead at the 10-minute mark, and the observed differences in our samples are due to random chance.

**Alternative Hypothesis**: Looking at tier-one professional leagues only, in the population, a team having a gold lead at the 10-minute mark has an average higher rate than a team not having a gold lead. The observed difference cannot be explained by chance alone.

**Test Statistic**: Difference in groups of means. We would use a difference of means because at first, it can be thought that we should use Total Variation Distance as we are dealing with two categorical pieces of data `'result'` and `'positive'`. However, this would not be the case because we take the meaning of `'result'` and turn it into a probability, which is a numerical piece of data. We would use the test statistic: Winrate with Gold lead - Win Rate without Gold lead

**Significance Level**: We decided to use a significance of 5% or alpha = 0.05

<iframe src="assets/permutation.html" width=800 height=600 frameBorder=0></iframe>

**Conclusion**: Under the null hypothesis, we can reject the fact that these two groups come from the same distribution. Since our p-value had a value of 0.0 and the significance level was at 0.05, we are able to conclude this.

Our conclusion could be reasonable because having a gold lead at the 10-minute mark can signify a snowball effect. If you are not aware of the snowball effect, think of a snowball rolling down a hill. While the snowball might start small, as it rolls down the hill, it gets much much bigger. Similarly, when a player or a team gains an initial advantage (by first blood, first dragon, herald, higher CS score, etc.) and then leverages that advantage to secure further leads and control over the game. Ultimately, having even a small gold lead at the ten-minute mark can be used to the teams' advantage and can take over the game by being able to get even stronger by forcing favorable team fights, being able to get dragons and dragon souls, or being able to get Baron Nashor. Overall, we believe that this conclusion is very reasonable because having a gold lead at the ten-minute mark can signify who has more control of the game and has a higher potential to win the match.




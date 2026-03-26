**🦉 A Machine Learning Approach to User Retention at Duolingo**

**About Duolingo**

Duolingo was founded in 2011 by Luis von Ahn and Severin Hacker with a mission that was both simple and ambitious: make language education free, accessible and fun for everyone. What started as a side project out of Carnegie Mellon University has grown into the world's most downloaded education app, with over 500 million registered users across more than 40 languages.
The platform is built on a gamification model where users earn XP points, maintain daily streaks, compete in leagues and unlock achievements as they progress through bite-sized language lessons. This approach transformed what was once a dry academic exercise into something that feels closer to a mobile game than a classroom.
Duolingo went public on the Nasdaq in July 2021 and today generates the majority of its revenue through Duolingo Super, a premium subscription that removes ads and unlocks additional features. This means user retention is not just a product goal but a direct revenue driver. Every user that stays is a potential subscriber. Every user that leaves is revenue that walks out the door.

**The Business Problem**

Despite its enormous user base, Duolingo faces a challenge that every consumer app eventually confronts: churn. A large portion of users who sign up never make it past the first 30 days. They download the app, complete a few lessons, lose their streak and quietly disappear.
This pattern is costly. Acquiring a new user through marketing and app store visibility is expensive. Losing that user within a month means the acquisition cost was wasted entirely. Even a marginal improvement in 30-day retention translates into millions of dollars in retained subscription revenue and a healthier daily active user to monthly active user ratio, which is one of the most closely watched metrics by Duolingo's investors.
The core question this project sets out to answer is:
> What behavioral signals predict whether a Duolingo user will churn within 30 days, and which user segments are most at risk?
> 
Answering this question gives Duolingo's product and growth teams the intelligence they need to intervene at the right moment, with the right message, for the right user.

**My Role**

I approached this project as a junior data analyst with a data science background. My responsibility was to take raw user behavioral data across sessions, notifications and engagement history and turn it into actionable intelligence that a product team could actually use.
This was not just a modeling exercise. The goal was to tell a clear story with the data, one that starts with understanding who Duolingo's users are, moves through how they behave, and ends with a prediction engine that identifies at-risk users before they leave.
The work covered the full analytical pipeline: data preparation, exploratory analysis, feature engineering, machine learning modeling and the translation of technical findings into plain language recommendations.

**Dataset Overview**

The dataset used in this project was synthetically generated to mirror the structure of a real Duolingo product analytics environment. It contains four interconnected tables that together paint a full picture of user behavior.
| Table | Rows | Description |
|---|---|---|
| users | 500 | User profiles including country, device, language studied, age group and subscription status |
| sessions | 4,700 | Individual learning sessions with duration, lessons completed, XP earned and streak data |
| notifications | 6,655 | Push notifications sent to users including type, open rate and whether they resulted in a session |
| churn labels | 500 | The outcome variable showing which users churned within 30 days of their last session |


All four tables are connected through a shared user ID column. The combined master dataset used for analysis contains 500 users across 15 features after merging and feature engineering.

**Tools and Libraries**

| Tool or Library | Purpose |
|---|---|
| Python 3.10 | Core programming language |
| Pandas | Data loading, merging and manipulation |
| NumPy | Numerical operations |
| Matplotlib / Seaborn | Data visualization and charts |
| Scikit-learn | Model preprocessing, train/test split and evaluation metrics |
| XGBoost | Primary churn prediction model |
| Google Colab | Development environment |
| GitHub | Version control and project hosting |

**Methodology**

Step 1: Data Loading and Merging

The four tables were loaded separately from an Excel workbook and then merged into a single master dataset using the user ID as the shared key. Before merging the sessions and notifications tables, both were summarized at the user level since a single user can have multiple sessions and notifications. This produced one clean row per user containing aggregated behavioral features.

Step 2: Data Cleaning
The master dataset was inspected for missing values, duplicate rows and incorrect data types. No missing values or duplicate records were found. The following data type corrections were applied:
 * The churned 30d and is paid columns were converted from boolean True or False values to integer 1 and 0.
 * The signup date and last active date columns were converted from text objects to proper datetime format.
 * The total sessions column was also corrected from boolean to integer.
   
Step 3: Exploratory Data Analysis
Before building any model, the data was explored visually to understand the distribution of churn across different user segments. Nine charts were produced covering overall churn rate, churn by country, churn by device type, churn by age group, churn by language studied, churn by league tier, streak length versus churn, notification open rate versus churn and a full correlation matrix.

Step 4: Feature Engineering
Session and notification data were summarized per user to produce the following features: total sessions, average session duration, average lessons completed per session, total XP earned, maximum streak length, average streak length, total notifications received, notification open rate and notification to session conversion rate.

Step 5: Categorical Encoding
Text columns including country, device type, language learned, age group, league tier and risk segment were converted to numeric values using label encoding so that the machine learning model could process them.

Step 6: Model Training and Evaluation
An XGBoost classifier was trained on 80 percent of the data and tested on the remaining 20 percent. The model was evaluated using accuracy, precision, recall, F1-score and AUC-ROC score. Feature importance scores were extracted to understand which variables drove the predictions most strongly.

**Exploratory Data Analysis: Findings and Insights**

**Overall Churn Rate:**
Out of 500 users, 268 churned within 30 days and 232 remained active. This gives an overall churn rate of 53.6 percent. The dataset is well balanced between churned and retained users which is ideal for training a classification model.
Insight: More than half of users leave within a month. This is not a minor retention issue. It is a structural problem that requires targeted intervention at the earliest stages of the user journey.

**Churn by Country:**
Korea recorded the highest churn rate followed by the United Kingdom, India, France, Japan, the United States, Mexico, Brazil, Germany and Australia.
Insight: The variation across countries suggests that localization may be a factor. Users in markets where Duolingo has less cultural resonance or where competing language learning tools are more popular may be quicker to disengage.

**Churn by Device Type:**
Web users churned the most at 55.7 percent, followed by Android at 54.7 percent and iOS at 52.1 percent.
Insight: Web users are the least committed. They are likely casual visitors who landed on the platform without the intentionality that comes with downloading an app. iOS users show marginally stronger retention, possibly due to the effectiveness of Apple's native notification system in re-engaging lapsed users.

**Churn by Age Group:**
Users aged 45 and above showed the highest churn rate. This was followed by the 18 to 24 age group, then 13 to 17 year olds, with the 25 to 34 and 35 to 44 groups showing the strongest retention.
Insight: The 25 to 34 and 35 to 44 segments are Duolingo's most loyal users. These are people who are learning a language with a clear purpose, whether for career advancement, travel or relocation. That sense of personal motivation keeps them coming back. Younger users are more easily distracted and older users may find the gamified format less intuitive or engaging.

**Churn by Language Studied:**
Japanese showed the highest churn rate followed by German, Portuguese, French, Korean, Italian, Mandarin and Spanish which had the lowest churn rate.
Insight: Language difficulty is a churn driver. Japanese and German are among the most structurally complex languages for English speakers. Users who hit a difficulty wall early and feel like they are not progressing are more likely to disengage. Spanish, on the other hand, benefits from widespread cultural familiarity particularly in the United States, which keeps motivation higher for longer.

**Churn by League Tier:**
The top three league tiers by churn count were Bronze at 35.9 percent, Silver at 35.2 percent and Gold at 29.0 percent. Together Bronze and Silver account for 71 percent of all churned users.
Insight: Early league placement is a significant churn risk. New users placed in competitive environments before they have built confidence or habits are more likely to feel discouraged and leave. Duolingo should consider a more gradual competitive ramp-up for new users to give them time to develop engagement before introducing pressure.

**Streak Length vs Churn:**
Users who stayed had an average maximum streak of 8 days. Users who churned had an average maximum streak of just 3.1 days.
Insight: This is the single most important finding in the entire analysis. Getting a user past a 7-day streak dramatically reduces their likelihood of churning. The first week of a user's journey is the most critical window for retention. Any intervention strategy should be concentrated here.

**Notification Open Rate vs Churn:**
Users who stayed opened their notifications at a higher rate than users who churned. This confirms that push notifications are an effective retention tool when users engage with them.
Insight: Notifications work, but only if users open them. The challenge is sending the right type of notification at the right moment. Streak reminders and streak freeze offers sent within the first hour of a missed session show the strongest potential for re-engagement.

**Correlation Matrix:**
The correlation analysis revealed several notable relationships. Total sessions and total XP showed a very strong correlation of 0.98, meaning they are essentially measuring the same user behavior. Maximum streak and average streak were also highly correlated. Notification count and open rate showed a moderate correlation of 0.78.
Insight: Because total sessions and total XP are so closely related, only one needs to be included in the model to avoid redundancy. The strong streak correlations confirm that streak behavior is one of the most internally consistent signals in the dataset.

**Machine Learning Model: Churn Prediction**

**Model Choice:**
An XGBoost classifier was selected as the primary model. XGBoost is a gradient boosted tree algorithm that builds an ensemble of decision trees sequentially, with each tree correcting the errors of the previous one. It performs well on structured tabular data and handles imbalanced classes effectively.


**Model Configuration:**
The model was trained with 200 estimators, a maximum tree depth of 4 and a learning rate of 0.05. The data was split 80 percent for training and 20 percent for testing with stratification to preserve the churn ratio in both splits.
Model Performance
| Metric | Score |
|---|---|
| Overall Accuracy | 75% |
| Precision (Churned) | 0.77 |
| Recall (Churned) | 0.76 |
| F1-Score (Churned) | 0.77 |
| AUC-ROC Score | 0.749 |
The model correctly identified 75 out of 100 users as either churned or retained. When it flagged a user as a churn risk it was correct 77 percent of the time. For a dataset of 500 users this is a strong baseline performance.

**Confusion Matrix:**
| Actual vs Predicted | Stayed | Churned |
|---|---|---|
| Actually Stayed | 34 correct | 12 wrong |
| Actually Churned | 13 wrong | 41 correct |
The model caught 41 out of 54 users who actually churned. Only 13 churned users slipped through undetected. This is a strong result given the dataset size and confirms the model is well calibrated for the problem it is solving.

**Feature Importance:**
The three most important features driving churn predictions were total XP earned, language studied and device type.
 * Total XP was the strongest predictor. Users who accumulated very little XP barely engaged with the platform before leaving. Low XP is essentially a signal of minimal investment.
 * Language studied confirmed the EDA finding that harder languages drive higher churn. The model learned to weight language difficulty as a meaningful signal.
 * Device type reinforced the finding that web users are the most at-risk segment. The model identified device context as an early behavioral signal worth acting on.

**Key Recommendations:**
Based on the full analysis the following actions are recommended for Duolingo's product and growth teams:
 * The single highest impact intervention is protecting the first 7 days. Every retention strategy should be concentrated in the first week of a new user's journey. Streak freeze offers, daily encouragement messages and simplified early lessons all have a role to play here.
 * Bronze and Silver league users need dedicated retention support. These two tiers represent 71 percent of all churned users. Targeted campaigns for this segment including motivational notifications and reduced competitive pressure in the early leagues could meaningfully move the retention needle.
 * Web users should be treated as a distinct acquisition and onboarding challenge. Their higher churn rate suggests they need a stronger nudge toward downloading the mobile app where notification-based re-engagement is more effective.
 * For users studying high-difficulty languages like Japanese and German, introducing an explicit difficulty acknowledgment and a more gradual progression curve in the early units could reduce early dropout caused by discouragement.
   
**Project Structure:**
duolingo-churn-analysis/
│
├── duolingo_dataset.xlsx       # Full simulated dataset across 4 tables

├── churn_analysis.ipynb        # Full analysis notebook

└── README.md                   # This file

**About Me:**

I am a junior data analyst with a growing background in data science and machine learning. This project reflects my approach to analytical work: start with a real business question, let the data tell the story and translate findings into recommendations that non-technical stakeholders can act on.
I am currently building my portfolio across EdTech, fintech and consumer app domains. Feel free to connect or reach out if you would like to discuss the work.

**NOTE:**

This project uses a synthetically generated dataset modeled after real product analytics structures. It is intended for portfolio and educational purposes.
 

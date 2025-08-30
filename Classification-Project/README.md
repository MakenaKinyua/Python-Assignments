### CLASSIFICATION PROJECT: AI ASSISTANCE USAGE

---

This is a classification project that aims to predict whether or not a student uses AI again for different tasks and also the final outcome after the student has interacted with AI.

The data set is from [Kagggle](https://www.kaggle.com/datasets/ayeshasal89/ai-assistant-usage-in-student-life-synthetic).

The data set has 1000 rowa and 11 columns:

a. SessionID - unique identifier for each session.

b. StudentLevel - level of education that the student is pursuing ie: highschool, undergraduate, graduate 

c. Discipline - area of study ie: Computer Science, Psychology, Business etc

d. SessionDate - date that session occurred.

e. SessionLengthMin - length of the session in minutes 

f. TotalPrompts - total prompts made by the unique sessionID.

g. TaskType - type of task that they needed assistance in ie: coding, studying, writing

h. AI_AssistanceLevel - on a scale of 1-5, the level of assistance that the stident needed

i. FinalOutcome - outcome from using AI: did the student complete the assignment, give up,get new ideas or end up confused?

j. UsedAgain - whether or not the student used AI again.

k. SatisfactionRating - level of satisfaction the student got from engaging with AI on the tasks.

---

### SECTION 1 : BASIC EDA

Import the necessary libraries and load the data. Check the shape and information of the data to have a better understanding of it. Check for missing values and unique values, value counts in different columns of the data 

- The are no missing values
- All the columns have the correct data type
- Unique values in StudentLevel: 3,Discipline: 7,TaskType: 6, UsedAdain: 2, FinalOutcome: 4
- The most common tasktype is writing while the least is research.
- Graduate students have the highest mean SessionLength as compared to highschool and undergraduate students.

---

### SECTION 2: VISUALIZATION

#### a. SessionLengthMin

A Histplot is used to show SessionLengthMin Distribution. The values range between 0-110. The data is skewed to the right and the mean value lies between 19-20 minutes. Most of the data lies between 0-40 minutes with fewer data points streching between 60-100 minutes.

#### b. SessionCount for each StudentLevel

There are 3 student levels. The barchart shows the number of sessions by each student level. Undergraduate students have the highest count with about 6000 students using AI. Highschool students and graduate students have an almost equal count but highschool students appear slightly higher.

#### c. TaskType

For TaskType, to show the counts of each task type, a countplot comes in handy. Writing is the most common task while research is the least. To visualise whether or not the students opted to use AI again for the tasks, set the hue to UsedAgain. Writing has the highest UsedAgain rate while research has the least.

#### d. SessionLength for each StudentLevel

Using a boxplot, the min, max, mean and outlier values are visualised. The x axis shoes the StudentLevel and the y axis shows the sessionLength. The three levels are quite similar. The mean lies between 19-20 mins. Outliers are present from about 50mins. Undergraduate Studentlevel has the most outliers and the maximum value in the data set as well. 

#### e. FinalOutcome 

There are 5 possible outcomes and the piechart shows the proportions of each of the outcomes. Most of the students were able to complete their assignments while the least gave up. 

#### f. SessionLength and TotalPrompts

The relationship between the two is visualised using a scatterplot. There is a positive relationship between the two ie: as the SessionLength increases, the TotalPrompts increase. The values are clustured together upto about 60mins where they start to spread out. 

#### g. Correlation Heatmap

The numeric columns are: SessionlengthMin, SatisfactionRating, AI_assistancelevel, TotalPrompts. 

Intepretation:

- TotalPrompts and SessionLengthMin have the highest positive correlation (0.9) which could mean that they relay the same information.
- AI_assistancelevel and SatisfactionRating have a high positive correlation (0.78) but its not too high
- AI_assistancelevel and SatisfactionRating have negative correlation with both SessionLengthMin and TotalPrompts

---

### SECTION 3: GROUPBY & AGGREGATION

#### a. TaskType

Finding the average SessionLength for each task. Most of the values lie between 19-21. Brainstorming has the longest average SessionLength at 21.964223mins while Coding has the lowest at 19.467659.

#### b. Discipline

Which discipline has the most sessions? Most of theseeions are by Biology students with 1458 users and Business has the least with 1410 users.

#### c. StudentLevel

The undergraduate students have the higest average assistance level followed by Highschool students then graduate students. Considering that graduate students needed a lower assistance level, what was their most common final outcome? Most of them completed their assignments while the least of them gave up.

#### d. FinalOutcome 

The median SessionLength for each FinalOutcome. Median values lie between 16-17. Those who gave up have the highest median value while those who ended up confused have the lowest median value.

---

### SECTION 4: FEATURE ENGINEERING AND ENCODING

Preparing the data set for Machine Learning and extracting data from existing columns to create new columns.

- The SessionDate column is used to extract the year, month and date columns.

- The three student levels are ordinal data points. Label encoding is used to properly capture the StudentLevels in their correct order: Highschool 0, Undergraduate 1, Graduate 2.

- There are other categorical columns which need to be encoded eg Discipline, Tasktype. The data points are not ordival. OneHotEncoder is used to encode these columns in preparation for the ML models.

- The column PromptsPerMinute = (TotalPrompts / SessionLengthMin) is created.

- SessionLength is put into categories of short, medium, long depending on the length.

### SECTION 5: CLASSIFICATION MODELS

There are two columns of interest for classification: FinalOutcome and UsedAgain. FinalOutcome is encoded using LabelEncoder and UsedAgain is encoded using OneHotEncoder and one of the columns is dropped.

#### FinalOutcome

Fit Decision Tree, Random Forest and NaiveBeyes and print out the classification report and confusion matrix.

Findings:

- Classification report

The three models struggle with correct predictions; the accuracy score ranges between 0.4 and 0.5 which implies that the models are not learning and predicting but rather guessing.

The models do well in predicting class 0 than the other classes. The models predict more data points as class 0 when they are in fact not class 0.

The f1_score for the models is also low which means that with the current parameters, the models are not doing well.

#### UsedAgain

Fit LogisticRegression, KNN, GradientBoostClassifier and evaluate using classification report and confusion matrix.

Findings:

- Logistic Regression

The model struggles with predicting class 0 positives with a precision of 0.33 which is low. The recall is also low which shows that the model only correctly predicts about half the number of true positives.

The accuracy of the model is low thus it is not efficient.

- GradientBoostClassifier(Performs the best)

The model struggles with class 0. The precision and recall values are higher as compared to logistic regression but still not good enough. 

The model accuracy is high at 0.74. 

- KNN 

It struggles with predicting class 0 according to the recall and precision values. It is good at capturing class 1 but at the cost of capturing false positives.

The accuracy is 0.67

---

### SECTION 6: MODEL EVALUATION AND HYPERPARAMETER TUNING

- Cross-Validation on logistic regression yields a mean accuracy of 0.53

- Gridsearch to tune the decision tree classifier yields a best score of 0.44

- Tuning RadndomForestClassifier using GridSearchCV yields a best score of 0.739

#### UsedAgain Summary

| Model | Accuracy | Weightedavg_f1 |
| ----------- | ----------- | ----------- |
| LogisticRegression | 0.53 | 0.55 |
| GradientBoostClassifier | 0.74 | 0.73 | 
| KNN | 0.67 | 0.63 |
| DecisionTree | 0.70 | 0.70 |
| RandomForestClassifier | 0.73 | 0.70 |
| GaussianNB | 0.67 | 0.58 |

The GradientBoostClassifier is the better model.

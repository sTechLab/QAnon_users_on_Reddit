# QReddit Analysis

This repository contains the code behind the analysis in the "Characterizing Reddit Participation of Users who Engage in the QAnon Conspiracy Theories" paper (CSCW '22).

## File locations
* Input data can be found in the inputFiles folder. 
* Output files can be found in the outputFiles folder. 

## Repository set up
```
pip install -r requirements.txt
```

## Input data 
### inputFiles/Hashed_Q_Submissions_Raw_Combined.csv

#### Key Stats
**2,099,875** unique submissions\
&emsp; from **13,182** unique Q-users\
&emsp; **50,002** unique subreddits\
&emsp; from **October 28. 2016 to January 23rd, 2021**\
&emsp; using the **Pushshift API** (psaw)

#### Column Information
|Column              | Data Type       | Definition       |
|:-------------|:---------------|:---------------|
| subreddit    | object         | subreddit name where submission is located|
| id           | object         | submission id|
| score        | int64          | number of upvotes for submission|
| numReplies   | int64          | count of comments|
| author       | object         | submission author's Reddit username|
| title        | object         | submission title|
| text         | object         | submission selftext, if no text then link post|
| is_self      | bool           | indication of text-only submission|
| domain       | object         | url link domain, if no url then submission permalink domain|
| url          | object         | full url link for submission, if no url then submission permalink|
| permalink    | object         | submission permalink|
| date_created | datetime64[ns] | submission creation date-time group (UTC)|
| type         | object         | submission or comment|

#### Sample
|    | subreddit      | id     |   score |   numReplies | author                                   | title                                                                                                                                                                                                               |   text | is_self   | domain    | url                                 | permalink                                                                            |   upvote_ratio | date_created        |
|---:|:---------------|:-------|--------:|-------------:|:-----------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------:|:----------|:----------|:------------------------------------|:-------------------------------------------------------------------------------------|---------------:|:--------------------|
|  0 | greatawakening | 8xuv4i |       1 |           14 | 879f283b831c13474e219e88663d95b0763cca9b | I’ve been writing “Trump Lives Here” on my $20’s after seeing the stamp idea advertised, but seeing this posted again gave me a better idea...going to start writing #QANON or #GreatAwakening on all my bills now! |    nan | False     | i.redd.it | https://i.redd.it/h3mbbxvxq7911.jpg | /r/greatawakening/comments/8xuv4i/ive_been_writing_trump_lives_here_on_my_20s_after/ |             -1 | 2018-07-11 00:27:24 |
|  1 | greatawakening | 8ydw3e |       1 |           13 | 879f283b831c13474e219e88663d95b0763cca9b | Trying to take him seriously but...                                                                                                                                                                                 |    nan | False     | i.redd.it | https://i.redd.it/62gaw0th4l911.jpg | /r/greatawakening/comments/8ydw3e/trying_to_take_him_seriously_but/                  |             -1 | 2018-07-12 21:26:32 |

### inputFiles/Hashed_Q_Comments_Raw_Combined.csv

#### Key Stats
**10,831,922** unique comments\
&emsp; from **11,210** unique Q-users\
&emsp; **36,947** unique subreddits\
&emsp; from **October 28. 2016 to January 23rd, 2021**\
&emsp; using the **Pushshift API** (psaw)

#### Column Information
Column              | Data Type       | Definition       |
|:-------------|:---------------|:---------------|
| id           | object         | comment id|
| link_id      | object         | submission id the comment is in response to|
| parent_id    | object         | parent comment id|
| author       | object         | comment author's Reddit username|
| subreddit    | object         | subreddit name where comment is located|
| body         | object         | body of the comment|
| date_created | datetime64[ns] | comment creation date-time group (UTC)|

#### Sample
|    | id      | link_id   | parent_id   | author                                   | subreddit      | body                                                                                                                                                   | date_created        |
|---:|:--------|:----------|:------------|:-----------------------------------------|:---------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------|
|  0 | e0mztbn | t3_8qy7gp | t3_8qy7gp   | 182c774799aac38a84f5117fc59cde99b0df19af | greatawakening | My account is new because i lost my password to my last account                                                                                        | 2018-06-14 02:17:37 |
|  1 | e0n0e9q | t3_8qy9wy | t3_8qy9wy   | 182c774799aac38a84f5117fc59cde99b0df19af | greatawakening | new account only because i lost the password to my old account. you can see my post history at JaM0k3 if you doubt my authenticity and genuine concern | 2018-06-14 02:28:21 |

### inputFiles/Hashed_allAuthorStatus.csv

#### Key Stats
**13,182** users\
&emsp; QAnon-enthusiastic = **3,506** users\
&emsp; QAnon-interested = **9,676** users

User status:\
&emsp; Active = **11,014** users\
&emsp; DNE = **1,624** users\
&emsp; Is_suspended = **544** users

#### Column Information
|Column              | Data Type       | Definition       |
|:--------|:-------|:-------|
| QAuthor | object | Q-users (authors of submissions in the 19 identified subreddits from the 2018 QAnon ban |
| isUQ    | int64  | 0 = QAnon-interested, 1 = QAnon-enthusiastic |
| status  | object | Active, DNE, Is_suspended = user account status derived from Reddit API Metadata (as of June, 2021) |

#### Sample
|    | QAuthor   | isUQ         | status |
|---:|:------------|:--------------|:-----------------------|
|  0 | aa65b7dd5d5fa660d058e094669f884bf7d52299 | 0 | Active |
|  1 | 2b1505f289338751829dfa129c0b52d145c9eceb | 1 | Active | 

### inputFiles/Hashed_subredditStats.csv

#### Key Stats
**12,987** unique subreddits

#### Column Information
|Column              | Data Type       | Definition       |
|:----------------------------------|:--------|:---------------|
| subreddit                         | object  | subreddit name that contains 2+ QAnon-enthusiastic (excludes user profile wikis)|
| numSubscribers                    | float64 | count of subreddit subscribers (as of June, 2021 or earlier loss of unrestricted subreddit access)|
| status                            | object  | subreddit access: unrestricted, quarantined, or private, and banned (as of June, 2021)|
| allModNames                       | object  | moderator Reddit usernames (as of June, 2021)|
| allMods                           | int64   | count of moderators (as of June, 2021)|
| qModNames                         | object  | Q-user moderator Reddit usernames (as of June, 2021)|
| qMods                             | float64 | count of Q-user moderators (as of June, 2021)|
| top_qModNames                     | object  | QAnon-enthusiastic moderator Reddit usernames (as of June, 2021)|
| top_qMods                         | float64 | count of QAnon-enthusiastic moderators (as of June, 2021)|
| firstPostSubmission               | object  | date_created for earliest submission|
| lastPostSubmission                | object  | date_created for latest submission|
| firstPostComment                  | object  | date_created for earliest comment|
| lastPostComment                   | object  | date_created for latest comment|
| qModsRatio                        | float64 | qMods / allMods (as of June, 2021)|
| top_qModsRatio                    | float64 | top_qMods / allMods (as of June, 2021)|
| activePreBanOnly                  | int64   | 1 = subreddit's last submission or comment is 2018-09-12 or earlier, 0 = active post-2018 QAnon ban|
| activePreQ                        | int64   | 1 = subreddit's first submission or comment is before 2017-10-28, 0 = not active pre- first Q drop|
| activePostBan                     | int64   | 1 = subreddit's last submission or comment is after 2018-09-12, 0 = acrtive pre-2018 QAnon ban|
| qAuth                             | int64   | count of Q-user submission authors|
| top_qAuth                         | int64   | count of QAnon-enthusiastic submission authors|
| qSubmissions                      | int64   | count of Q-user submissions|
| top_qSubmissions                  | int64   | count of QAnon-enthusiastic submissions|
| nonTop_qSubmissions               | int64   | count of QAnon-interested submissions|
| qComments                         | float64 | count of Q-user comments|
| top_qComments                     | float64 | count of QAnon-enthusiastic comments|
| nonTop_qComments                  | float64 | count of QAnon-interested comments|
| top_qPercent                      | float64 | (top_qAuth / 3506) * 100)|
| qPercent                          | float64 | (qAuth / 13182 * 100)|
| Monthly Average Total Authors     | float64 | average number of total subreddit submission authors per month (between 2018-03-12 and 2019-03-12)|
| Monthly Average Total Submissions | float64 | average number of total subreddit submissions per month (between 2018-03-12 and 2019-03-12)|
| Monthly Average UQ Authors        | float64 | average number of QAnon-enthusiastic subreddit submission authors per month (between 2018-03-12 and 2019-03-12)|
| Monthly Average UQ Submissions    | float64 | average number of QAnon-enthusiastic subreddit submissions per month (between 2018-03-12 and 2019-03-12)|
| Monthly Average QAnon Authors     | float64 | average number of Q-user subreddit submission authors per month (between 2018-03-12 and 2019-03-12)|
| Monthly Average QAnon Submissions | float64 | average number of Q-user subreddit submissions per month (between 2018-03-12 and 2019-03-12)|
| % UQ Submissions                  | float64 | (Monthly Average UQ Submissions / Monthly Average Total Submissions) * 100|
| % UQ Authors                      | float64 | (Monthly Average UQ Authors / Monthly Average Total Authors) * 100|
| % QAnon Submissions               | float64 | (Monthly Average QAnon Submissions / Monthly Average Total Submissions) * 100|
| % QAnon Authors                   | float64 | (Monthly Average QAnon Authors / Monthly Average Total Authors) * 100|

#### Sample
|    | subreddit   |   numSubscribers | status   | allModNames                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |   allMods | qModNames   |   qMods | top_qModNames   |   top_qMods | firstPostSubmission   | lastPostSubmission   | firstPostComment    | lastPostComment     |   qModsRatio |   top_qModsRatio |   activePreBanOnly |   activePreQ |   activePostBan |   qAuth |   top_qAuth |   qSubmissions |   top_qSubmissions |   nonTop_qSubmissions |   qComments |   top_qComments |   nonTop_qComments |   top_qPercent |   qPercent |   Monthly Average Total Authors |   Monthly Average Total Submissions |   Monthly Average UQ Authors |   Monthly Average UQ Submissions |   Monthly Average QAnon Authors |   Monthly Average QAnon Submissions |   % UQ Submissions |   % UQ Authors |   % QAnon Submissions |   % QAnon Authors |
|---:|:------------|-----------------:|:---------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------:|:------------|--------:|:----------------|------------:|:----------------------|:---------------------|:--------------------|:--------------------|-------------:|-----------------:|-------------------:|-------------:|----------------:|--------:|------------:|---------------:|-------------------:|----------------------:|------------:|----------------:|-------------------:|---------------:|-----------:|--------------------------------:|------------------------------------:|-----------------------------:|---------------------------------:|--------------------------------:|------------------------------------:|-------------------:|---------------:|----------------------:|------------------:|
|  0 | Watches     |      1.52524e+06 | public   | ['f1c355408b78fd88ebc13aade4c9a7924005c2ab','e8ba168935c0aaf4c836f754134ab7ce2af1cb78','3d24107f83da2c1b164f9ef6056534c411a014c5','4761651055cbc095f958408c130db085207d94f0','2f3685e3246480acf62db4fa47134136abb72fa8','e91b7152b521a4c2ee38d40b4269db6ed3a4a864','733a3079682a418d3ac0a0082a1c73c82426ab64','f07a25e26c9be732c6a8e0baeb259de0012db9c0','5324eebc18306707d97d9a20cf7ab0d333a852f6','1a2ab94f2a57af66faaf6c6cc1c2aedfbf7adeaf','1bfb0b9d4f7209ceeb0fd8a41032dc03dc270d55','beb0c1ef7acd0d2c116eb53afe5e9c6466268087','b6b4b8c71fbbf8e56cbffdaa2909cbe4d23c6e25']                                                                                                                                                                                                                                                                                                              |        13 | []          |       0 | []              |           0 | 2016-12-07 03:21:16   | 2020-11-27 19:24:49  | 2016-11-26 13:24:54 | 2021-01-22 18:22:43 |            0 |                0 |                  0 |            1 |               1 |      58 |          10 |            219 |                 99 |                   120 |        1681 |             244 |               1437 |           0.29 |       0.44 |                         2881.38 |                             5911.85 |                      1.2     |                             2.4  |                         3.75    |                             6       |          0.0405965 |      0.0416466 |              0.101491 |          0.130146 |
|  1 | MMA         |      1.51845e+06 | public   | ['69e403df92bb49af60d5046c0be60f1a46bfd53d','db2bc6d4901d4b045b6106f6d8fa05d39d7299bc','e6a27fb7e49c024a93e255f6aebf7d050286d555','662214643ba4236170bdd0930c3960aec957eccc','346dc08bbb0aba36e747eed2c60f71ed2421b801','42af3024c512b1570e9b18d14e489ea1aabc5289','ad60ae7a9dd74661af165743cc541e9a643d5653','9e165e1ea288ea76cc543606b60158297f53189c','8b1895f2ab3dd2937e57df3dfbd0a0f2722d22a6','a811e14d7aca9db33a0741e9b3ac224674a3678d','814f2b241069c14efd6505e3e6608fd6bd0e5837','20753b7f572e0d6a003037e751fc755fc2a69e7b','9ad716f0c5f5346b6bd5f9d25028f581a202f5d6','86294814dcd22c5c6ec366f986261825204f7d3b','e735ba8c261225c06427c4141ae52e3ccd63d069','64c14242cc4aba563a93ad72c2d27caa94c3f5d4','5c547518bd4db19ca4d04acbf7801e2d803bf3eb','19ec300a4b3eb3a65a7091dfec4618fe02a53caa','67d7a0de2ae09727ef61f0720e40125e555dd56a','8955818d50a9bcc9bca0877413fb030ba9452e3a'] |        20 | []          |       0 | []              |           0 | 2016-11-01 05:51:55   | 2021-01-03 00:43:44  | 2016-10-28 00:39:15 | 2021-01-23 08:16:57 |            0 |                0 |                  0 |            1 |               1 |      97 |          29 |            371 |                 99 |                   272 |       18910 |            2808 |              16102 |           0.83 |       0.74 |                         1840.46 |                             4953.46 |                      2.91667 |                             4.75 |                         6.38462 |                             9.92308 |          0.0958925 |      0.158475  |              0.200326 |          0.346903 |

### inputFiles/Subreddit_Cluster_Map.csv

#### Key Stats
* 915 unique sampled subreddits

#### Column Information
|Column              | Data Type       | Definition       |
|:------------------------|:-------|:-------|
| subreddit               | object | sampled subreddit name|
| Codes                   | object | qualitative codes for listed subreddit (n of 24)|
| Final Topic Clusters    | object | label common theme for listed subreddit (n of 9)|
| Final Relation Clusters | object | label of relationship to QAnon narrative (n of 6)|

#### Sample
|    | subreddit   | Codes         | Final Topic Clusters   | Final Relation Clusters   |
|---:|:------------|:--------------|:-----------------------|:--------------------------|
|  0 | 2070vids    | ['unrelated'] | ['unrelated']          | ['unrelated']             |
|  1 | 3Dprinting  | ['unrelated'] | ['unrelated']          | ['unrelated']             |

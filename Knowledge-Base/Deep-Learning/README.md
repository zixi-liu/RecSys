## Embedding + MLP

**Deep Crossing** [[KDD2016]](https://www.kdd.org/kdd2016/papers/files/adf0975-shanA.pdf)

Deep Crossing model is a deep neural network that automatically combines features to produce models. The input of Deep Crossing is a set of individual features that can be either dense or sparse. The important crossing features are discovered implicitly by the networks, which are comprised of an embedding and stacking layer, as well as a cascade of Residual Units.

**Deep Cross Model Architecture**

<img src="https://github.com/zixi-liu/Sparrow-Recsys/blob/main/Knowledge-Base/Img/deep-crossing.png" alt="deep-crossing" width = "600px" />

### Transformation of raw features

- **Combinatorial features (Cross features):** construct functions based on a combination of multiple features, and use their output as the input to a
learner. More specifically, it takes in individual features such as text, categorical, ID, and numerical features, and searches for the optimal combinations automatically based on the specific task.

### Web-scale Application

**Sponsored search**

The goal of the platform is to show the user the advertisement that best matches the user’s intent, which was expressed mainly through a specific query.

Key Concepts:

- Query: A text string a user types into the search box
- Keyword: A text string related to a product, specified by an advertiser to match a user query
- Title: The title of a sponsored advertisement (referred to as “an ad” hereafter), specified by an advertiser to capture a user’s attention
- Landing page: A product’s web site a user reaches when the corresponding ad is clicked by a user
- Match type: An option given to the advertiser on how closely the keyword should be matched by a user query, usually one of four kinds: exact, phrase, broad and contextual
- Campaign: A set of ads that share the same settings such as budget and location targeting, often used to organize products into categories 
- Impression: An instance of an ad being displayed to a user. An impression is usually logged with other information
available at run-time
- Click: An indication of whether an impression was clicked by a user. A click is usually logged with other information available at the run-time
- Click through rate: Total number of clicks over total number of impressions
- Click Prediction: A critical model of the platform that predicts the likelihood a user clicks on a given ad for a given query

**Individual Features**

Each individual feature Xi is represented as a vector.
- For text features such as a query, one option is to convert the string into a tri-letter gram with 49, 292 dimensions;
- Categorical input such as MatchType is represented by a one-hot vector, where exact match is [1, 0, 0, 0], phrase match is [0, 1, 0, 0], and so on;
- There are usually millions of campaigns in a sponsored search system. Simply converting campaign ids into a onehot vector would significantly increase the size of the model. One solution is to use a pair of companion features as exemplified in the table, where CampaignID is a one-hot representation consisting only of the top 10, 000 campaigns with the highest number of clicks. The 10, 000th slot (index starts from 0) is saved for all the remaining campaigns. Other campaigns are covered by CampaignIDCount, which is a numerical feature that stores per campaign statistics such as click through rate.

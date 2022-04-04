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

**Combinatorial Features**

Given individual features Xi ∈ Rni and Xj ∈ Rnj, a combinatorial feature Xi,j is defined in Rni×Rnj.

Combinatorial features also have sparse and dense representations. 
- An example of sparse representation is a CampaignId×MatchType feature, which is a one-hot vector of 10, 001 × 4 = 40, 004 dimensions. 
- An example of a dense representation counts how many ad clicks for a specific CampaignId and MatchType combination. The dimension for the dense representation is the same as its sparse counterpart.

**Deep Crossing avoids using combinatorial features. It works with both sparse and dense individual features.**

### Model Architecture

The input is a set of individual features. The model has four types of layers including the Embedding, the Stacking, the Residual Unit, and the Scoring Layer. The objective function is log loss for our web-scale application but can be easily customized to soft-max or other functions.

**Embedding and Stacking Layers**

Embedding is applied per individual feature to transform the input features. The embedding layer consists of a single layer of a neural network, with the general form: XOj = max(0, Wj x Xij + bj ), where j indexes the individual feature, Xij ∈ Rnj is the input feature, Wj is an mj × nj matrix, b ∈ Rnj, and XOjis the embedded feature. When mj < nj , embedding is used to reduce the dimensionality of the input feature. The per element max operator is usually referred to as a rectified linear unit (ReLU) in the context of neural networks. 

The output features are then stacked (concatenated) into one vector as the input to the next layer: XO = [XO0 , XO1 , · · · , XOK], where K is the number of input features.

Note that both {Wj} and {bj} are the parameters of the network, and will be optimized together with the other parameters in the network. This is an important property of embedding in Deep Crossing. Unlike the embedding only approaches such as word2vec, it is an integral part of the overall optimization process.

**Residual Layers**

The Residual Unit is the basic building block of the [Residual Net](https://arxiv.org/pdf/1512.03385.pdf) that claimed the world record in the ImageNet contest.

Building Block of Residual Learning 

<img src="https://github.com/zixi-liu/Sparrow-Recsys/blob/main/Knowledge-Base/Img/residual-learning.png" alt="residual-learning" width = "400px" />

**A Deep Crossing Model with five layers of Residual Units, described in the CNTK modeling language**

<img src="https://github.com/zixi-liu/Sparrow-Recsys/blob/main/Knowledge-Base/Img/deep-crossing-model.png" alt="deep-crossing-model" width = "400px" />

## Wide & Deep Learning

Wide & Deep Learning[[ArXiv2016]](https://arxiv.org/pdf/1606.07792.pdf)

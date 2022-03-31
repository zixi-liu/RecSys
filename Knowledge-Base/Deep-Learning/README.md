## Embedding + MLP

**Deep Crossing** [[KDD2016]](https://www.kdd.org/kdd2016/papers/files/adf0975-shanA.pdf)

Deep Crossing model is a deep neural network that automatically combines features to produce models. The input of Deep Crossing is a set of individual features that can be either dense or sparse. The important crossing features are discovered implicitly by the networks, which are comprised of an embedding and stacking layer, as well as a cascade of Residual Units.

<img src="https://github.com/zixi-liu/Sparrow-Recsys/blob/main/Knowledge-Base/Img/deep-crossing.png" alt="deep-crossing" width = "600px" />

### Transformation of raw features

- **Combinatorial features (Cross features):** construct functions based on a combination of multiple features, and use their output as the input to a
learner.

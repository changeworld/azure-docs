---
title: "One-vs-All Multiclass"
titleSuffix: Azure Machine Learning
description: Learn how to use the One-vs-All Multiclass component in Azure Machine Learning designer to create an ensemble of binary classification models.
services: machine-learning
ms.service: azure-machine-learning
ms.subservice: core
ms.topic: reference

author: likebupt
ms.author: keli19
ms.date: 11/13/2020
---
# One-vs-All Multiclass

This article describes how to use the One-vs-All Multiclass component in Azure Machine Learning designer. The goal is to create a classification model that can predict multiple classes, by using the *one-versus-all* approach.

This component is useful for creating models that predict three or more possible outcomes, when the outcome depends on continuous or categorical predictor variables. This method also lets you use binary classification methods for issues that require multiple output classes.

### More about one-versus-all models

Some classification algorithms permit the use of more than two classes by design. Others restrict the possible outcomes to one of two values (a binary, or two-class model). But even binary classification algorithms can be adapted for multi-class classification tasks through a variety of strategies. 

This component implements the one-versus-all method, in which a binary model is created for each of the multiple output classes. The component assesses each of these binary models for the individual classes against its complement (all other classes in the model) as though it's a binary classification issue. In addition to its computational efficiency (only `n_classes` classifiers are needed), one advantage of this approach is its interpretability. Since each class is represented by one and one classifier only, it is possible to gain knowledge about the class by inspecting its corresponding classifier. This is the most commonly used strategy for multiclass classification and is a fair default choice. The component then performs prediction by running these binary classifiers and choosing the prediction with the highest confidence score. 

In essence, the component creates an ensemble of individual models and then merges the results, to create a single model that predicts all classes. Any binary classifier can be used as the basis for a one-versus-all model.  

For example, let’s say you configure a [Two-Class Support Vector Machine](two-class-support-vector-machine.md) model and provide that as input to the One-vs-All Multiclass component. The component would create two-class support vector machine models for all members of the output class. It would then apply the one-versus-all method to combine the results for all classes.  

The component uses OneVsRestClassifier of sklearn, and you can learn more details [here](https://scikit-learn.org/stable/modules/generated/sklearn.multiclass.OneVsRestClassifier.html).

## How to configure the One-vs-All Multiclass classifier  

This component creates an ensemble of binary classification models to analyze multiple classes. To use this component, you need to configure and train a *binary classification* model first. 

You connect the binary model to the One-vs-All Multiclass component. You then train the ensemble of models by using [Train Model](train-model.md) with a labeled training dataset.

When you combine the models, One-vs-All Multiclass creates multiple binary classification models, optimizes the algorithm for each class, and then merges the models. The component does these tasks even though the training dataset might have multiple class values.

1. Add the One-vs-All Multiclass component to your pipeline in the designer. You can find this component under **Machine Learning - Initialize**, in the **Classification** category.

   The One-vs-All Multiclass classifier has no configurable parameters of its own. Any customizations must be done in the binary classification model that's provided as input.

2. Add a binary classification model to the pipeline, and configure that model. For example, you might use [Two-Class Support Vector Machine](two-class-support-vector-machine.md) or [Two-Class Boosted Decision Tree](two-class-boosted-decision-tree.md).

3. Add the [Train Model](train-model.md) component to your pipeline. Connect the untrained classifier that is the output of One-vs-All Multiclass.

4. On the other input of [Train Model](train-model.md), connect a labeled training dataset that has multiple class values.

5. Submit the pipeline.

## Results

After training is complete, you can use the model to make multiclass predictions.

Alternatively, you can pass the untrained classifier to [Cross-Validate Model](cross-validate-model.md) for cross-validation against a labeled validation dataset.


## Next steps

See the [set of components available](component-reference.md) to Azure Machine Learning. 

### Notes on Decision Tree Classifiers

General Resources:

* [Let’s Write a Decision Tree Classifier from Scratch - Machine Learning Recipes #8
](https://youtu.be/LDRbO9a6XPU)
* [Gini Impurity](https://en.wikipedia.org/wiki/Decision_tree_learning#Gini_impurity) 
* [Jupyter Notebook From Video](https://github.com/random-forests/tutorials/blob/master/decision_tree.ipynb)
* [`sklearn.tree.DecisionTreeClassifier`](https://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeClassifier.html)
* [Decision Tree Classification in Python](https://www.datacamp.com/community/tutorials/decision-tree-classification-python)
* [StatQuest: Decision Trees](https://youtu.be/7VeUPuFGJHk)


Overall, a decision tree asks a series of `True/False` questions to classify a given dataset.

The algorithm of a decision tree classifier is called the Classification and Regression Tree (CART) algorithm.

For the CART algorithm, the first (or root) node, receives the entire dataset and a `True/False` question is asked about the data. In response to the question, the algorithm splits the dataset into two subsets.

The overall goal of the CART is to purify the classes from the entire dataset so that each subset contains the highest number of items of a single class/label. That is, for each side of a note, all of the items in that subset contains all of the same data elements. 

A measure of how impure a given subset is, is determined by the Gini Impurity (`Gini`). A `Gini == 0` means that the subset is 100% pure.  On the other hand, a `Gini > 0` means that the subset is not 100% pure. 

The ability of a given tree to properly classify all for the points in a given dataset is a function of two components: information gain and uncertainty. 

Information gain is used to determine which questions will reduce the uncertainty  (`Gini impurity`) the most. Information gain is used to select the question to ask at each decision point (node). As mentioned earlier, the Gini impurity is the measure of uncertainty, or ability of a single question to split the data into buckets that have a high purity of data points from a single class.

As a whole, the way that a decision tree works to determine the kinds of questions to as is as follows.

* Calculate the `Gini impurity` for the starting dataset.
* Ask a question, classify the points, and calculate the `Gini impurity` at each node. 
* Take the weighted average of all of the `Gini` values at each node. 
* Subtract the initial `Gini` vale from the weighted average `Gini` value. 
* This difference is determined to the question with the best information gain score, and this is interpreted to be the best question to ask in the decision tree classifier. 

#### [Notes from Hands-On Machine Learning with Scikit-Learn & TensorFlow (First Edition, pg. 167-179)](https://www.amazon.com/Hands-Machine-Learning-Scikit-Learn-TensorFlow-dp-1491962291/dp/1491962291)

* You can visualize the decision tree using the `graphviz` package.
* Since you are able to see how the classifier purifies the data at each step, a decision tree classifier is called a _whitebox model_ as opposed to a _blackbox model_. This is good from a perspective of interpretability. 
* One of the pros of a decision tree classifier is that the model only needs to look at the value of one feature at a time and so the complexity is $(O(log_{2}(m))$.
* Training a decision tree on the other hand requires to compare all of the features so training complexity is $(O(n x m log_{2}(m))$.
* In addition to the `Gini impurity`, one can also use entropy as a measure of impurity. Using the `Gini impurity` tends to have a model that will isolate the most frequent class but using `entropy` will tend to produce more balanced trees. It is best to try both and see what one leads to a better result for your use case.
* Decision trees are prone to overfitting since they make very few assumptions about the data. To overcome this, you'll need to consider regularization of the hyperparameters to avoid overfitting the data.
	* In Scikit-Learn, this is accomplished by the `max_depth` parameter. Reducing the `max_depth` will regularize the model and avoid the chances of overfitting. 
* Given how decision trees work, this type of model works best with orthogonal decision boundaries, where all of the splits are made perpendicular to an axis. 
* Finally, decision trees are very sensitive to small variations in the training data. That is, if you remove samples from the datasets, then it is possible to get very different outcomes. You can ensure reproductivity by setting a `random_state` in the model's parameters since the model is stochastic.

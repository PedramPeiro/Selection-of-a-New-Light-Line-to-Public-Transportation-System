# Selection of a New Light Line to Public Transportation System
This project is for Basics of Decision-Making course which was based on [this article](https://www.sciencedirect.com/science/article/abs/pii/S0950705119302254), named **Ordinal consensus measure with objective threshold for heterogeneous large-scale group decision making**. The task was to fully understand the article and provide the python code for solving the problem.

In this problem, we have 5 alternatives to choose. To chose the best ranking of alternatives, and by ranking I mean which alternative comes first, which come second and so on, we collect expert's preferences about the rankings which are on the basis of 5 criteria. Now our task is to grapple with the heterogeneous large-scale group decision making problem by the usage of these rankings provided by the experts. 

A sample of a ranking can be (2,4,5,3,1) which means: alternative 1 is ranked 2nd, alternative 2 is ranked 4th and so on.

The code is explained below in different sections.

# 1. Clustering
We want to cluster experts' opinions based on their preferences, so we can decide the ultimate ranking more conviniently. In order to cluster the experts, we use the well-known algorithm K-Means. Therefore, we divide this section to differnt parts of this algorithm:

## 1.1. Centroids Initializations
There are many ways to start the initialization centroids in this algorithm e.g. random initialization. But according to the article, we use the improved max-min initialization. To use this algorithm, we got to go through the following steps:

**Step 1.** Calculate the average distance of each point to all other points, and select the point that has the smallest average distance as the first center point.

**Step 2.** Calculate the distances of all other points to the first point, and select the point that is furthest to the first center point as the second center point.

**Step 3.** Calculate the distances of the remaining points to the first two points and select the point that has maximum nearest distance as the third center point, and so on, until K points are selected.

## 1.2. Assigning experts' preferences to each cluster
In the next step, we calculate the Euclidean distance between each ordering and each cluster and assign the label of the closest cluster to the ordering. The formula used for Euclidean distance is as below:
<img width="217" alt="Screenshot 2022-11-12 002208" src="https://user-images.githubusercontent.com/102898063/201428928-a16fe82e-9076-46a9-a325-0cbcbc5e9320.png">

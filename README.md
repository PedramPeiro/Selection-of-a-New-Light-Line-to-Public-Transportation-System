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

# 2. Calculation of preference vector
Next we calculate the preference vector for each preference, It's easily calculated and the only thing that should be done is to if the ordering is considered as an array, we have to subtract it from n (number of alternatives) and devide the result by the summation starting from 1 to n-1.

For example for r = (2,4,5,3,1): (5 - (2,4,5,3,1))/(1+2+3+4) = (3,1,0,2,4)/10 = (0.3 , 0.1 , 0 , 0.2 , 0.4) = w

In the next stage, the prefernce vector for each cluster should be calculated. The preference vector of subgroup $C_l$ is:

<img width="115" alt="Screenshot 2022-11-12 003256" src="https://user-images.githubusercontent.com/102898063/201430250-bf42f7be-16bb-4d3c-8465-ec4003472f3f.png">

where \# $C_l$ is the number of decision makers in the $l$th cluster. $w^(lt) _i$ $=$ $(w^{(lt)} _1 , w^{(lt)} _2 , ... , w^{(lt)}_n)^T$  is the preference vector of the $t$th expert in subgroup $C_l$.

After dividing DMs into several subgroups, the next thing is to assign weight to each cluster. Generally, the number of DMs in a group reflects its importance. Let $ζ_l$ be the weight of cluster Cl. If all DMs are given equal importance weights, the weight of a subgroup can be given as:

$ζ_l$ = \# $C_l/m$

where m is the number total number of DMs.

For example if we have 15 experts in subgroup 1 adn total experts of 20, $ζ_1$ = $15/20 = 0.75$

Now after calculating the preference vector for all of the vectors, we calculate the average of the preference vectors belonging to experts who are in a same subgroup. The result is the preference vector of the relevant subgroup. Assume that a subgroup has 2 experts in it with pref. vectors: $w^{(1)} = (0.3,0.2,0.1,0.4,0)$ and $w^{(2)} = (0.4,0.2,0,0.3,0.1)$. The pref. vector of the subgroup is:

$(\frac{0.3+0.4}{2} , \frac{0.2+0.2}{2} , \frac{0.1+0.0}{2} , \frac{0.4+0.3}{2} , \frac{0.0+0.1}{2}) = (0.35 , 0.2 , 0.05 , 0.35 , 0.05)$

And the  ordinal preference of this subgroup is obtained by sorting the pref. vector in a decsending order: $r^{(C_1)} = (1 , 3 , 4 , 2 , 5)$

To calculate the pref. vector of the global group we should multiply the pref. vectors of the subgroups to their corresponding weight and sum them up, i.e. (for 2 sugroups) $w^{(C_1)} = (0.3,0.2,0.1,0.4,0)$ and $w^{(C_2)} = (0.4,0.2,0,0.3,0.1)$, and $ζ_1 = 0.6$ and $ζ_2 = 0.4$. 

$w^{(C_G)} = 0.6*(0.3,0.2,0.1,0.4,0) + 0.4* (0.4,0.2,0,0.3,0.1) = (0.32 , 0.2 , 0.06 , 0.36 , 0.04)$

And the ordinal preference of the global group is $r^{(C_G)} = (2 , 3 , 4 , 1 , 5)$

# 3. Consensus checking process with ordinal consensus measures
In this section, the ordinal consensus is introduced to measure the consensus level of subgroups and the global group.

The subgroup ordinal consensus index (SOCI) for subgroup $C_l$ is defined as follows:

<img width="285" alt="Screenshot 2022-12-01 232032" src="https://user-images.githubusercontent.com/102898063/205146306-500942ce-c7b2-4551-a196-0b0c83066152.png">

and the global ordinal consensus index (GOCI) is defined as follows:

<img width="325" alt="Screenshot 2022-12-01 232154" src="https://user-images.githubusercontent.com/102898063/205146587-bd652a19-0464-4405-a9c2-af8a8758a24c.png">

For example if $r^{(C_1)} = (3, 1, 2, 4), r^{(C_2)} = (2, 3, 4, 1), r^{(C_3)} = (1, 3, 2, 4) and r^{(C_G)} = (3, 2, 1, 4)$ and and the weights of the 3 subgroups are equal, then $SOCI^{(C_1)} = 0.6838, SOCI^{(C_2)} = 0.0 , SOCI^{(C_3)} = 0.6127, GOCI = 0.4322$

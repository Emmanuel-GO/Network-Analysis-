# Network Analysis on Amazon Co-Purchasing Network
### Introduction
In today's interconnected digital landscape, networks play a fundamental role in understanding complex relationships across various domains, including social
interactions, product recommendations, and information flow. Network analysis provides powerful tools to uncover hidden patterns, detect influential nodes, and explore structural properties within large datasets.
This study leverages a dataset from the Stanford Large Network Dataset Collection (https://snap.stanford.edu/data/) to conduct a comprehensive network analysis. Our primary objective is to explore the dataset, formulate insightful research questions, and apply network science techniques to extract valuable insights.
We will investigate key aspects of the network using advanced methodologies such as degree centrality, community detection, shortest path analysis, and betweenness centrality.
### Objective
The objective of this study is to conduct an in-depth network analysis of the Amazon product co-purchasing network dataset from the Stanford Large Network Dataset
Collection. This dataset represents products as nodes and co-purchasing relationships as edges, providing valuable insights into product recommendations, influence, and category clustering.
Using advanced network science techniques, this analysis aims to:
📌 How does the removal of low-degree nodes (web pages with very few links) affect the overall connectivity and robustness of the web network?

📌	Analyze the Relationship Between Sales Rank and Connectivity – Investigate the correlation between a product’s degree centrality and its Sales Rank to understand if highly connected products perform better in sales.

📌	Which web pages in the dataset have no outgoing links (broken links) or no incoming links (orphan pages), and how do these impact the overall network structure?

📌	Detect Key Bridge Products – Compute betweenness centrality to identify the top 10 products that act as intermediaries, bridging different product categories.

📌	Which product groups have the best-selling products? - To determine which product groups, have the best-selling products, we can analyze the SalesRank distribution across different product groups. Since a lower SalesRank indicates better sales, we can compute the average SalesRank per Group and
identify

📌 Which groups tend to have the best-performing products.

📌 Do similar products tend to have close SalesRanks? - To analyze whether similar products tend to have close SalesRanks, we can compute the SalesRank difference between connected products in the network and visualize their correlation

Through data-driven analysis, network graphs, and visualizations, this study will reveal important patterns in Amazon’s co-purchasing behavior, offering insights into product
influence, recommendation dynamics, and consumer purchasing trends.

## Dataset Description
## **Data Information**

The network data was collected by crawling the Amazon website. It is based on Customers Who Bought This Item Also Bought feature of the Amazon website. If product i is frequently
 
co-purchased with product j, the graph contains an undirected edge from i to j. Each product category provided by Amazon defines each ground-truth community.
We regard each connected component in a product category as a separate ground-truth community. We remove the ground-truth communities which have less than 3 nodes

### **_Key Attributes/Columns Used in the Analysis_**
**From amazon_meta.csv**

✅	Id: Unique product identifier.

✅	ASIN: Amazon Standard Identification Number.

✅	Title: Product title.

✅	Group: Product category (e.g., Books, DVDs, etc.).

✅	SalesRank: Rank based on sales performance.

✅	Similar: List of similar products.

✅	Categories: Number of categories the product belongs to.

✅	Reviews: Contains total reviews, downloaded reviews, and average rating.

**From amazon_graph.csv**

✅	FromNodeId: The source product in a connection.

✅	ToNodeId: The target product, meaning the first product links to the second.

**Summary Statistics**

✅	Nodes (Unique Products): 548,552

✅	Edges (Connections Between Products): 925,660

✅	Types of Connections: Directed relationships between products, indicating recommendations or similar product links.


### __Tools Used in the Analysis__
The network analysis was conducted using the following tools and libraries:
👉🏽 Programming Language Environment

•	Python – Primary language for data analysis and visualization
 
•	Jupyter Notebook – Interactive coding environment for running and refining analysis

👉🏽 Data Processing s Manipulation

•	Pandas – For handling large datasets, filtering, merging, and aggregation

•	NumPy – For numerical computations and data transformations

👉🏽 Network Analysis s Graph Processing

•	NetworkX – To construct, analyze, and visualize the product co-purchasing network

•	Community Detection Algorithms (Louvain, Greedy Modularity) – To identify product clusters

👉🏽 Statistical Analysis s Correlation Computation

•	Scipy (Spearman s Pearson Correlations) – To measure relationships between connectivity and sales rank

•	Seaborn s Matplotlib – To visualize trends, correlations, and distributions

👉🏽 Visualization s Data Exploration

•	Matplotlib – For static visualizations of the network and correlation plots

•	Seaborn – For enhanced statistical plotting, including regression trends and histograms

•	NetworkX Graph Visualization – For rendering sampled subgraphs to show key patterns

👉🏽Optimization Techniques

•	Set Operations s Vectorized Pandas Functions – To optimize node identification (broken/orphan pages)

•	Graph Sampling Methods – To improve efficiency in large-scale network visualization






















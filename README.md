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


## **Research Questions**
__Formulated data-driven questions__
 
1.	How does the removal of low-degree nodes (web pages with very few links) affect the overall connectivity and robustness of the web network?
2.	Analyze the Relationship Between Sales Rank and Connectivity – Investigate the correlation between a product’s degree centrality and its Sales Rank to understand if highly connected products perform better in sales.
3.	Which web pages in the dataset have no outgoing links (broken links) or no incoming links (orphan pages), and how do these impact the overall network structure?
4.	Detect Key Bridge Products – Compute betweenness centrality to identify the top 10 products that act as intermediaries, bridging different product categories.
5.	Which product groups have the best-selling products? - To determine which product groups, have the best-selling products, we can analyze the SalesRank distribution across different product groups. Since a lower SalesRank indicates
better sales, we can compute the average SalesRank per Group and identify which groups tend to have the best-performing products.
6.	Do similar products tend to have close SalesRanks? - To analyze whether similar products tend to have close SalesRanks, we can compute the SalesRank difference between connected products in the network and visualize their correlation


## **Data Preprocessing**
__Data Cleaning__

1️⃣ Load the Datasets

__I first loaded both datasets:__

•	amazon_graph.csv → Contains product connections (edges in the network).

•	amazon_meta.csv → Contains product details like sales rank, category, and reviews.

2️⃣ Handling Missing Values

•	Amazon Graphs dataset: Checked if any missing values exist in FromNodeId or ToNodeId. If a product ID was missing, I removed that row since an incomplete edge is meaningless.
 
•	Amazon Meta dataset: Checked for missing values in SalesRank, Category, and AvgRating. If a product was missing its sales rank, I replaced it with 999999 (a high value to indicate "not ranked").

3️⃣ Removing Duplicates
•	Amazon Graphs dataset: Ensured each connection (edge) appeared only once.

•	Amazon Meta dataset: Ensured each product had only one unique entry.

4️⃣Converting Data Types
__Some columns were stored as text but needed to be numeric for analysis.__

•	SalesRank → Converted to an integer.

•	AvgRating → Converted to a float.

•	Price → Removed currency symbols and converted to float.

5️⃣ Filtering Out Irrelevant Data

•	Some products in Amazon Meta were not in Amazon Graphs (i.e., they had no connections).

•	I kept only products that appeared in the graph dataset to focus on relevant data.

6️⃣Handling Isolated Nodes (Orphan Products)

•	Some products had no outgoing (FromNodeId) or incoming (ToNodeId) links.

•	These were identified as "orphan pages" (products with no connections).

•	I kept them separately for research on broken links but excluded them from the main network analysis.


### Data Merging
I started with the Amazon Graphs dataset, which primarily contained product relationships—showing how different products were linked, either throughrecommendations or co-purchases. This dataset essentially formed a network structure, where each product acted as a node, and the connections between them were edges.However, while this data gave me insights into how products were connected, it lacked contextual information about these products. I needed additional details to answer deeper research questions, especially those related to sales performance, product categories, and customer reviews.
That’s where the Amazon Meta dataset came in. This dataset contained rich product metadata, including:

•	Sales Rank (to analyze if well-connected products perform better in sales)

•	Category (to identify bridge products linking different product groups)

•	Average Rating s Number of Reviews (to study the relationship between product popularity and network influence)

To make this analysis meaningful, I had to merge these datasets using a commonidentifier—the Product ID, which matched with the FromNodeId and ToNodeId in the Amazon Graphs dataset. However, instead of merging everything, I carefully selected only the columns that were relevant to my research questions.By combining network connectivity from Amazon Graphs with product attributes from Amazon Meta, I could now explore how connectivity impacts sales, which products act as key bridges, and Which product groups have the best-selling products—unlocking deeper insights that wouldn’t have been possible using just one dataset alone.

### Key Merging Strategy

•	Join Type → Inner Join on ProductID (from amazon_meta.csv) and FromNodeId/ToNodeId (from amazon_graph.csv).

•	Columns Used → We selectively merge with only relevant attributes to keep the dataset manageable and focused.

## **Method Applied in Research Ǫuestion 1** 

How does the removal of low-degree nodes (web pages with very few links) affect the overall connectivity and robustness of the web
network?

**Graph Construction**

•	**Method**: A directed graph (DiGraph) is constructed using the FromNodeId and ToNodeId columns from the dataset.

•	**Technique**:
1.The graph is built by adding edges between nodes using G.add_edges_from(edges).
2.This represents a directed network, where edges have direction (from one node to another).

## Identification of Low-Degree Nodes
•	**Method**: Nodes with a degree of 2 or less are identified as low-degree nodes.

•	**Technique**:

o	The degree of each node is calculated using G.degree(node).

o	A list of nodes with degree ≤ 2 is created using a list comprehension:

## Network Properties Calculation

The following network properties are computed before and after the removal of low- degree nodes:

### a.	Largest Connected Component (LCC)

•	**Method**: The largest weakly connected component is identified.

## Technique:

-	Weakly connected components are computed using nx.weakly_connected_components(G).

-	The largest component is selected using max (..., key=len).
 
-	The size of the largest component is calculated using len(original_lcc).

### b.	Average Path Length

•	**Method**: The average shortest path length is calculated for the largest strongly connected component.

•	**Technique**:

o	Strongly connected components are computed using nx.strongly_connected_components(G).

o	The largest strongly connected component is selected using max (..., key=len).

o	The average shortest path length is computed using nx.average_shortest_path_length(subgraph).

### c.	Network Diameter

•	**Method**: The diameter of the largest strongly connected component is calculated.

•	**Technique**:

o	The diameter is computed using nx.diameter(subgraph).

o	This measures the longest shortest path in the subgraph.

##	Removal of Low-Degree Nodes

•	**Method**: Low-degree nodes are removed from the graph.

•	**Technique**:

o	Nodes are removed using G.remove_nodes_from(low_degree_nodes).
o	This operation modifies the graph in-place, and the network properties are recalculated after removal.

## Visualization

•	**Method**: A bar chart is used to compare the size of the largest connected component before and after the removal of low-degree nodes.

•	**Technique**:

o	The matplotlib library is used to create bar charts.

o	The x-axis represents the two states ("Before Removal" and "After Removal"), and the y-axis represents the size of the largest connected component.

## Key Algorithms and Functions Used

•	**Weakly Connected Components**: nx.weakly_connected_components(G)

o	Identifies components in a directed graph where nodes are connected if the graph is treated as undirected.

•	**Strongly Connected Components**: nx.strongly_connected_components(G)

o	Identifies components in a directed graph where nodes are mutually reachable.

•	**Average Shortest Path Length**: nx.average_shortest_path_length(subgraph)

o	Computes the average of the shortest paths between all pairs of nodes in the subgraph.

•	**Network Diameter**: nx.diameter(subgraph)

o	Computes the longest shortest path in the subgraph.

•	**Node Degree**: G.degree(node)

o	Computes the number of edges connected to a node.

## Summary of Methods

The analysis uses the following network analysis methods:

1.	Graph Construction: Building a directed graph from edge data.
2.	Node Degree Analysis: Identifying and removing low-degree nodes.
3.	Component Analysis: Identifying weakly and strongly connected components.
4.	Path-Based Metrics: Calculating average path length and diameter.
5.	Visualization: Comparing network properties before and after node removal.
 
## Method Applied in Research Ǫuestion 2 
Analyze the Relationship Between Sales Rank and Connectivity – Investigate the correlation between a product’s degree centrality and its Sales Rank to understand if highly connected products perform better in sales.

## Graph Construction
•	**Method**: A directed graph (DiGraph) is constructed from the dataset.

•	**Technique**:

o	The graph is built using the FromNodeId and ToNodeId columns from the dataset.

o	The nx.from_pandas_edgelist function is used to efficiently create the graph from a pandas DataFrame.

## Centrality Measures Calculation

Centrality measures are computed to quantify the importance or influence of nodes in the network. The following centrality measures are calculated:

## a.	**Degree Centrality**

•	**Method**: Measures the number of connections a node has.

•	**Technique**:

o	Computed using nx.degree_centrality(G).

o	Formula: Degree Centrality=Number of connectionsTotal possible connectio nsDegree Centrality=Total possible connectionsNumber of connections.

### b.	**Betweenness Centrality**

•	**Method**: Measures the extent to which a node lies on the shortest paths between other nodes.

•	**Technique**:

o	Computed using nx.betweenness_centrality(G, k=500).

o	The k=500 parameter is used to approximate the centrality for large networks by sampling 500 nodes.

### c.	**PageRank**
 
•	**Method**: Measures the importance of a node based on the structure of the network.

•	**Technique**:

o	Computed using nx.pagerank(G, alpha=0.85).

o	The alpha parameter controls the damping factor (probability of random jumps in the network).

## Data Merging

•	**Method**: The centrality measures are merged with the original dataset to associate each node with its centrality values and sales rank.

•	**Technique**:

o	A DataFrame is created for centrality measures using pd.DataFrame.

o	The centrality measures are merged with the original dataset using df.merge().

## Correlation Analysis

The relationship between Sales Rank and Degree Centrality is analyzed using two correlation methods:

### a.	Spearman Correlation

•	**Method**: Measures the rank-based relationship between two variables.

•	**Technique**:

o	Computed using spearmanr(df_merged["SalesRank"], df_merged["DegreeCentrality"]).

o	Spearman correlation is non-parametric and assesses how well the relationship between two variables can be described by a monotonic function.

### b.	Pearson Correlation

•	**Method**: Measures the linear relationship between two variables.

•	**Technique**:

o	Computed using pearsonr(df_merged["SalesRank"], df_merged["DegreeCentrality"]).

o	Pearson correlation is parametric and assumes a linear relationship between the variables.

## Visualization
## •	Method: 
A scatter plot is used to visualize the relationship between Sales Rank and Degree Centrality.

## •	Technique:

o	The scatter plot is created using sns.scatterplot() from the Seaborn library.
o	The x-axis represents Degree Centrality, and the y-axis represents Sales Rank.
o	The plot includes transparency (alpha=0.5) to handle overlapping data points.

## Key Algorithms and Functions Used
•	**Graph Construction**:

o	nx.from_pandas_edgelist(): Efficiently constructs a graph from a pandas DataFrame.

•	**Centrality Measures**:

o	nx.degree_centrality(): Computes degree centrality.

o	nx.betweenness_centrality(): Computes betweenness centrality (with approximation for large networks).

o	nx.pagerank(): Computes PageRank centrality.

## Correlation Analysis:

o	spearmanr(): Computes Spearman correlation.
o	pearsonr(): Computes Pearson correlation.

•	**Visualization**:
 
o	sns.scatterplot(): Creates a scatter plot for visualizing relationships.

## Summary of Methods
The analysis uses the following methods:
1.	Graph Construction: Building a directed graph from edge data.
2.	Centrality Measures: Computing degree centrality, betweenness centrality, and PageRank to quantify node importance.
3.	Data Merging: Combining centrality measures with sales rank data for analysis.
4.	Correlation Analysis: Quantifying the relationship between Sales Rank and Degree Centrality using Spearman and Pearson correlations.
5.	Visualization: Creating a scatter plot to visually explore the relationship between Sales Rank and Degree Centrality.

## Strengths of the Methods
•	Comprehensive Centrality Analysis: Multiple centrality measures are computed to capture different aspects of node importance.

•	Robust Correlation Analysis: To understand the relationship, rank-based (Spearman) and linear (Pearson) correlations are used.

•	Effective Visualization: The scatter plot provides a clear visual representation of the distribution and trends.


 ### Method Applied in Research Ǫuestion 3 
 Detect Key Bridge Products – Compute betweenness centrality to identify the top 10 products that act as intermediaries, bridging different product categories.
 
## Graph Construction
•	**Method**: A directed graph (DiGraph) is constructed from the dataset.
•	**Technique**:
o	The graph is built using the FromNodeId and ToNodeId columns from the dataset.
 
o	The G.add_edges_from(df) function is used to efficiently add edges to the graph.

##	Betweenness Centrality Calculation
•	**Method**: Betweenness Centrality is computed to identify bridge products.
•	**Technique**:
o	Computed using nx.betweenness_centrality(G, k=500).
o	The k=500 parameter is used to approximate the centrality for large networks by sampling 500 nodes.
o	Betweenness Centrality measures the extent to which a node lies on the shortest paths between other nodes, making it a key metric for identifying bridge products.

## Top 10 Bridge Products Identification
•	**Method**: The top 10 nodes with the highest Betweenness Centrality are identified.
•	**Technique**:
o	The centrality values are sorted in descending orderusing sorted(betweenness_centrality.items(), key=lambda x: x[1], reverse=True)[:10].
o	The top 10 nodes are extracted and stored in a DataFrame for visualization.

## Data Visualization
•	**Method**: A bar plot is used to visualize the Betweenness Centrality of the top 10 bridge products.
•	**Technique**:
o	The bar plot is created using sns.barplot() from the Seaborn library.
o	The x-axis represents the Product Node ID, and the y-axis represents the Betweenness Centrality.
o	The plot includes color coding (palette="viridis") and disables the legend for clarity.

### Key Algorithms and Functions Used
•	**Graph Construction**:
o	nx.DiGraph(): Creates a directed graph.
o	G.add_edges_from(): Adds edges to the graph from a list of tuples.
•	**Betweenness Centrality**:
o	nx.betweenness_centrality(): Computes Betweenness Centrality for all nodes in the graph.
o	The k=500 parameter is used to approximate the centrality for large networks.
•	**Sorting and Filtering**:
o	sorted(): Sorts the centrality values in descending order.
o	List slicing ([:10]): Extracts the top 10 nodes.
•	**Data Visualization**:
o	sns.barplot(): Creates a bar plot for visualizing the top 10 bridge products.

## Summary of Methods
The analysis uses the following methods:
1.	Graph Construction: Building a directed graph from edge data.
2.	Betweenness Centrality Calculation: Computing Betweenness Centrality to identify bridge products.
3.	Top 10 Bridge Products Identification: Sorting and filtering the top 10 nodes with the highest centrality.
4.	Data Visualization: Creating a bar plot to visualize the results.
   
5.	Subgraph Creation:
	__The subgraph includes:__

a.	The top 10 bridge products (nodes with the highest Betweenness Centrality).
b.	Their immediate connections (predecessors and successors in the directed graph).

6.	Visualization:
a.	Top 10 Bridge Products: Highlighted in red with larger node sizes.
b.	Connected Products: Highlighted in blue with smaller node sizes.
c.	Edges: Represented as gray lines to show connections between nodes.
b.	Labels: Added to nodes for better identification.

7.	Layout:
a.	The nx.spring_layout function is used to position the nodes visually appealingly.

8.	Legend:
a.	A legend is added to distinguish between the top 10 bridge products and their connected products.













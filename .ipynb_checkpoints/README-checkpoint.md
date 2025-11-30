# MSCS 634 Lab 6: Association Rule Mining

## Purpose of the Lab

This lab demonstrates association rule mining using two fundamental algorithms: **Apriori** and **FP-Growth**. The objective is to discover frequent itemsets and generate meaningful association rules from transactional data, specifically analyzing genre patterns in the IMDB Top 1000 movies dataset.

### Objectives
- Transform a non-transactional dataset into a transactional format suitable for association rule mining
- Implement and compare Apriori and FP-Growth algorithms for frequent itemset mining
- Generate association rules with support, confidence, and lift metrics
- Analyze and interpret discovered patterns to extract meaningful insights
- Compare algorithm performance and understand their trade-offs

## Dataset

**IMDB Top 1000 Movies Dataset**
- **Source**: Kaggle (IMDB dataset of top 1000 movies and TV shows)
- **Transactions**: 1,000 movies
- **Items**: 21 unique movie genres
- **Format**: Each movie (transaction) contains one or more genres (items)

The dataset was transformed by treating each movie as a transaction and its genres as items, creating a transactional dataset suitable for association rule mining.

## Key Insights from Analysis

### Frequent Itemsets Discovered
- **Total frequent itemsets** (support ≥ 5%): 28 itemsets
- **1-itemsets**: 15 single genres (e.g., Drama, Comedy, Crime)
- **2-itemsets**: 13 genre combinations (e.g., Drama-Crime, Drama-Comedy)

**Most Frequent Genre Combinations:**
- Drama + Crime (16.0% support)
- Drama + Comedy (12.3% support)
- Action + Adventure (8.3% support)

### Association Rules Generated
- **Total rules** (confidence ≥ 50%): Varies based on support threshold
- **Strongest associations**: Rules with confidence ≥ 70% and lift > 1.2

**Key Genre Associations Discovered:**
1. **Drama** appears in the highest number of movies (72.4%) and frequently combines with other genres
2. **Crime-Drama** combination is the most common 2-itemset, indicating strong association
3. **Comedy-Drama** shows significant co-occurrence, suggesting hybrid genre popularity
4. **Action-Adventure** pairs frequently, reflecting common movie categorization

### Metric Insights
- **Support**: Drama has the highest support (0.724), appearing in 724 out of 1000 movies
- **Confidence**: Strong rules show confidence values ranging from 50% to 90%+
- **Lift**: Many rules have lift > 1.0, indicating positive associations between genres
- **Pattern**: Multi-genre movies are common, with an average of 2.54 genres per movie

### Practical Applications
The discovered association rules can be used for:
- **Movie Recommendation Systems**: Predict genre preferences based on user history
- **Content Analysis**: Understand genre trends in top-rated movies
- **Market Insights**: Identify popular genre combinations for content creation
- **Pattern Discovery**: Reveal hidden relationships between movie genres

## Challenges Faced and Decisions Made

### Challenge 1: Dataset Format Transformation
**Problem**: The IMDB dataset is not naturally in transactional format. It contains movie information with comma-separated genre strings.

**Solution**: Transformed the dataset by:
- Splitting comma-separated genre strings into lists
- Treating each movie as a transaction
- Treating each genre as an item
- Creating a one-hot encoded matrix for algorithm compatibility

**Decision**: Used genres as items rather than other attributes (directors, actors) because genres naturally form multi-item transactions suitable for association rule mining.

### Challenge 2: Library Dependencies
**Problem**: The `mlxtend` library was not installed in the environment which caused some errors.

**Solution**: Installed the library using `pip install mlxtend`, which provides efficient implementations of both Apriori and FP-Growth algorithms.

### Challenge 3: Support Threshold Selection
**Problem**: Determining the optimal support threshold was challenging - too high would miss patterns, too low would generate excessive itemsets.

**Solution**: 
- Analyzed dataset characteristics (1000 transactions, 21 unique items)
- Experimented with multiple thresholds (0.01, 0.05, 0.10, 0.15, 0.20)
- Selected **0.05 (5%)** as optimal balance:
  - Ensures minimum 50 transactions per itemset
  - Captures meaningful patterns without overwhelming output
  - Maintains computational efficiency

**Decision**: Chose 5% support threshold to balance pattern discovery with result interpretability.

## Algorithm Comparison Results

### Performance
- **Faster Algorithm**: FP-Growth (typically 1.5-3x faster)
- **Execution Time**: FP-Growth averages faster execution times
- **Scalability**: FP-Growth shows better performance as dataset size increases

### Why FP-Growth is Faster
1. **No Candidate Generation**: FP-Growth uses pattern growth approach, avoiding expensive candidate generation
2. **Compact Data Structure**: Uses FP-tree to compress database, reducing memory access
3. **Fewer Database Scans**: Requires only 2 scans vs. multiple scans for Apriori
4. **Better Complexity**: O(n × m) vs. Apriori's O(2^d × n) for larger itemsets

### Result Consistency
- Both algorithms produce **identical frequent itemsets**
- Same 28 itemsets found with support ≥ 0.05
- Confirms correctness of both implementations

# **Supermarket Sales Predictor**

This project aims to predict the quantity of products sold in a supermarket using regression models and identify transaction characteristics by clustering the data. The project provides valuable insights into supermarket sales and customer purchasing behaviors.

---

## **Project Overview**
Supermarkets generate large amounts of transactional data daily. Analyzing this data helps understand customer purchasing patterns, predict sales, and optimize product offerings. This project uses machine learning techniques to:  
- **Predict Quantity Sold**: Build regression models to predict the quantity of products sold.  
- **Cluster Transactions**: Group transactions using **K-Means Clustering** to understand patterns among different types of purchases.  

---

## **Dataset Structure**
The dataset contains detailed information about transactions:  

```
dataset/
├── Supermarket_sales.csv
```

### **Columns in the Dataset**
- **Product Details**:  
  - `Product Category`, `Product Subcategory`, `Product Rating`, `Product Score 1`, `Product Score 2`, `Product ID`.  
- **Transaction Details**:  
  - `Transaction Number`, `Transaction Date`, `Total Amount Per Unit`, `Quantity`.  
- **Customer Details** (with missing values):  
  - `Age`, `State Code`, `Annual Income`, `Occupation Level`.  
- **Seller Details**:  
  - `Seller ID`.  
- **Pricing Details**:  
  - `Base Price`, `Discount Percentage`, `Service Charge Percentage`.  

---

## **Data Preprocessing**

### **1. Handling Missing Values**
- Columns with missing values (`Age`, `State Code`, `Annual Income`, `Occupation Level`) were imputed using **KNNImputer** to replace missing entries with the nearest neighbors.  

### **2. Feature Engineering**
- **Cyclical Features**:  
  - Extracted `Day of Week` and `Month of Year` from the `Transaction Date` to encode cyclical patterns in time-related data.  

### **3. Feature Reduction**
- Dropped irrelevant columns (e.g., `Transaction Number`, `Customer ID`, `Seller ID`) to focus on attributes directly influencing sales and clustering.  

### **4. Feature Scaling**
- Applied MinMax Scaling to ensure numerical columns had values between 0 and 1.  

### **5. Categorical Encoding**
- Encoded categorical variables using **Label Encoding** to prepare product category and subcategory for regression and clustering models.  

### **6. Outlier Removal**
- Visualized outliers using boxplots and removed them using the **IQR method** (values outside fences).  

---

## **Exploratory Data Analysis**

- **Bar Plots**: Explored data distributions and relationships, observing:  
  - Product score affects quantity sold.  
  - Total amount per unit impacts quantity sold.  
- **Sweetviz Analysis**: Comprehensive visual report of the dataset. [Link to Sweetviz HTML](#).  

### **1. Correlation Analysis**
- Generated a **Correlation Heatmap** to identify relationships between features.  
- Strong correlations were observed between:  
  - `Product Score 1` and `Base Price`.  
  - `Product Score 2` and `Product Rating`.  

### **2. Dimensionality Reduction**
- **Principal Component Analysis (PCA)**:  
  - Merged correlated features into single components for better model performance:  
    - `Product Score 1` and `Base Price`.  
    - `Product Score 2` and `Product Rating`.  

---

## **Model Building and Results**

### **Regression Models**
The following regression models were tested using **5-Fold Cross Validation** after hyperparameter tuning:  

| **Model**                  | **MSE**  | **R²**  | **RMSE** |  
|----------------------------|----------|---------|----------|  
| **Linear Regression**      | 147.38   | -0.00   | 12.14    |  
| **Lasso Regression**       | 146.80   | -0.00   | 12.12    |  
| **Decision Tree Regressor**| 321.26   | -1.19   | 17.92    |  
| **Random Forest Regressor**| 158.38   | -0.08   | 12.58    |  
| **Ridge Regression**       | 147.38   | -0.00   | 12.14    |  
| **Polynomial Regression**  | 150.77   | -0.03   | 12.28    |  

### **Selected Model**
- **Lasso Regression** was chosen for its lowest **Mean Squared Error (MSE)** and simplicity.  

### **Feature Importance**
- Features influencing sales:  
  - **Product Subcategory**.  
  - **Day of the Week**.  
  - **Month**.  

---

## **Clustering Analysis**

### **K-Means Clustering**
- Clustering was performed on transactions from the past 6 months using the following features:  
  - `Product Category`, `Product Subcategory`, `Product Score 1`, `Product Score 2`, `Total Amount Per Unit`, `Product Rating`.  
- **Optimal K**: Determined using the **Elbow Method**, which identified **K=5** as the optimal number of clusters.  

---

## **Cluster Insights**
The following patterns were observed among the clusters:

| **Cluster** | **Characteristics** |  
|-------------|---------------------|  
| **Cluster 0** | **High-End Buyers with Moderate Product Rating**: Buyers prioritize high-quality products and are forgiving of minor flaws. |  
| **Cluster 1** | **Top-Rated Products with Moderate Spending**: Buyers focus on highly-rated products that balance quality and price. |  
| **Cluster 2** | **Balanced Products for Value-Conscious Buyers**: Buyers seek good performance at reasonable costs. |  
| **Cluster 3** | **Budget-Friendly Products with Average Ratings**: Buyers prioritize affordability while accepting basic functionality. |  
| **Cluster 4** | **Niche Market for Average-Quality Products**: Buyers pay more for specific features, even with average reviews. |  

## **Interpretation**
**Cluster 0:**

High-End Buyers with Moderate Product Rating: This cluster represents products with high product score 1 (51.31), slightly below average product score 2 (-1.20), and high total amount per unit (30.29). However, they have a moderate product rating (3.05). This suggests that buyers in this cluster prioritize high-quality products but are more forgiving of minor flaws. They might be willing to pay more for a product that meets their basic needs but may not be overly concerned with aesthetics or additional features.

**Cluster 1:**

Top-Rated Products with Moderate Spending: This cluster includes products with the highest product score 1 (97.50), slightly above average product score 2 (1.01), and a moderate total amount per unit (59.51). Additionally, they have the highest product rating (4.43). This indicates that buyers in this cluster prioritize both high-quality products and positive customer reviews. They are willing to spend more on products that are highly rated and offer excellent performance.

**Cluster 2:**

Balanced Products for Value-Conscious Buyers: This cluster consists of products with moderate product score 1 (56.35), slightly below average product score 2 (-0.86), and a moderate total amount per unit (33.06). Their product rating is also moderate (3.49). This suggests that buyers in this cluster value a balance between quality and price. They are looking for products that offer good performance at a reasonable cost.

**Cluster 3:**

Budget-Friendly Products with Average Rating: This cluster includes products with moderate product score 1 (52.38), the lowest product score 2 (-1.66), and the lowest total amount per unit (29.32). Their product rating is average (4.33). This suggests that buyers in this cluster prioritize affordability. They are looking for products that offer basic functionality at a low price point and may be less concerned with minor flaws or lack of advanced features.

**Cluster 4:**

Average-Quality Products for Niche Market: This cluster consists of products with slightly above average product score 1 (65.18), slightly above average product score 2 (0.42), and the highest total amount per unit (40.75). However, their product rating is the lowest (2.50). This suggests that buyers in this cluster prioritize specific features or cater to a niche market. They might be willing to pay more for a product that meets their unique needs, even if the overall quality or customer reviews are average.


---
## **How to Use**
### **Prerequisites**
1. Python 3.7+  
2. Install required libraries:
   ```bash
   pip install numpy pandas scikit-learn matplotlib
   ```

---

## **Future Improvements**
1. **Deep Learning Models**: Use advanced neural networks like ANN or RNN for capturing complex, non-linear relationships in the data.  
2. **Time-Series Analysis**: Incorporate temporal patterns in the data for better sales forecasting.  
3. **Enhanced Feature Engineering**: Explore additional cyclical and interaction features for improved model accuracy.  

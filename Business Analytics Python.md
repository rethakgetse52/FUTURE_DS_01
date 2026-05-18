# FUTURE_DS_01
Data Science &amp; Analytics
# ============================================
# BUSINESS SALES PERFORMANCE ANALYTICS
# Superstore Dataset Analysis
# CREATED BY NATHANIEL RETHAKGETSE MOKOENA
# ============================================

# Import libraries
import pandas as pd
import matplotlib.pyplot as plt

# ============================================
# LOAD DATASET
# ============================================

df = pd.read_csv("Sample - Superstore.csv", encoding='latin1')

# ============================================
# VIEW DATA
# ============================================

print(df.head())

# Dataset information
print(df.info())

# ============================================
# DATA CLEANING
# ============================================

# Check missing values
print(df.isnull().sum())

# Remove duplicates
df = df.drop_duplicates()

# Convert Order Date to datetime
df['Order Date'] = pd.to_datetime(df['Order Date'])

# Create Month-Year column
df['Month'] = df['Order Date'].dt.to_period('M')

# ============================================
# KPI ANALYSIS
# ============================================

# Total Sales
total_sales = df['Sales'].sum()

# Total Profit
total_profit = df['Profit'].sum()

# Total Orders
total_orders = df['Order ID'].nunique()

print("\n===== KPI SUMMARY =====")
print("Total Sales:", total_sales)
print("Total Profit:", total_profit)
print("Total Orders:", total_orders)

# ============================================
# MONTHLY SALES TREND
# ============================================

monthly_sales = df.groupby('Month')['Sales'].sum()

print("\n===== MONTHLY SALES =====")
print(monthly_sales)

# Plot Monthly Sales Trend
plt.figure(figsize=(12,6))

monthly_sales.plot(kind='line', marker='o')

plt.title("Monthly Sales Trend")
plt.xlabel("Month")
plt.ylabel("Sales")

plt.xticks(rotation=45)

plt.grid(True)

plt.show()

# ============================================
# TOP 10 PRODUCTS
# ============================================

top_products = df.groupby('Product Name')['Sales'].sum()

top_products = top_products.sort_values(ascending=False).head(10)

print("\n===== TOP PRODUCTS =====")
print(top_products)

# Plot Top Products
plt.figure(figsize=(10,6))

top_products.sort_values().plot(kind='barh')

plt.title("Top 10 Products by Sales")
plt.xlabel("Sales")
plt.ylabel("Product")

plt.show()

# ============================================
# SALES BY CATEGORY
# ============================================

category_sales = df.groupby('Category')['Sales'].sum()

print("\n===== CATEGORY SALES =====")
print(category_sales)

# Plot Category Sales
plt.figure(figsize=(8,5))

category_sales.plot(kind='bar')

plt.title("Sales by Category")
plt.xlabel("Category")
plt.ylabel("Sales")

plt.show()

# ============================================
# SALES BY REGION
# ============================================

region_sales = df.groupby('Region')['Sales'].sum()

print("\n===== REGION SALES =====")
print(region_sales)

# Plot Region Sales
plt.figure(figsize=(8,5))

region_sales.plot(kind='bar')

plt.title("Sales by Region")
plt.xlabel("Region")
plt.ylabel("Sales")

plt.show()

# ============================================
# CUSTOMER SEGMENT ANALYSIS
# ============================================

segment_sales = df.groupby('Segment')['Sales'].sum()

print("\n===== CUSTOMER SEGMENT SALES =====")
print(segment_sales)

# Plot Segment Sales
plt.figure(figsize=(8,5))

segment_sales.plot(kind='pie', autopct='%1.1f%%')

plt.title("Sales by Customer Segment")
plt.ylabel("")

plt.show()

# ============================================
# PROFIT ANALYSIS
# ============================================

profit_by_category = df.groupby('Category')['Profit'].sum()

print("\n===== PROFIT BY CATEGORY =====")
print(profit_by_category)

# ============================================
# DISCOUNT IMPACT
# ============================================

discount_profit = df.groupby('Discount')['Profit'].mean()

print("\n===== DISCOUNT VS PROFIT =====")
print(discount_profit)

# ============================================
# BUSINESS INSIGHTS
# ============================================

best_category = category_sales.idxmax()
best_region = region_sales.idxmax()
best_segment = segment_sales.idxmax()

print("\n===== BUSINESS INSIGHTS =====")

print(f"The best-performing category is {best_category}.")
print(f"The highest sales region is {best_region}.")
print(f"The strongest customer segment is {best_segment}.")

print("""
Recommendations:
1. Focus marketing on high-performing regions.
2. Increase inventory for top-selling products.
3. Review high discounts that reduce profit.
4. Target profitable customer segments.
""")

# ============================================
# OPTIONAL: EXPORT CLEANED DATA
# ============================================

df.to_csv("Cleaned_Superstore_Data.csv", index=False)

print("\nAnalysis Complete!")

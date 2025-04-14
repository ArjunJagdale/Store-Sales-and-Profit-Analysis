# 📊 Store Sales and Profit Analysis using Python

## 🧾 Overview
The goal of this project is to perform a detailed sales and profit analysis for a retail store using a publicly available dataset. This analysis helps in uncovering business trends across categories, sub-categories, customer segments, and time periods.

## 📂 Dataset Information
The dataset used for this project is sourced from Kaggle and includes the following key features:

- **Order Info:** Order ID, Order Date, Ship Date, Ship Mode  
- **Customer Info:** Customer ID, Customer Name, Segment  
- **Location Info:** Country, City, State, Postal Code, Region  
- **Product Info:** Category, Sub-Category, Product Name  
- **Sales Info:** Sales, Quantity, Discount, Profit

You can download the dataset from Kaggle: **[Sample - Superstore.csv](https://www.kaggle.com/datasets/)**

---

## 🛠️ Tools and Libraries Used
- **Pandas**: For data handling
- **Plotly**: For interactive visualizations
- **NumPy**: (Implicitly used via pandas)

---

## 📥 Data Loading and Preprocessing

```python
import pandas as pd
import plotly.express as px
import plotly.graph_objects as go
import plotly.io as pio
import plotly.colors as colors

pio.templates.default = "plotly_white"

# Load dataset
data = pd.read_csv("Sample - Superstore.csv", encoding='latin-1')
```

### Basic Exploration

```python
print(data.head())
print(data.describe())
```

### Datetime Feature Engineering

```python
data['Order Date'] = pd.to_datetime(data['Order Date'])
data['Ship Date'] = pd.to_datetime(data['Ship Date'])

data['Order Month'] = data['Order Date'].dt.month 
data['Order Year'] = data['Order Date'].dt.year
data['Order Day of Week'] = data['Order Date'].dt.dayofweek
```

---

## 📈 Exploratory Data Analysis (EDA)

### 🗓️ Monthly Sales Trend

```python
sales_by_month = data.groupby('Order Month')['Sales'].sum().reset_index()
fig = px.line(sales_by_month, x='Order Month', y='Sales', title='Monthly Sales Analysis')
fig.show()
```

---

### 📦 Sales by Category

```python
sales_by_category = data.groupby('Category')['Sales'].sum().reset_index()

fig = px.pie(sales_by_category, values='Sales', names='Category', hole=0.5,
             color_discrete_sequence=px.colors.qualitative.Pastel)

fig.update_traces(textposition='inside', textinfo='percent+label')
fig.update_layout(title_text='Sales Analysis by Category', title_font=dict(size=24))
fig.show()
```

---

### 📊 Sales by Sub-Category

```python
sales_by_subcategory = data.groupby('Sub-Category')['Sales'].sum().reset_index()
fig = px.bar(sales_by_subcategory, x='Sub-Category', y='Sales', title='Sales Analysis by Sub-Category')
fig.show()
```

---

### 💰 Monthly Profit Trend

```python
profit_by_month = data.groupby('Order Month')['Profit'].sum().reset_index()
fig = px.line(profit_by_month, x='Order Month', y='Profit', title='Monthly Profit Analysis')
fig.show()
```

---

### 💼 Profit by Category

```python
profit_by_category = data.groupby('Category')['Profit'].sum().reset_index()

fig = px.pie(profit_by_category, values='Profit', names='Category', hole=0.5,
             color_discrete_sequence=px.colors.qualitative.Pastel)

fig.update_traces(textposition='inside', textinfo='percent+label')
fig.update_layout(title_text='Profit Analysis by Category', title_font=dict(size=24))
fig.show()
```

---

### 🧾 Profit by Sub-Category

```python
profit_by_subcategory = data.groupby('Sub-Category')['Profit'].sum().reset_index()
fig = px.bar(profit_by_subcategory, x='Sub-Category', y='Profit', title='Profit Analysis by Sub-Category')
fig.show()
```

---

### 👥 Sales and Profit by Customer Segment

```python
sales_profit_by_segment = data.groupby('Segment').agg({'Sales': 'sum', 'Profit': 'sum'}).reset_index()

color_palette = colors.qualitative.Pastel

fig = go.Figure()
fig.add_trace(go.Bar(x=sales_profit_by_segment['Segment'], y=sales_profit_by_segment['Sales'],
                     name='Sales', marker_color=color_palette[0]))
fig.add_trace(go.Bar(x=sales_profit_by_segment['Segment'], y=sales_profit_by_segment['Profit'],
                     name='Profit', marker_color=color_palette[1]))

fig.update_layout(title='Sales and Profit Analysis by Customer Segment',
                  xaxis_title='Customer Segment', yaxis_title='Amount')
fig.show()
```

---

### 📉 Segment-wise Sales-to-Profit Ratio

```python
sales_profit_by_segment['Sales_to_Profit_Ratio'] = sales_profit_by_segment['Sales'] / sales_profit_by_segment['Profit']
print(sales_profit_by_segment[['Segment', 'Sales_to_Profit_Ratio']])
```

| Segment       | Sales to Profit Ratio |
|---------------|------------------------|
| Consumer      | 8.66                   |
| Corporate     | 7.68                   |
| Home Office   | 7.13                   |

---

## 🧾 Summary
Store sales and profit analysis provides valuable insights into customer behavior, product performance, and market trends. This analysis enables businesses to:

- Optimize pricing and marketing strategies
- Improve supply chain efficiency
- Make informed decisions based on time-wise and category-wise performance
- Target profitable customer segments

---

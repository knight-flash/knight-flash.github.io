

---
title: "Python数据可视化实战"
collection: portfolio
type: "Data Analysis"
permalink: /portfolio/python-data-visualization
date: 2025-11-19
excerpt: "掌握Matplotlib和Seaborn绘制常用统计图表，为数据分析和模型评估创建可视化图形，编写可复用绘图函数。"
header:
  teaser: /images/portfolio/python-data-visualization/age_distribution.png
tags:
  - Data Visualization
  - Python
  - Matplotlib
  - Seaborn
  - Exploratory Data Analysis
tech_stack:
  - name: Python
  - name: Matplotlib
  - name: Seaborn
  - name: Scikit-learn
  - name: Pandas
  - name: NumPy
---

## 项目背景
本项目旨在通过Python实现数据可视化实战，掌握使用Matplotlib和Seaborn绘制多种常用统计图表，为探索性数据分析和模型评估创建合适的可视化图形，并编写可复用的绘图函数提升代码效率。项目涵盖数据探索（EDA）和模型评估两大核心模块。


## 核心实现

### 1. 探索性数据分析可视化
**箱线图绘制函数**  
用于比较不同结局下各实验室指标分布：
```python
import matplotlib.pyplot as plt
import seaborn as sns

def plot_lab_boxplots(data, columns, target_col):
    fig, axs = plt.subplots(3, 2, constrained_layout=True, figsize=(10,10))
    for i, col in enumerate(columns):
        sns.boxplot(data=data, x=target_col, y=col, ax=axs[i//2, i%2])
    plt.suptitle("不同结局下各实验室指标分布", fontsize=16)
    plt.show()

# 使用示例
lab_columns = ['age_month', 'lab_5237_min', 'lab_5227_min', 'lab_5225_range', 'lab_5235_max', 'lab_5257_min']
plot_lab_boxplots(picu_data, lab_columns, 'HOSPITAL_EXPIRE_FLAG')
```

### 2. 模型评估可视化
**混淆矩阵热力图函数**  
用于评估分类模型性能：
```python
from sklearn.metrics import confusion_matrix

def confusion_matrix_plot(y_true, y_pred_prob, threshold=0.5):
    y_pred = (y_pred_prob > threshold).astype(int)
    cm = confusion_matrix(y_true, y_pred)
    
    fig, ax = plt.subplots(figsize=(5,4))
    sns.heatmap(cm, annot=True, fmt='d', cmap='Blues', ax=ax)
    ax.set_xlabel('预测标签')
    ax.set_ylabel('真实标签')
    ax.set_title('混淆矩阵')
    ax.xaxis.set_ticklabels(['存活(0)', '死亡(1)'])
    ax.yaxis.set_ticklabels(['存活(0)', '死亡(1)'])
    plt.show()

# 使用示例
confusion_matrix_plot(y_test, y_pred_prob, threshold=0.5)
```


## 分析结果

### 1. 数据分布探索
![年龄分布直方图](/images/portfolio/python-data-visualization/age_distribution.png)  
**分析结论**：患者年龄分布呈现多峰特征，主要集中在低龄段和中老年段，反映PICU患者年龄跨度较大。

![实验室指标箱线图](/images/portfolio/python-data-visualization/lab_indicators_boxplot.png)  
**分析结论**：部分实验室指标（如lab_5235_max）在存活与死亡患者间存在显著分布差异，可作为潜在预后预测因子。

### 2. 模型评估结果
![混淆矩阵热力图](/images/portfolio/python-data-visualization/confusion_matrix.png)  
**分析结论**：逻辑回归模型在测试集上表现良好，真阳性率较高，但仍存在一定假阳性和假阴性，需进一步优化阈值或特征工程。

![ROC曲线](/images/portfolio/python-data-visualization/roc_curve.png)  
**分析结论**：ROC曲线下面积（AUC）达到0.85，表明模型具有较好的区分能力，适用于临床风险评估场景。



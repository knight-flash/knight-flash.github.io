---
title: "Python数据可视化与模型评估实战"
collection: portfolio
type: "Machine Learning"
permalink: /portfolio/python-data-visualization-practice
date: 2024-01-01
excerpt: "使用Matplotlib和Seaborn进行探索性数据分析，并通过混淆矩阵和ROC曲线评估逻辑回归模型性能"
header:
  teaser: /images/portfolio/python-data-visualization/age_distribution.png
tags:
  - 数据可视化
  - 探索性数据分析
  - 模型评估
  - 逻辑回归
tech_stack:
  - name: Python
  - name: Matplotlib
  - name: Seaborn
  - name: Scikit-learn
---

## 项目背景  
本项目旨在通过Python实现数据可视化与模型评估的完整流程，掌握以下核心技能：  
1. 使用Matplotlib和Seaborn绘制直方图、箱线图等常用统计图表  
2. 为探索性数据分析（EDA）选择合适的可视化方法  
3. 通过混淆矩阵和ROC曲线评估机器学习模型性能  


## 核心实现  

### 1. 探索性数据分析（EDA）  
**年龄分布直方图**  
通过直方图观察患者年龄分布：  
```python
plt.figure(figsize=(8, 5))
sns.histplot(data=picu_data, x='age_month', kde=True)
plt.title("年龄分布直方图")
plt.show()
```  
**实验室指标箱线图**  
比较存活与死亡患者的实验室指标分布：  
```python
colname = ['age_month', 'lab_5237_min', 'lab_5227_min', 'lab_5225_range', 'lab_5235_max', 'lab_5257_min']
fig, axs = plt.subplots(3, 2, constrained_layout=True, figsize=(10, 10))

for i in range(len(colname)):
    sns.boxplot(data=picu_data, x='HOSPITAL_EXPIRE_FLAG', y=colname[i], ax=axs[i//2, i%2])

plt.suptitle("不同结局下各实验室指标分布", fontsize=16)
plt.show()
```  

### 2. 模型评估  
**混淆矩阵热力图**  
定义并调用混淆矩阵可视化函数：  
```python
def confusion_matrix_plot(y_true, y_pred_prob, threshold=0.5, title='混淆矩阵'):
    y_pred = (y_pred_prob > threshold).astype(int)
    cm = confusion_matrix(y_true, y_pred)
    
    fig, ax = plt.subplots(figsize=(5, 4))
    sns.heatmap(cm, annot=True, fmt='d', cmap='Blues', ax=ax)
    
    ax.set_xlabel('预测标签')
    ax.set_ylabel('真实标签')
    ax.set_title(title)
    ax.xaxis.set_ticklabels(['存活(0)', '死亡(1)'])
    ax.yaxis.set_ticklabels(['存活(0)', '死亡(1)'])
    
    plt.show()

# 调用函数
confusion_matrix_plot(y_true=y_test, y_pred_prob=y_pred_prob, threshold=0.5)
```  

**ROC曲线**  
绘制ROC曲线并计算AUC值：  
```python
fpr, tpr, thresholds = roc_curve(y_test, y_pred_prob)
roc_auc = roc_auc_score(y_test, y_pred_prob)

plt.figure(figsize=(8, 6))
plt.plot(fpr, tpr, color='darkorange', lw=2, label=f'ROC曲线 (AUC = {roc_auc:.2f})')
plt.plot([0, 1], [0, 1], color='navy', lw=2, linestyle='--')
plt.xlim([0.0, 1.0])
plt.ylim([0.0, 1.05])
plt.xlabel('假阳性率')
plt.ylabel('真阳性率')
plt.title('ROC曲线')
plt.legend(loc="lower right")
plt.show()
```  


## 分析结果  

### 1. 探索性数据分析结果  
![年龄分布直方图](/images/portfolio/python-data-visualization/age_distribution.png)  
年龄分布直方图显示患者年龄主要集中在0-120个月之间，呈右偏分布。  

![实验室指标箱线图](/images/portfolio/python-data-visualization/lab_indicators_boxplots.png)  
箱线图对比了存活与死亡患者的实验室指标分布，部分指标（如lab_5235_max）在两组间存在显著差异，可作为潜在预测因子。  

### 2. 模型评估结果  
![混淆矩阵](/images/portfolio/python-data-visualization/confusion_matrix.png)  
混淆矩阵显示模型在测试集上的表现：真阳性率和真阴性率较高，但存在少量假阳性和假阴性预测。  

![ROC曲线](/images/portfolio/python-data-visualization/roc_curve.png)  
ROC曲线下面积（AUC）为0.85，表明模型具有较好的判别能力，能够有效区分存活与死亡患者。  


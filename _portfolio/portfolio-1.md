---
title: "Python数据可视化实战"
collection: portfolio
type: "Data Analysis"
permalink: /portfolio/python-data-visualization-practice
date: 2025-11-18
excerpt: "使用Matplotlib和Seaborn进行数据可视化，包括直方图、箱线图、混淆矩阵和ROC曲线"
header:
  teaser: /images/portfolio/python-data-visualization-practice/roc_curve.png
tags:
  - 数据可视化
  - Matplotlib
  - Seaborn
  - 机器学习
tech_stack:
  - name: Python
  - name: Pandas
  - name: Matplotlib
  - name: Seaborn
  - name: Scikit-learn
---

## 项目背景  
本项目旨在通过Python数据可视化技术，掌握探索性数据分析和模型评估的核心方法。主要学习目标包括：  
1. 使用Matplotlib和Seaborn绘制常用统计图表  
2. 为不同分析目的选择合适的可视化图形  
3. 编写可复用的绘图函数提升效率  

项目使用PICU患者数据集，涵盖年龄分布分析、实验室指标对比、模型混淆矩阵和ROC曲线评估等内容。


## 核心实现  

### 1. 探索性数据分析可视化  
#### 年龄分布直方图  
使用Seaborn绘制年龄分布直方图，同时显示核密度估计曲线：  

```python
import matplotlib.pyplot as plt
import seaborn as sns

plt.figure(figsize=(8, 5))
sns.histplot(data=picu_data, x='age_month', kde=True)
plt.title("年龄分布直方图")
plt.show()
```  

**说明**：该图展示PICU患者年龄分布特征，帮助快速识别患者群体的年龄集中趋势。

#### 实验室指标箱线图  
通过循环绘制多子图，比较不同结局下各实验室指标的分布差异：  

```python
colname = ['age_month', 'lab_5237_min', 'lab_5227_min', 'lab_5225_range', 'lab_5235_max', 'lab_5257_min']
fig, axs = plt.subplots(3, 2, constrained_layout=True, figsize=(10, 10))

for i in range(len(colname)):
    sns.boxplot(data=picu_data, x='HOSPITAL_EXPIRE_FLAG', y=colname[i], ax=axs[i//2, i%2])

plt.suptitle("不同结局下各实验室指标分布", fontsize=16)
plt.show()
```  

**说明**：箱线图组可直观比较存活/死亡患者在各指标上的分布差异，识别潜在预后预测因子。


### 2. 模型评估可视化  
#### 混淆矩阵热力图  
定义混淆矩阵绘图函数，评估分类模型性能：  

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

# 使用示例
confusion_matrix_plot(y_true=y_test, y_pred_prob=y_pred_prob, threshold=0.5)
```  

**说明**：混淆矩阵清晰展示模型的真阳性、真阴性、假阳性和假阴性预测结果，帮助计算准确率、灵敏度等评估指标。

#### ROC曲线  
绘制ROC曲线并计算AUC值：  

```python
fpr, tpr, thresholds = roc_curve(y_test, y_pred_prob)
roc_auc = roc_auc_score(y_test, y_pred_prob)

plt.figure(figsize=(8, 6))
plt.plot(fpr, tpr, color='darkorange', lw=2, label=f'ROC曲线 (AUC = {roc_auc:.3f})')
plt.plot([0, 1], [0, 1], color='navy', lw=2, linestyle='--')
plt.xlim([0.0, 1.0])
plt.ylim([0.0, 1.05])
plt.xlabel('假阳性率')
plt.ylabel('真阳性率')
plt.title('ROC曲线')
plt.legend(loc="lower right")
plt.show()
```  

**说明**：ROC曲线反映模型在不同阈值下的敏感性和特异性权衡，AUC值量化模型整体性能。


## 分析结果  

### 1. 探索性分析结果  
![年龄分布直方图](/images/portfolio/age_distribution.png)  
**结论**：患者年龄分布呈现多峰特征，主要集中在婴幼儿和青少年阶段。  

![实验室指标箱线图](/images/portfolio/lab_indicators_boxplot.png)  
**结论**：死亡患者在多个实验室指标上表现出显著异常，尤其是炎症相关指标明显升高。

### 2. 模型评估结果  
![混淆矩阵](/images/portfolio/confusion_matrix.png)  
**结论**：模型对存活患者预测准确率较高，但对死亡患者存在一定漏诊（假阴性）。  

![ROC曲线](/images/portfolio/roc_curve.png)  
**结论**：模型ROC曲线下面积（AUC）为0.85，表现良好，适用于临床辅助决策。


## 项目总结  
本项目系统练习了数据可视化的核心技术，从探索性分析到模型评估形成完整流程。通过封装可复用函数（如混淆矩阵绘图）提升了代码效率，同时掌握了不同图表的选择原则和解读方法，为后续数据分析项目奠定了坚实基础。

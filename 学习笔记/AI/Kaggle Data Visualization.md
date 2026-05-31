---
title : 'Kaggle Data Visualization'
date : 2026-03-22T20:43:04+08:00
lastmod: 2026-05-04T12:22:45+08:00
description : "pd.plotting.register_matplotlib_converters()"
image : img/cat.jpg
draft : false
categories : ["AI"]
tags : ["学习笔记", "AI"]
---
# Kaggle Data Visualization

## Setup

```py
import pandas as pd
pd.plotting.register_matplotlib_converters()
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
print("Setup Complete")
```

### pandas read

```py
ign_data = pd.read_csv(ign_filepath, index_col = "Platform")
```

## Line Charts

```py
plt.figure(figsize = (12, 6))
sns.lineplot(data = museum_data)
plt.title("Monthly Visitors to Los Angeles City Museums")
```

```py
plt.figure(figsize = (12, 6))
plt.title("Monthly Visitors to Avila Adobe Museum")
sns.lineplot(data = museum_data["Avila Adobe"])
plt.xlabel("Avila Adobe")
```

## Bar Charts

```py
plt.figure(figsize=(8,6))
plt.title("Bar chart for racing games")
sns.barplot(y=ign_data.index, x=ign_data['Racing'])
plt.ylabel("platform")
```

## Heatmap



```py
plt.figure(figsize=(10,10))
plt.title("Average score for Each platform and genre")
sns.heatmap(data=ign_data, annot=True)
plt.xlabel("platform")
```


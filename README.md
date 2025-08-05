# 🏠 Sberbank Housing Data Analysis / Анализ данных о недвижимости Сбербанка

This project explores real estate pricing data using the **Sberbank housing dataset**.  
It includes statistical analysis, outlier detection, and log transformation to better understand the distribution of housing prices in Russia.

Этот проект посвящён анализу цен на недвижимость с использованием **набора данных Сбербанка**.  
В нём проводится статистическая обработка, выявление выбросов и логарифмирование для получения более наглядного распределения цен.

---

## 📦 Libraries Used / Используемые библиотеки

- `pandas`
- `numpy`
- `matplotlib`
- `seaborn`

To install the libraries / Установка библиотек:

```bash
pip install pandas numpy matplotlib seaborn
```

---

## 📊 Dataset / Набор данных

**File:** `drive/MyDrive/sber_data.csv`

**Main columns / Основные столбцы:**
- `price_doc` — Final sale price / Итоговая цена продажи
- `full_sq` — Total area in square meters / Общая площадь в квадратных метрах

---

## 🔍 Steps / Этапы анализа

### 1. 📈 Initial Visualization / Начальная визуализация

- Histogram and boxplot of `price_doc`  
- Гистограмма и boxplot распределения `price_doc`

### 2. 🧹 Outlier Removal (Tukey’s IQR) / Удаление выбросов (метод Тьюки)

```python
def outliers_iqr(data, feature, left=1.5, right=1.5):
    Q1 = data[feature].quantile(0.25)
    Q3 = data[feature].quantile(0.75)
    IQR = Q3 - Q1
    left_bound = Q1 - left * IQR
    right_bound = Q3 + right * IQR

    filter = data[(data[feature] >= left_bound) & (data[feature] <= right_bound)]
    return filter
```

- Based on interquartile range  
- Основан на межквартильном размахе

### 3. 🔢 Log Transformation / Логарифмирование

```python
data['price_doc_log'] = np.log(data['price_doc'] + 1)
```

- Applied `log(price_doc + 1)`  
- Применено логарифмирование для приближения к нормальному распределению

### 4. 🧮 Z-Score Outlier Removal / Удаление выбросов по Z-отклонению

```python
def outliers_z_score(data, feature, log_scale=True, left=3.7, right=3.7):
    if log_scale:
        feature_data = np.log(data[feature] + 1)
    else:
        feature_data = data[feature]

    mean = feature_data.mean()
    std = feature_data.std()

    lower_bound = mean - left * std
    upper_bound = mean + right * std

    mask = (feature_data >= lower_bound) & (feature_data <= upper_bound)
    return data[mask]
```

- Removes outliers based on deviation from mean in log scale  
- Удаляет выбросы по отклонению от среднего значения в логарифмической шкале

---

## 📷 Visualizations / Визуализации

> Add these files to your `/images` folder (if generated):  
> ❗ `price_distribution.png`, `log_price_distribution.png`, `boxplot_cleaned.png`

- Distribution of raw and log-transformed `price_doc`  
- Распределение исходных и логарифмированных цен  
- Boxplots before and after outlier removal  
- Boxplot до и после удаления выбросов

---

## 📁 Project Structure / Структура проекта

```
sber_data_analysis/
├── sber_data.csv
├── sber_analysis.ipynb
├── README.md
└── images/
    ├── price_distribution.png
    ├── log_price_distribution.png
    ├── boxplot_cleaned.png
```

---

## 🧠 Conclusions / Выводы

- `price_doc` is heavily skewed — strong right tail  
  `price_doc` сильно смещён вправо (асимметрия)
- Outlier removal (IQR + Z-score) improves clarity and model robustness  
  Удаление выбросов повышает чистоту данных и стабильность моделей
- Log transformation makes distribution closer to normal  
  Логарифмирование помогает нормализовать распределение

---

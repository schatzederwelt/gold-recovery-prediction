#  Добыча золота - исследование технологического процесса
![gold recovery](https://st4.depositphotos.com/10614052/25181/i/600/depositphotos_251812044-stock-photo-gold-nuggets-on-dark-background.jpg)
## Описание проекта

**ПРОБЛЕМА:**

В золотодобывающей отрасли есть большие риски запуска предприятий с **убыточными** характеристиками.

Основная характеристика  - низкий коэффициент `восстановления золота` из золотосодержащей руды.

**ЦЕЛЬ ПРОЕКТА:**

Построить прототип модели машинного обучения, которая сможет предсказывать `коэффициент обогащения` и поможет оптимизировать производство.

В нашем распоряжении различные `параметры` с 4-х последовательных этапов процесса **извлечения золота** из руды:

1. `rougher` — флотация

==> **черновой концентрат**


2. `primary_cleaner` — первичная очистка
3. `secondary_cleaner` — вторичная очистка
4. `final` — финальные характеристики

==> **финальный концентрат**

=========================================================================================

**ЛИЧНАЯ ЦЕЛЬ:**

- Попробовать свои силы в прикладных задачах **для промышленности**, разобраться в неизвестной `предметной области` и провести детальный анализ `EDA`.


- Научиться строить модели для датасетов с `большим количеством признаков`. 


- Применить на практике подход с **Feature Engineering** и добиться более `высоких результатов` предсказаний с помощью несложных алгоритмов.


- Научиться использовать для оценки моделей **пользовательские метрики**

[Посмотреть проект](Research_of_gold_recovery_process_v1.ipynb)

## Навыки

<div class="alert alert-success">
<br> ✔️ Исследовательский анализ ✔️ Временные интервалы</br>
<br> ✔️ Регрессия  ✔️ Числовые признаки </br>
<br> ✔️ Большой набор атрибутов (> 80) ✔️ Пайплайны обработки данных </br>
<br> ✔️ Матрицы корреляции  ✔️ Feature Engineering </br>
<br> ✔️ Аутлайеры и доверительные интервалы ✔️ Skewness распределений</br>
<br> ✔️ Кастомная метрика качества ✔️ Обучающие кривые </br>
</div>

## Инструменты

<div class="alert alert-success">
<br> ✔️ LinearRegression ✔️ DecisionTree ✔️ RandomForest </br>
<br> ✔️ PowerTransformer </br>
<br> ✔️ learning_curve </br>
<br> ✔️ make_scorer ✔️ cross_val_score </br>
</div>

## Исходные данные


```python
import pandas as pd

df = pd.read_csv("/datasets/gold_recovery_train_new.csv")
display(df.iloc[:, :6].head(3))
df.iloc[:, 84:].head(3)
```


<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>final.output.concentrate_ag</th>
      <th>final.output.concentrate_pb</th>
      <th>final.output.concentrate_sol</th>
      <th>final.output.concentrate_au</th>
      <th>final.output.recovery</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2016-01-15 00:00:00</td>
      <td>6.055403</td>
      <td>9.889648</td>
      <td>5.507324</td>
      <td>42.192020</td>
      <td>70.541216</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2016-01-15 01:00:00</td>
      <td>6.029369</td>
      <td>9.968944</td>
      <td>5.257781</td>
      <td>42.701629</td>
      <td>69.266198</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2016-01-15 02:00:00</td>
      <td>6.055926</td>
      <td>10.213995</td>
      <td>5.383759</td>
      <td>42.657501</td>
      <td>68.116445</td>
    </tr>
  </tbody>
</table>
</div>





<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>secondary_cleaner.state.floatbank5_b_level</th>
      <th>secondary_cleaner.state.floatbank6_a_air</th>
      <th>secondary_cleaner.state.floatbank6_a_level</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-500.470978</td>
      <td>14.151341</td>
      <td>-605.841980</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-500.582168</td>
      <td>13.998353</td>
      <td>-599.787184</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-500.517572</td>
      <td>14.028663</td>
      <td>-601.427363</td>
    </tr>
  </tbody>
</table>
</div>



Все поля имеют формат `название_этапа.тип_параметра.название_параметра` и отражают состояние процесса в указанный **момент времени**.

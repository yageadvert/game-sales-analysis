# Предобработка данных датасета о продажах игр

```text
new_games/
├── .gitignore
├── README.md
├── data/
    └── new_games.csv
└── notebooks/
    └── new_games.ipynb
```

### Цели и задачи проекта
В этой тетрадке используем датасет `new_games.csv`, который содержит информацию о продажах игр разных жанров и платформ, а также пользовательские и экспертные оценки игр. На примере датасета познакомимся с основными данными, проверим их на корректность и проведем предобработку, проведя необходимый срез данных.

### Описание данных
В проекте будут использованы данные датасета `new_games.csv` с таким описанием:
- `Name` — название игры.
- `Platform` — название платформы.
- `Year of Release` — год выпуска игры.
- `Genre` — жанр игры.
- `NA sales` — продажи в Северной Америке (в миллионах проданных копий).
- `EU sales` — продажи в Европе (в миллионах проданных копий).
- `JP sales` — продажи в Японии (в миллионах проданных копий).
- `Other sales` — продажи в других странах (в миллионах проданных копий).
- `Critic Score` — оценка критиков (от 0 до 100).
- `User Score` — оценка пользователей (от 0 до 10).
- `Rating` — рейтинг организации ESRB (англ. Entertainment Software Rating Board). Эта ассоциация определяет рейтинг компьютерных игр и присваивает им подходящую возрастную категорию.


### Содержимое проекта
1. Загрузка и знакомство с данными
2. Проверка ошибок в данных и их предобработка\
   2.1 Названия столбцов датафрейма\
   2.2 Типы данных\
       2.2.1 Корректировка типов данных\
   2.3 Наличие пропусков в данных\
   2.4 Явные и неявные дубликаты в данных
3. Фильтрация данных
4. Категоризация данных
5. Итоговый вывод

## 1. Загрузка и знакомство с данными

Загрузим необходимые библиотеки для анализа данных и данные из датасета `new_games.csv`. Затем выведем основную информацию о данных с помощью метода `info()` и первые строки датафрейма.


```python
# Импортируем библиотеку pandas
import pandas as pd
```


```python
# Выгружаем данные из датасета hotel_dataset.csv в датафрейм hotels
games = pd.read_csv('https://code.s3.yandex.net/datasets/new_games.csv')
```

## 2.Проверка ошибок в данных и их предобработка
### 2.1 Названия столбцов датафрейма


```python
# Выводим первые строки датафрейма на экран
games.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Platform</th>
      <th>Year of Release</th>
      <th>Genre</th>
      <th>NA sales</th>
      <th>EU sales</th>
      <th>JP sales</th>
      <th>Other sales</th>
      <th>Critic Score</th>
      <th>User Score</th>
      <th>Rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Wii Sports</td>
      <td>Wii</td>
      <td>2006.0</td>
      <td>Sports</td>
      <td>41.36</td>
      <td>28.96</td>
      <td>3.77</td>
      <td>8.45</td>
      <td>76.0</td>
      <td>8</td>
      <td>E</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Super Mario Bros.</td>
      <td>NES</td>
      <td>1985.0</td>
      <td>Platform</td>
      <td>29.08</td>
      <td>3.58</td>
      <td>6.81</td>
      <td>0.77</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mario Kart Wii</td>
      <td>Wii</td>
      <td>2008.0</td>
      <td>Racing</td>
      <td>15.68</td>
      <td>12.76</td>
      <td>3.79</td>
      <td>3.29</td>
      <td>82.0</td>
      <td>8.3</td>
      <td>E</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Wii Sports Resort</td>
      <td>Wii</td>
      <td>2009.0</td>
      <td>Sports</td>
      <td>15.61</td>
      <td>10.93</td>
      <td>3.28</td>
      <td>2.95</td>
      <td>80.0</td>
      <td>8</td>
      <td>E</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Pokemon Red/Pokemon Blue</td>
      <td>GB</td>
      <td>1996.0</td>
      <td>Role-Playing</td>
      <td>11.27</td>
      <td>8.89</td>
      <td>10.22</td>
      <td>1.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



Необходимо привести все названия столбцов к единому стилю snake case


```python
# Корректируем названия столбцов в соответствии со стилем snake case
games = games.rename(columns = lambda col: (col.strip().lower().replace(' ', '_')))
games.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Wii Sports</td>
      <td>Wii</td>
      <td>2006.0</td>
      <td>Sports</td>
      <td>41.36</td>
      <td>28.96</td>
      <td>3.77</td>
      <td>8.45</td>
      <td>76.0</td>
      <td>8</td>
      <td>E</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Super Mario Bros.</td>
      <td>NES</td>
      <td>1985.0</td>
      <td>Platform</td>
      <td>29.08</td>
      <td>3.58</td>
      <td>6.81</td>
      <td>0.77</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mario Kart Wii</td>
      <td>Wii</td>
      <td>2008.0</td>
      <td>Racing</td>
      <td>15.68</td>
      <td>12.76</td>
      <td>3.79</td>
      <td>3.29</td>
      <td>82.0</td>
      <td>8.3</td>
      <td>E</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Wii Sports Resort</td>
      <td>Wii</td>
      <td>2009.0</td>
      <td>Sports</td>
      <td>15.61</td>
      <td>10.93</td>
      <td>3.28</td>
      <td>2.95</td>
      <td>80.0</td>
      <td>8</td>
      <td>E</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Pokemon Red/Pokemon Blue</td>
      <td>GB</td>
      <td>1996.0</td>
      <td>Role-Playing</td>
      <td>11.27</td>
      <td>8.89</td>
      <td>10.22</td>
      <td>1.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### 2.2 Типы данных


```python
# Выводим информацию о датафрейме
games.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 16956 entries, 0 to 16955
    Data columns (total 11 columns):
     #   Column           Non-Null Count  Dtype  
    ---  ------           --------------  -----  
     0   name             16954 non-null  object 
     1   platform         16956 non-null  object 
     2   year_of_release  16681 non-null  float64
     3   genre            16954 non-null  object 
     4   na_sales         16956 non-null  float64
     5   eu_sales         16956 non-null  object 
     6   jp_sales         16956 non-null  object 
     7   other_sales      16956 non-null  float64
     8   critic_score     8242 non-null   float64
     9   user_score       10152 non-null  object 
     10  rating           10085 non-null  object 
    dtypes: float64(4), object(7)
    memory usage: 1.4+ MB



```python
# Сохраняем количество строк в датасете до того, как начнем предобработку
count_rows_before = games.shape[0]
print(f'Изначальное количество строк в датасете: {count_rows_before}')
```

    Изначальное количество строк в датасете: 16956


Датасет `new_games.csv` содержит 11 столбцов и 16956 строк, в которых представлена информация о продажах игр разных жанров и платформ.

Названия столбцов датасета были переименованы в соответствии со стилем snake case: `name`, `platform`, `year_of_release`, `genre`, `na_sales`, `eu_sales`, `jp_sales`, `other_sales`, `critic_score`, `user_score`, `rating`. Такой формат необходим для удобства чтения и поддержки чистоты кода

Изучим типы данных и их корректность:

- **Числовые значения с плавающей запятой (float64).** Четыре столбца имеют тип данных float64:
    - `year_of_release`- содержит год выпуска игры. Тип `float` может автоматически устанавливаться на столбцах с датой, где есть пропуски значений. Для анализа, возможно, нам не понадобится `datetime`, поэтому достаточно будет преобразовать в `int16` для "правильного" отображения в выводе, без плаващей точки.
    - `na_sales` - содержит продажи в Северной Америке, тип данных выбран корректно.
    - `other_sales` - содержит продажи в других странах, тип данных выбран корректно.
    - `critic_score` - содержит оценку критиков (от 0 до 100) в виде только целочисленных значений. Следовательно, оптимальным будет использовать тип даннх `int16`.
- **Строковые данные (object).** Семь столбцов имеют тип данных `object`:
    - `name`, `platform`, `genre` - содержат сроковую информацию (название игры, название платформы и жанр игры), что логично для текстовых данных. Здесь тип данных `object` подходит.
    - `rating` - также хранит текстовые данные. Но их можно рассматривать как категориальные признаки.
    - `eu_sales`, `jp_sales` содержат данные о продажах в формате вещественных чисел, поэтому тип данных неободимо заменить на float64.
    - `user_score` содержит оценки пользователей в формате вещественных чисел, поэтому тип данных неободимо заменить на float64.

После анализа типов данных видно, что некоторые столбцы представлены некорректно и необходимо преобразовать типы данных для оптимизации обработки. В числовых столбцах могут встретиться строковые значения, например `unknown` или другие, поэтому будем использовать команду `errors="coerce"` для замены строковых значений на пропуски.

Прежде чем изменить тип данных в столбце `user_score`, проверим какие значения там содержатся, потому что после перевода в `float64` мы потеряем эти данные.


```python
games['user_score'].unique()
```




    array(['8', nan, '8.3', '8.5', '6.6', '8.4', '8.6', '7.7', '6.3', '7.4',
           '8.2', '9', '7.9', '8.1', '8.7', '7.1', '3.4', '5.3', '4.8', '3.2',
           '8.9', '6.4', '7.8', '7.5', '2.6', '7.2', '9.2', '7', '7.3', '4.3',
           '7.6', '5.7', '5', '9.1', '6.5', 'tbd', '8.8', '6.9', '9.4', '6.8',
           '6.1', '6.7', '5.4', '4', '4.9', '4.5', '9.3', '6.2', '4.2', '6',
           '3.7', '4.1', '5.8', '5.6', '5.5', '4.4', '4.6', '5.9', '3.9',
           '3.1', '2.9', '5.2', '3.3', '4.7', '5.1', '3.5', '2.5', '1.9', '3',
           '2.7', '2.2', '2', '9.5', '2.1', '3.6', '2.8', '1.8', '3.8', '0',
           '1.6', '9.6', '2.4', '1.7', '1.1', '0.3', '1.5', '0.7', '1.2',
           '2.3', '0.5', '1.3', '0.2', '0.6', '1.4', '0.9', '1', '9.7'],
          dtype=object)




```python
(games["user_score"] == "tbd").sum()
```




    np.int64(2464)



Из последнего вычисления видим, что 2464 строки имеют значение 'tbd' в столбце `user_score`. Вполне вероятно, что TBD означает сокращение от английского выражения to be determined, которое переводится как «будет определено позже», «подлежит уточнению» или «будет решено позже». Доля таких значений велика, но мы не можем их вычислить арифметически. После перехода в тип `float64` эти данные превратятся в пропуски.

### 2.2.1 Корректировка типа данных

На стадии знакомства с данными было установлено, что для оптимизации возможно применение целочисленных типов с уменьшенной разрядностью (столбец `critic_score`), преобразование вещественных данных столбца `year_of_release` в целочисленный тип `int16`, преобразование строковых данных о продажах и оценках пользователей в `float64`.


```python
# Преобразовываем столбец с годом выпуска игры в тип int16
games['year_of_release'] = games['year_of_release'].astype('Int16')
```


```python
# Преобразовываем тип данных столбца critic_score из вещественного числа в целочисленный с пониженной разрядностью
games['critic_score'] = games['critic_score'].astype('Int16')
```


```python
# Преобразовываем тип данных столбцов `eu_sales`, `jp_sales`, `user_score` в float64
for column in ['eu_sales', 'jp_sales', 'user_score']:
    games[column] = pd.to_numeric(games[column], errors="coerce")
```


```python
# Проверяем результат преобразования
games[['year_of_release', 'critic_score', 'eu_sales', 'jp_sales', 'user_score']].dtypes
```




    year_of_release      Int16
    critic_score         Int16
    eu_sales           float64
    jp_sales           float64
    user_score         float64
    dtype: object



Для оптимизации работы с данными в датафрейме были сделаны такие изменения типов данных:
- `year_of_release`: тип данных изменён с `float64` на `int16`.
- `critic_score`: тип данных изменён с `float64` на `int16`.
- `eu_sales`, `jp_sales` и `user_score`: тип данных изменён с `object` на `float64`.


```python
games.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Wii Sports</td>
      <td>Wii</td>
      <td>2006</td>
      <td>Sports</td>
      <td>41.36</td>
      <td>28.96</td>
      <td>3.77</td>
      <td>8.45</td>
      <td>76</td>
      <td>8.0</td>
      <td>E</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Super Mario Bros.</td>
      <td>NES</td>
      <td>1985</td>
      <td>Platform</td>
      <td>29.08</td>
      <td>3.58</td>
      <td>6.81</td>
      <td>0.77</td>
      <td>&lt;NA&gt;</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mario Kart Wii</td>
      <td>Wii</td>
      <td>2008</td>
      <td>Racing</td>
      <td>15.68</td>
      <td>12.76</td>
      <td>3.79</td>
      <td>3.29</td>
      <td>82</td>
      <td>8.3</td>
      <td>E</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Wii Sports Resort</td>
      <td>Wii</td>
      <td>2009</td>
      <td>Sports</td>
      <td>15.61</td>
      <td>10.93</td>
      <td>3.28</td>
      <td>2.95</td>
      <td>80</td>
      <td>8.0</td>
      <td>E</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Pokemon Red/Pokemon Blue</td>
      <td>GB</td>
      <td>1996</td>
      <td>Role-Playing</td>
      <td>11.27</td>
      <td>8.89</td>
      <td>10.22</td>
      <td>1.00</td>
      <td>&lt;NA&gt;</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



### 2.3 Наличие пропусков в данных

В этом разделе посмотрим на количество пропущенных значений по столбцам датафрейма в асболютных и относительных значениях.


```python
# Выводим количество пропущенных строк в датафрейме
games.isna().sum()
```




    name                  2
    platform              0
    year_of_release     275
    genre                 2
    na_sales              0
    eu_sales              6
    jp_sales              4
    other_sales           0
    critic_score       8714
    user_score         9268
    rating             6871
    dtype: int64




```python
# Подсчитываем процент строк с пропусками
games.isna().sum() / games.shape[0] * 100
#pd.DataFrame(round(games.isna().mean()*100,)).style.background_gradient('coolwarm')
```




    name                0.011795
    platform            0.000000
    year_of_release     1.621845
    genre               0.011795
    na_sales            0.000000
    eu_sales            0.035386
    jp_sales            0.023590
    other_sales         0.000000
    critic_score       51.391838
    user_score         54.659118
    rating             40.522529
    dtype: float64



В данных наблюдаются пропуски в следующих столбцах:
- `name`: в 2 строках (0.01% данных) отсутствует информация о названии игры. Это может затруднить идентификацию, если необходимо провести анализ по конкретным играм. Но доля пропусков не высокая и можно их не принимать во внимание.
- `year_of_release`: в 275 строках (1.62% данных) отсутствует информация о годе выпуска игры. Пропуски в этом столбце могут повлиять на анализ по временным признакам. Так как основная задача провести анализ среди игр, выпущенных за определенный временной период, а заменить эти пропуски статистическими данными мы не можем, и их доля не превышает 2%, то просто удалим эти строки из датасета.
- `genre` в 2 строках (0.01%) отсутствует информация о жанре игры. Возможно, это те же самые строки, где пропущены названия игр. Проверим это ниже в коде и удалим при необходимости.
- `eu_sales`: в 6 строках (0.035%) отсутствует информация о продажах, их можно заменить на среднее значение в зависимости от названия платформы и года выхода игры.
- `jp_sales`: в 4 строках (0.024%) отсутствует информация о продажах, их можно заменить на среднее значение в зависимости от названия платформы и года выхода игры.
- `critic_score`: в 8714 строках (51.39% данных) отсутствует информация об оценке критиков. Пропуски в этом столбце могут повлиять на фильтр по рейтингу игр. Автоматическая замена этих пропусков усредненными значениями может исказить выводы. Заменим пропуски метками "-1".
- `user_score`: в 9268 строках (54.66% данных) отсутствует информация об оценке пользователей. Автоматическая замена этих пропусков усредненными значениями может исказить выводы. Заменим пропуски метками "-1".
- `rating`: в 6871 строках (40.52% данных) отсутствует информация о рейтинге ESRB. Это ухудшает работу рекоммендательных систем. Заменим пропуски метками "-1".
- Проверим также "нулевые" продажи по всем трем регионам.


```python
# Проверяем пропуски в строках `name` и `genre`
games[games['name'].isna()]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>661</th>
      <td>NaN</td>
      <td>GEN</td>
      <td>1993</td>
      <td>NaN</td>
      <td>1.78</td>
      <td>0.53</td>
      <td>0.00</td>
      <td>0.08</td>
      <td>&lt;NA&gt;</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14439</th>
      <td>NaN</td>
      <td>GEN</td>
      <td>1993</td>
      <td>NaN</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.03</td>
      <td>0.00</td>
      <td>&lt;NA&gt;</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Удаляем две строки по индексам:
games = games.drop(index=[661, 14439])
```


```python
# заполняем пропуски средними значениями в столбце 'eu_sales'
group_eu = games.groupby(['platform', 'year_of_release'])['eu_sales'].transform('mean')
games['eu_sales'] = games['eu_sales'].fillna(group_eu)
```


```python
# заполняем пропуски средними значениями в столбце 'jp_sales'
group_jp = games.groupby(['platform', 'year_of_release'])['jp_sales'].transform('mean')
games['jp_sales'] = games['jp_sales'].fillna(group_jp)
```


```python
# Проверяем наличие строк с пустыми продажами
games[
    (games["na_sales"] == 0)
    & (games["eu_sales"] == 0)
    & (games["jp_sales"] == 0)
    & (games["other_sales"] == 0)
]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>16916</th>
      <td>G1 Jockey 4 2008</td>
      <td>PS3</td>
      <td>2008</td>
      <td>Sports</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>&lt;NA&gt;</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16950</th>
      <td>SCORE International Baja 1000: The Official Game</td>
      <td>PS2</td>
      <td>2008</td>
      <td>Racing</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>&lt;NA&gt;</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Удаляем две строки без продаж
games = games.drop(index=[16916, 16950])
```


```python
# Заменяем пропуски в оценках игры на метку "-1"
games[['critic_score', 'user_score', 'rating']] = (games[['critic_score', 'user_score', 'rating']].fillna(-1))
```


```python
# Удаляем строки, где пропущен год выпуска игры
games = games.dropna(subset=["year_of_release"])
```


```python
# Еще раз проверяем пропуски
games.isna().sum()
```




    name               0
    platform           0
    year_of_release    0
    genre              0
    na_sales           0
    eu_sales           0
    jp_sales           0
    other_sales        0
    critic_score       0
    user_score         0
    rating             0
    dtype: int64



### 2.4 Явные и неявные дубликаты в данных
В этом разделе изучим уникальные значения в категориальных данных, например с названиями жанра игры, платформы, рейтинга и года выпуска. 
Проверим, встречаются ли среди данных неявные дубликаты, связанные с опечатками или разным способом написания.

Приведем к нижнему регистру значения в столбцах `name` и `genre`, чтобы найти возможные неявные дубликаты в разности написания.
Явные дубликаты будем искать по совпадению значений в столбцах `name` и `platform`.


```python
# Приводим значения к нижнему регистру в столбце `name`
games['name'] = games['name'].str.lower()
```


```python
# Приводим значения к нижнему регистру в столбце `genre`
games['genre'] = games['genre'].str.lower()
```


```python
# Выводим уникальные значения столбца 'platform'
games['platform'].unique()
```




    array(['Wii', 'NES', 'GB', 'DS', 'X360', 'PS3', 'PS2', 'SNES', 'GBA',
           'PS4', '3DS', 'N64', 'PS', 'XB', 'PC', '2600', 'PSP', 'XOne',
           'WiiU', 'GC', 'GEN', 'DC', 'PSV', 'SAT', 'SCD', 'WS', 'NG', 'TG16',
           '3DO', 'GG', 'PCFX'], dtype=object)




```python
# Выводим уникальные значения столбца 'year_of_release'
games['year_of_release'].unique()
```




    <IntegerArray>
    [2006, 1985, 2008, 2009, 1996, 1989, 1984, 2005, 1999, 2007, 2010, 2013, 2004,
     1990, 1988, 2002, 2001, 2011, 1998, 2015, 2012, 2014, 1992, 1997, 1993, 1994,
     1982, 2016, 2003, 1986, 2000, 1995, 1991, 1981, 1987, 1980, 1983]
    Length: 37, dtype: Int16




```python
# Выводим уникальные значения столбца 'genre'
games['genre'].unique()
```




    array(['sports', 'platform', 'racing', 'role-playing', 'puzzle', 'misc',
           'shooter', 'simulation', 'action', 'fighting', 'adventure',
           'strategy'], dtype=object)




```python
# Выводим уникальные значения столбца 'rating'
games['rating'].unique()
```




    array(['E', -1, 'M', 'T', 'E10+', 'K-A', 'AO', 'EC', 'RP'], dtype=object)




```python
# Выводим уникальные значения столбца 'user_score'
games['user_score'].unique()
```




    array([ 8. , -1. ,  8.3,  8.5,  6.6,  8.4,  8.6,  7.7,  6.3,  7.4,  8.2,
            9. ,  7.9,  8.1,  8.7,  7.1,  3.4,  5.3,  4.8,  3.2,  8.9,  6.4,
            7.8,  7.5,  2.6,  7.2,  9.2,  7. ,  7.3,  4.3,  7.6,  5.7,  5. ,
            9.1,  6.5,  8.8,  6.9,  9.4,  6.8,  6.1,  6.7,  5.4,  4. ,  4.9,
            4.5,  9.3,  6.2,  4.2,  6. ,  3.7,  4.1,  5.8,  5.6,  5.5,  4.4,
            4.6,  5.9,  3.9,  3.1,  2.9,  5.2,  3.3,  4.7,  5.1,  3.5,  2.5,
            1.9,  3. ,  2.7,  2.2,  2. ,  9.5,  2.1,  3.6,  2.8,  1.8,  3.8,
            0. ,  1.6,  9.6,  2.4,  1.7,  1.1,  0.3,  1.5,  0.7,  1.2,  2.3,
            0.5,  1.3,  0.2,  0.6,  1.4,  0.9,  1. ,  9.7])




```python
# Ищем дубликаты по двум столбцам 'name' и 'platform'
dubl=games[games.duplicated(subset=['name','platform'], keep=False)]
dubl
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>267</th>
      <td>batman: arkham asylum</td>
      <td>PS3</td>
      <td>2009</td>
      <td>action</td>
      <td>2.24</td>
      <td>1.31</td>
      <td>0.07</td>
      <td>0.61</td>
      <td>91</td>
      <td>8.9</td>
      <td>T</td>
    </tr>
    <tr>
      <th>268</th>
      <td>batman: arkham asylum</td>
      <td>PS3</td>
      <td>2009</td>
      <td>action</td>
      <td>2.24</td>
      <td>1.31</td>
      <td>0.07</td>
      <td>0.61</td>
      <td>91</td>
      <td>8.9</td>
      <td>T</td>
    </tr>
    <tr>
      <th>367</th>
      <td>james bond 007: agent under fire</td>
      <td>PS2</td>
      <td>2001</td>
      <td>shooter</td>
      <td>1.90</td>
      <td>1.13</td>
      <td>0.10</td>
      <td>0.41</td>
      <td>72</td>
      <td>7.9</td>
      <td>T</td>
    </tr>
    <tr>
      <th>368</th>
      <td>james bond 007: agent under fire</td>
      <td>PS2</td>
      <td>2001</td>
      <td>shooter</td>
      <td>1.90</td>
      <td>1.13</td>
      <td>0.10</td>
      <td>0.41</td>
      <td>72</td>
      <td>7.9</td>
      <td>T</td>
    </tr>
    <tr>
      <th>606</th>
      <td>madden nfl 13</td>
      <td>PS3</td>
      <td>2012</td>
      <td>sports</td>
      <td>2.11</td>
      <td>0.22</td>
      <td>0.00</td>
      <td>0.23</td>
      <td>83</td>
      <td>5.5</td>
      <td>E</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>16799</th>
      <td>transformers: prime</td>
      <td>Wii</td>
      <td>2012</td>
      <td>action</td>
      <td>0.00</td>
      <td>0.01</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>-1</td>
      <td>-1.0</td>
      <td>-1</td>
    </tr>
    <tr>
      <th>16911</th>
      <td>metal gear solid v: the definitive experience</td>
      <td>XOne</td>
      <td>2016</td>
      <td>action</td>
      <td>0.01</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>-1</td>
      <td>-1.0</td>
      <td>M</td>
    </tr>
    <tr>
      <th>16912</th>
      <td>metal gear solid v: the definitive experience</td>
      <td>XOne</td>
      <td>2016</td>
      <td>action</td>
      <td>0.01</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>-1</td>
      <td>-1.0</td>
      <td>M</td>
    </tr>
    <tr>
      <th>16939</th>
      <td>the longest 5 minutes</td>
      <td>PSV</td>
      <td>2016</td>
      <td>action</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.01</td>
      <td>0.00</td>
      <td>-1</td>
      <td>-1.0</td>
      <td>-1</td>
    </tr>
    <tr>
      <th>16940</th>
      <td>the longest 5 minutes</td>
      <td>PSV</td>
      <td>2016</td>
      <td>action</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.01</td>
      <td>0.00</td>
      <td>-1</td>
      <td>-1.0</td>
      <td>-1</td>
    </tr>
  </tbody>
</table>
<p>476 rows × 11 columns</p>
</div>



- Столбцы `name` и `genre` приведены к нижнему регистру для устранения возможных опечаток.
- В категориальных столбцах `platform`, `year_of_release`, `genre` дубликаты не обнаружены.
- В столбце `rating` не найдены дубликаты, но есть значение, которое не было указано в описании к задаче: `K-A`. Это обозначение использовалось раньше, но в современном классификаторе ESRB оно переименовано в `Е`. Нет смысла корректировать эти значения, потому что они верные в соотстветствии с годом выпуска игры, но нужно помнить про этот нюанс при анализе.
- Найдены 476 дублирующих строк по значению столбоц `name` и `platform`, для чистого датасета все дубли нужно удалить.


```python
# Удаляем дубликаты по двум столбцам `name` и `platform`, оставляя только первый из повтора
games = games.drop_duplicates(subset=['name', 'platform'], keep="first")
```

Проверяем получившиеся уникальные значения в столбцах `genre` и `rating`:


```python
games.isna().sum()
```




    name               0
    platform           0
    year_of_release    0
    genre              0
    na_sales           0
    eu_sales           0
    jp_sales           0
    other_sales        0
    critic_score       0
    user_score         0
    rating             0
    dtype: int64




```python
# Количество удаленных строк
count_rows_after = games.shape[0]
print(f"Было строк: {count_rows_before}")
print(f"Стало строк: {count_rows_after}")
print(f"Удалено строк: {count_rows_before - count_rows_after}")
```

    Было строк: 16956
    Стало строк: 16439
    Удалено строк: 517


## 3. Фильтрация данных
По заданию нас интересует период с 2000 по 2013 год включительно. Отберем данные по этому показателю. Сохраним новый срез данных в отдельном датафрейме `df_actual`.


```python
# Фильтруем датасет по годам от 2000 до 2013 включительно
df_actual = games.loc[games["year_of_release"].between(2000, 2013)].copy()
df_actual.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>wii sports</td>
      <td>Wii</td>
      <td>2006</td>
      <td>sports</td>
      <td>41.36</td>
      <td>28.96</td>
      <td>3.77</td>
      <td>8.45</td>
      <td>76</td>
      <td>8.0</td>
      <td>E</td>
    </tr>
    <tr>
      <th>2</th>
      <td>mario kart wii</td>
      <td>Wii</td>
      <td>2008</td>
      <td>racing</td>
      <td>15.68</td>
      <td>12.76</td>
      <td>3.79</td>
      <td>3.29</td>
      <td>82</td>
      <td>8.3</td>
      <td>E</td>
    </tr>
    <tr>
      <th>3</th>
      <td>wii sports resort</td>
      <td>Wii</td>
      <td>2009</td>
      <td>sports</td>
      <td>15.61</td>
      <td>10.93</td>
      <td>3.28</td>
      <td>2.95</td>
      <td>80</td>
      <td>8.0</td>
      <td>E</td>
    </tr>
    <tr>
      <th>6</th>
      <td>new super mario bros.</td>
      <td>DS</td>
      <td>2006</td>
      <td>platform</td>
      <td>11.28</td>
      <td>9.14</td>
      <td>6.50</td>
      <td>2.88</td>
      <td>89</td>
      <td>8.5</td>
      <td>E</td>
    </tr>
    <tr>
      <th>7</th>
      <td>wii play</td>
      <td>Wii</td>
      <td>2006</td>
      <td>misc</td>
      <td>13.96</td>
      <td>9.18</td>
      <td>2.93</td>
      <td>2.84</td>
      <td>58</td>
      <td>6.6</td>
      <td>E</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_actual.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Index: 12776 entries, 0 to 16954
    Data columns (total 11 columns):
     #   Column           Non-Null Count  Dtype  
    ---  ------           --------------  -----  
     0   name             12776 non-null  object 
     1   platform         12776 non-null  object 
     2   year_of_release  12776 non-null  Int16  
     3   genre            12776 non-null  object 
     4   na_sales         12776 non-null  float64
     5   eu_sales         12776 non-null  float64
     6   jp_sales         12776 non-null  float64
     7   other_sales      12776 non-null  float64
     8   critic_score     12776 non-null  Int16  
     9   user_score       12776 non-null  float64
     10  rating           12776 non-null  object 
    dtypes: Int16(2), float64(5), object(4)
    memory usage: 1.0+ MB


Мы отфильтровали новый датасет df_actual, который содержит 12776 строк и 11 столбцов.

## 4. Категоризация данных
Разделим все игры по оценкам пользователей и выделим такие категории: 
- высокая оценка (от 8 до 10 включительно),
- средняя оценка (от 3 до 8, не включая правую границу интервала),
- низкая оценка (от 0 до 3, не включая правую границу интервала).


```python
# Категоризируем все игры по оценкам пользователей:
#- высокая оценка (от 8 до 10 включительно)
#- средняя оценка (от 3 до 8, не включая правую границу интервала)
#- низкая оценка (от 0 до 3, не включая правую границу интервала)

df_actual['user_grades'] = pd.cut(df_actual['user_score'], bins=[-1, 0, 3, 8, 10.1],
                                  labels=['Нет оценки', 'Низкая оценка', 'Средняя оценка', 'Высокая оценка'], right=False)
df_actual.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
      <th>user_grades</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>wii sports</td>
      <td>Wii</td>
      <td>2006</td>
      <td>sports</td>
      <td>41.36</td>
      <td>28.96</td>
      <td>3.77</td>
      <td>8.45</td>
      <td>76</td>
      <td>8.0</td>
      <td>E</td>
      <td>Высокая оценка</td>
    </tr>
    <tr>
      <th>2</th>
      <td>mario kart wii</td>
      <td>Wii</td>
      <td>2008</td>
      <td>racing</td>
      <td>15.68</td>
      <td>12.76</td>
      <td>3.79</td>
      <td>3.29</td>
      <td>82</td>
      <td>8.3</td>
      <td>E</td>
      <td>Высокая оценка</td>
    </tr>
    <tr>
      <th>3</th>
      <td>wii sports resort</td>
      <td>Wii</td>
      <td>2009</td>
      <td>sports</td>
      <td>15.61</td>
      <td>10.93</td>
      <td>3.28</td>
      <td>2.95</td>
      <td>80</td>
      <td>8.0</td>
      <td>E</td>
      <td>Высокая оценка</td>
    </tr>
    <tr>
      <th>6</th>
      <td>new super mario bros.</td>
      <td>DS</td>
      <td>2006</td>
      <td>platform</td>
      <td>11.28</td>
      <td>9.14</td>
      <td>6.50</td>
      <td>2.88</td>
      <td>89</td>
      <td>8.5</td>
      <td>E</td>
      <td>Высокая оценка</td>
    </tr>
    <tr>
      <th>7</th>
      <td>wii play</td>
      <td>Wii</td>
      <td>2006</td>
      <td>misc</td>
      <td>13.96</td>
      <td>9.18</td>
      <td>2.93</td>
      <td>2.84</td>
      <td>58</td>
      <td>6.6</td>
      <td>E</td>
      <td>Средняя оценка</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Суммируем количества по категориям
df_actual.groupby('user_grades', observed=False)['name'].count().sort_values(ascending=False)
```




    user_grades
    Нет оценки        6296
    Средняя оценка    4080
    Высокая оценка    2284
    Низкая оценка      116
    Name: name, dtype: int64




```python
# В качестве проверки сравниваем сумму категориальных данных и общее количество строк в df_actual
df_actual.groupby('user_grades', observed=False)['name'].count().sum() == df_actual.shape[0]
```




    np.True_




```python
# Категоризируем все игры по оценкам критиков:
#- высокая оценка (от 80 до 100 включительно)
#- средняя оценка (от 30 до 80, не включая правую границу интервала)
#- низкая оценка (от 0 до 30, не включая правую границу интервала)

df_actual['critic_grades'] = pd.cut(df_actual['critic_score'], bins=[-1, 0, 30, 80, 101],
                                  labels=['Нет оценки', 'Низкая оценка', 'Средняя оценка', 'Высокая оценка'], right=False)
df_actual.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>platform</th>
      <th>year_of_release</th>
      <th>genre</th>
      <th>na_sales</th>
      <th>eu_sales</th>
      <th>jp_sales</th>
      <th>other_sales</th>
      <th>critic_score</th>
      <th>user_score</th>
      <th>rating</th>
      <th>user_grades</th>
      <th>critic_grades</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>wii sports</td>
      <td>Wii</td>
      <td>2006</td>
      <td>sports</td>
      <td>41.36</td>
      <td>28.96</td>
      <td>3.77</td>
      <td>8.45</td>
      <td>76</td>
      <td>8.0</td>
      <td>E</td>
      <td>Высокая оценка</td>
      <td>Средняя оценка</td>
    </tr>
    <tr>
      <th>2</th>
      <td>mario kart wii</td>
      <td>Wii</td>
      <td>2008</td>
      <td>racing</td>
      <td>15.68</td>
      <td>12.76</td>
      <td>3.79</td>
      <td>3.29</td>
      <td>82</td>
      <td>8.3</td>
      <td>E</td>
      <td>Высокая оценка</td>
      <td>Высокая оценка</td>
    </tr>
    <tr>
      <th>3</th>
      <td>wii sports resort</td>
      <td>Wii</td>
      <td>2009</td>
      <td>sports</td>
      <td>15.61</td>
      <td>10.93</td>
      <td>3.28</td>
      <td>2.95</td>
      <td>80</td>
      <td>8.0</td>
      <td>E</td>
      <td>Высокая оценка</td>
      <td>Высокая оценка</td>
    </tr>
    <tr>
      <th>6</th>
      <td>new super mario bros.</td>
      <td>DS</td>
      <td>2006</td>
      <td>platform</td>
      <td>11.28</td>
      <td>9.14</td>
      <td>6.50</td>
      <td>2.88</td>
      <td>89</td>
      <td>8.5</td>
      <td>E</td>
      <td>Высокая оценка</td>
      <td>Высокая оценка</td>
    </tr>
    <tr>
      <th>7</th>
      <td>wii play</td>
      <td>Wii</td>
      <td>2006</td>
      <td>misc</td>
      <td>13.96</td>
      <td>9.18</td>
      <td>2.93</td>
      <td>2.84</td>
      <td>58</td>
      <td>6.6</td>
      <td>E</td>
      <td>Средняя оценка</td>
      <td>Средняя оценка</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Суммируем количества по категориям
df_actual.groupby('critic_grades', observed=False)['name'].count().sort_values(ascending=False)
```




    critic_grades
    Нет оценки        5610
    Средняя оценка    5422
    Высокая оценка    1689
    Низкая оценка       55
    Name: name, dtype: int64




```python
# В качестве проверки сравниваем сумму категориальных данных и общее количество строк в df_actual
df_actual.groupby('user_grades', observed=False)['name'].count().sum() == df_actual.shape[0]
```




    np.True_




```python
# Выведем Топ-7 платформ по количеству игр, выпущенных за весь актуальный период
df_actual["platform"].value_counts().head(7)
```




    platform
    PS2     2126
    DS      2120
    Wii     1275
    PSP     1180
    X360    1120
    PS3     1085
    GBA      811
    Name: count, dtype: int64



## 5. Итоговый вывод

Были загружены данные `new_games.csv`. Они содержат 11 столбцов и 16956 строк, в которых представлена информация о продажах игр разных жанров и платформ.
При первичном знакомстве с данными и их предобработке получили такие результаты:
Названия столбцов были скорректированы под стандарт стиля snake case. У пяти столбцов данных были изменены типы данных:
- `year_of_release`: тип данных изменён с `float64` на `int16`.
- `critic_score`: тип данных изменён с `float64` на `int16`.
- `eu_sales`, `jp_sales` и `user_score`: тип данных изменён с `object` на `float64`
Отображение года реализации игры скорректировано после изменения типа данных.
Больше всего пропусков данных было найдено в трех столбцах:
`user_score` - 54%
` critic_score` - 51%
` rating` - 40%
Пропуски заполнили метками.
Найдено и устранено 476 строк дубликатов названий игр.
Всего очищено и удалено 517 строк.

Для детального анализа по заданным требованиям с фильтром по годам с 2000 по 2013 год был создан новый датафрейм `df_actual` на 12776 строк.

Проведены две категоризации данных по оценкам пользователей и оценкам критиков. Для этого в датасет добавлены два новых столбца `user_grades` и `critic_grades`. 
По итогам анализа сделаны следующие выводы:

32% пользователей поставили среднюю оценку играм, вышедшим за заданный период времени.\
18% пользователей поставили высокую оценку играм, вышедшим за заданный период времени.\
При этом 49% игр остались без оценок пользователей.

42% критиков поставили среднюю оценку играм, вышедшим за заданный период времени.\
13% критиков поставили высокую оценку играм, вышедшим за заданный период времени.\
При этом 44% игр остались без оценок критиков.

Платформа, на которой вышло больше всего игр за заданный период - PS2 (2126), почти столько же на платформе DS (2120)


```python

```

## Теория

pandas - библиотека для обработки и анализа данных
data frames - df
series - колонки в data frames


## Операции

| Пример | Описание |
|:-----------|:--|
|`pd.concat([df, df2], axis=1)`|соединение двух дата фреймов|
|`df.drop(['Language'], axis=1)`|удаление столбца без сохранения результата|
|`df.drop(['Language'], axis=1, inplace=True)`|удаление столбца|
|`df.drop([2, 3], inplace=True)`|удаление строк|


## Методы dataframe

| Пример | Описание |
|:-----------|:--|
|`pd.date_range(start='2023-03-01', end='2023-08-01')`|создаст рендж дат|
|`df = pd.DataFrame(forum_users)`|создаст дата фрейм из словаря|
|`df[['Username', 'Age']]`|создаст дата фрейм из указанных полей|
|`df.shape`|покажет размерность дата фрейма|
|`df.columns.tolist()`|выведет список названий столбцов|
|`df.index.tolist()`|выведет список названий рядов|
|`df.dtypes`|выведет тип данных в колонках|
|`df.values`|выведет двумерный массив numpy|
|`df.values[1, 1]`|выведет элемент 1:1|
|`df.head(n)`|выведет первые n рядов|
|`df.tail(n)`|выводит последние n рядов|
|`df.describe()`|выводит статистические данные (count - количество значений, mean - среднее значение, std - стандартное отклонение, min - минимальное значение, max - максимальное значение, 25-75% процентиль)|
|`df.select_dtypes(include='object')`|выводит колонки с определенным типом данных|
|`df.isna()`|вернет список отсутствующих данных|
|`df.isna().sum()`|сумма неопределенных значений для каждой колонки|
|`pd.to_datetime(['2023-01-01', '2023-02-15'])`|преобразует данные в timestamp|
|`df[df['Age'] >= 25]`|фильтрация|
|`df[(df['Total Posts'] >= 300) & (df['Age'] >= 25)]`|фильтрация по нескольким условиям|
|`df['Language'] = 'English'`|создаст новую колонку или перезапишет данные существующей |
|`df['Active'] = np.array([True, False, False, True, True]) `|заполнит данные из списка или массива|


## Методы serial

| Пример | Описание |
|:-----------|:--|
|`username_series = df['Username']`|создает series объект по колонке из data frame|
|`username_series.values`|выводит ndarray значений из series объекта|
|`username_series.index`|выводит рендж индексов|
|`df['Reputation'].value_counts()`|выводит количество значений поля dataframe. Индекс - значение|
|`df['Reputation'].unique()`|вернет одномерный массив уникальных значений|
|`df['Username'].sort_values(ascending=False)`|отсортирует значения по убыванию|
|`df.loc[3]`|найти ряд по индексу|
|`df.loc[2:3 , ['Username', 'Joined Date', 'Reputation']]`|вывести определенные поля определенных рядов|
|`df.iloc[2:4, 1:4]`|выведет колонки и ряды по индексам исключая верхние|


## Фильтрация

| Пример | Описание |
|:-----------|:--|
|`df['Age'] >= 25`|вернет false or true по условию|
|`df[df['Reputation'].isin([200, 500])]`|проверяет вхождение значений по списку|
|`df[df['Joined Date'].isin(pd.date_range(start='2023-03-01', end='2023-08-01'))]`|фильтрация дат|


## Сортировка

| Пример | Описание |
|:-----------|:--|
|`df.sort_values(by='Joined Date', ascending=False)`|сортировка по полю|
|`df[['User ID', 'Total Posts', 'Reputation']].sort_values(by=2, axis=1, ascending=False)`|сортировка по ряду|
|`df.groupby('Product')['Qty'].sum()`|группировка c суммирование значений|
|`df.groupby('Product')['Price'].mean()`|группировка со средним значением|


## Выгрузка

| Пример | Описание |
|:-----------|:--|
|`df.to_csv('random_sales.csv', index=False)`|сохранить дата фрейм в cvs|
|`df = pd.read_csv('random_sales.csv', parse_dates=['Date'])`|создание дата фрейма из cvs|
|`df.to_excel('sales_data.xlsx', index=False)`|запись данных в excel|
|`df.to_json('sales_data.json')`|запись в excel|


___

Пример генерации данных
np.random.seed(1)
n = 100
dates = pd.date_range(start='2050-01-01', periods=n, freq='D')
products = np.random.choice(['Tablet', 'Camera', 'Phone'], n)
prices = np.round(np.random.uniform(50.0, 200.0, n), 2)
qty = np.random.randint(1, 15, n)

data = {
    'Date': dates,
    'Product': products,
    'Price': prices,
    'Qty': qty
}

df = pd.DataFrame(data)
df.dtypes

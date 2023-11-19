## ������

pandas - ���������� ��� ��������� � ������� ������
data frames - df
series - ������� � data frames


## ��������

| ������ | �������� |
|:-----------|:--|
|`pd.concat([df, df2], axis=1)`|���������� ���� ���� �������|
|`df.drop(['Language'], axis=1)`|�������� ������� ��� ���������� ����������|
|`df.drop(['Language'], axis=1, inplace=True)`|�������� �������|
|`df.drop([2, 3], inplace=True)`|�������� �����|


## ������ dataframe

| ������ | �������� |
|:-----------|:--|
|`pd.date_range(start='2023-03-01', end='2023-08-01')`|������� ����� ���|
|`df = pd.DataFrame(forum_users)`|������� ���� ����� �� �������|
|`df[['Username', 'Age']]`|������� ���� ����� �� ��������� �����|
|`df.shape`|������� ����������� ���� ������|
|`df.columns.tolist()`|������� ������ �������� ��������|
|`df.index.tolist()`|������� ������ �������� �����|
|`df.dtypes`|������� ��� ������ � ��������|
|`df.values`|������� ��������� ������ numpy|
|`df.values[1, 1]`|������� ������� 1:1|
|`df.head(n)`|������� ������ n �����|
|`df.tail(n)`|������� ��������� n �����|
|`df.describe()`|������� �������������� ������ (count - ���������� ��������, mean - ������� ��������, std - ����������� ����������, min - ����������� ��������, max - ������������ ��������, 25-75% ����������)|
|`df.select_dtypes(include='object')`|������� ������� � ������������ ����� ������|
|`df.isna()`|������ ������ ������������� ������|
|`df.isna().sum()`|����� �������������� �������� ��� ������ �������|
|`pd.to_datetime(['2023-01-01', '2023-02-15'])`|����������� ������ � timestamp|
|`df[df['Age'] >= 25]`|����������|
|`df[(df['Total Posts'] >= 300) & (df['Age'] >= 25)]`|���������� �� ���������� ��������|
|`df['Language'] = 'English'`|������� ����� ������� ��� ����������� ������ ������������ |
|`df['Active'] = np.array([True, False, False, True, True]) `|�������� ������ �� ������ ��� �������|


## ������ serial

| ������ | �������� |
|:-----------|:--|
|`username_series = df['Username']`|������� series ������ �� ������� �� data frame|
|`username_series.values`|������� ndarray �������� �� series �������|
|`username_series.index`|������� ����� ��������|
|`df['Reputation'].value_counts()`|������� ���������� �������� ���� dataframe. ������ - ��������|
|`df['Reputation'].unique()`|������ ���������� ������ ���������� ��������|
|`df['Username'].sort_values(ascending=False)`|����������� �������� �� ��������|
|`df.loc[3]`|����� ��� �� �������|
|`df.loc[2:3 , ['Username', 'Joined Date', 'Reputation']]`|������� ������������ ���� ������������ �����|
|`df.iloc[2:4, 1:4]`|������� ������� � ���� �� �������� �������� �������|


## ����������

| ������ | �������� |
|:-----------|:--|
|`df['Age'] >= 25`|������ false or true �� �������|
|`df[df['Reputation'].isin([200, 500])]`|��������� ��������� �������� �� ������|
|`df[df['Joined Date'].isin(pd.date_range(start='2023-03-01', end='2023-08-01'))]`|���������� ���|


## ����������

| ������ | �������� |
|:-----------|:--|
|`df.sort_values(by='Joined Date', ascending=False)`|���������� �� ����|
|`df[['User ID', 'Total Posts', 'Reputation']].sort_values(by=2, axis=1, ascending=False)`|���������� �� ����|
|`df.groupby('Product')['Qty'].sum()`|����������� c ������������ ��������|
|`df.groupby('Product')['Price'].mean()`|����������� �� ������� ���������|


## ��������

| ������ | �������� |
|:-----------|:--|
|`df.to_csv('random_sales.csv', index=False)`|��������� ���� ����� � cvs|
|`df = pd.read_csv('random_sales.csv', parse_dates=['Date'])`|�������� ���� ������ �� cvs|
|`df.to_excel('sales_data.xlsx', index=False)`|������ ������ � excel|
|`df.to_json('sales_data.json')`|������ � excel|


___

������ ��������� ������
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


<img src="http://imgur.com/1ZcRyrc.png" style="float: left; margin: 20px; height: 55px">

# Project 2: Analyzing Chipotle Data

_Author: Joseph Nelson (DC)_

---

https://github.com/TheUpshot/chipotle

For project two, you will complete a series of exercises exploring [order data from Chipotle](https://github.com/TheUpshot/chipotle), compliments of The New York Times's "The Upshot."

For these exercises, you are conducting basic exploratory data analysis (Pandas not required) to understand the basics of Chipotle's order data: how many orders are being made, what is the average price per order, how many different ingredients, etc. These allow you to conduct business analyst skills while becoming comfortable with Python.

---

## Basic Level

### Part 1: Read in the file with `csv.reader()` and store it in an object called `file_nested_list`.

Hint: This is a TSV (tab-separated value) file, and `csv.reader()` needs to be told how to handle it.
      https://docs.python.org/2/library/csv.html


```python
#Reading TSV file using csv reader
import csv
with open('chipotle.tsv') as file:
    reader = csv.reader(file, delimiter='\t')
    file_nested_list = [row for row in reader]
file_nested_list[1:5]  
```




    [['1', '1', 'Chips and Fresh Tomato Salsa', 'NULL', '$2.39 '],
     ['1', '1', 'Izze', '[Clementine]', '$3.39 '],
     ['1', '1', 'Nantucket Nectar', '[Apple]', '$3.39 '],
     ['1', '1', 'Chips and Tomatillo-Green Chili Salsa', 'NULL', '$2.39 ']]




```python
#Reading TSV file using pandas
import pandas as pd
file_nested_list_pd = pd.read_table('chipotle.tsv', sep='\t')
file_nested_list_pd.head(5)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>order_id</th>
      <th>quantity</th>
      <th>item_name</th>
      <th>choice_description</th>
      <th>item_price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1</td>
      <td>Chips and Fresh Tomato Salsa</td>
      <td>NaN</td>
      <td>$2.39</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>Izze</td>
      <td>[Clementine]</td>
      <td>$3.39</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>1</td>
      <td>Nantucket Nectar</td>
      <td>[Apple]</td>
      <td>$3.39</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>1</td>
      <td>Chips and Tomatillo-Green Chili Salsa</td>
      <td>NaN</td>
      <td>$2.39</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>2</td>
      <td>Chicken Bowl</td>
      <td>[Tomatillo-Red Chili Salsa (Hot), [Black Beans...</td>
      <td>$16.98</td>
    </tr>
  </tbody>
</table>
</div>



### Part 2: Separate `file_nested_list` into the `header` and the `data`.



```python
#Header using csv object
header = file_nested_list[0]
print(header)
```

    ['order_id', 'quantity', 'item_name', 'choice_description', 'item_price']



```python
#Data using csv object
data = file_nested_list[1:]
print(data[1:5]) # printing sample data for five records
```

    [['1', '1', 'Izze', '[Clementine]', '$3.39 '], ['1', '1', 'Nantucket Nectar', '[Apple]', '$3.39 '], ['1', '1', 'Chips and Tomatillo-Green Chili Salsa', 'NULL', '$2.39 '], ['2', '2', 'Chicken Bowl', '[Tomatillo-Red Chili Salsa (Hot), [Black Beans, Rice, Cheese, Sour Cream]]', '$16.98 ']]



```python
#Header using pandas object
head_pd = file_nested_list_pd.columns.tolist()
head_pd
```




    ['order_id', 'quantity', 'item_name', 'choice_description', 'item_price']



---

## Intermediate Level

### Part 3: Calculate the average price of an order.

Hint: Examine the data to see if the `quantity` column is relevant to this calculation.

Hint: Think carefully about the simplest way to do this!


```python
print(header)
print(data[1:5]) 
```

    ['order_id', 'quantity', 'item_name', 'choice_description', 'item_price']
    [['1', '1', 'Izze', '[Clementine]', '$3.39 '], ['1', '1', 'Nantucket Nectar', '[Apple]', '$3.39 '], ['1', '1', 'Chips and Tomatillo-Green Chili Salsa', 'NULL', '$2.39 '], ['2', '2', 'Chicken Bowl', '[Tomatillo-Red Chili Salsa (Hot), [Black Beans, Rice, Cheese, Sour Cream]]', '$16.98 ']]



```python
###   We want to find the average price of an order.
###   This means we need the sum of the price of all orders and the total number of orders

#calculating total price sum with the built-in sum function
sum_of_all_prices = sum([float(row[4][1:]) for row in data])#Used row[4] to pick Item price & used row[4][1:] to remove dollar sign
sum_of_all_prices
```




    34500.16000000046




```python
#Calculating the total number of orders
total_order_num = float(data[-1][0])# Used data[-1] to pick the last order id which inturn give the total number of orders
total_order_num
```




    1834.0




```python
#Calculating the average price
average_price = sum_of_all_prices/total_order_num 
average_price
```




    18.811428571428824



### Part 4: Create a list (or set) named `unique_sodas` containing all unique sodas and soft drinks that they sell.

Note: Just look for 'Canned Soda' and 'Canned Soft Drink', and ignore other drinks like 'Izze'.


```python

unique_sodas = set([row[3][1:-1] for row in data if 'Canned' in row[2]])
# Used Set to get unique values, Used if condition on item_name i.e row[2] to search Canned Soda,row[3] is used to get choice_description
unique_sodas
```




    {'Coca Cola',
     'Coke',
     'Diet Coke',
     'Diet Dr. Pepper',
     'Dr. Pepper',
     'Lemonade',
     'Mountain Dew',
     'Nestea',
     'Sprite'}



---

## Advanced Level


### Part 5: Calculate the average number of toppings per burrito.

Note: Let's ignore the `quantity` column to simplify this task.

Hint: Think carefully about the easiest way to count the number of toppings!



```python
###To calculate the average number of toppings, we need to divide the total number of burritos by the total number of toppings
#Calculating the total number of burritos
burrito_count = 0
topping_count = 0
# loop through the data, looking for lines containing 'Burrito'
for row in data:
    if 'Burrito' in row[2]:
        burrito_count = burrito_count + 1
        topping_count += (row[3].count(',') + 1)
        
print('burrito_count: ',burrito_count) 
print('topping_count: ',topping_count)
print('average_num_toppings: ',topping_count/burrito_count) 
```

    burrito_count:  1172
    topping_count:  6323
    average_num_toppings:  5.395051194539249



```python
#Using List comprehensions
burrito_count = sum(1 for row in data if 'Burrito' in row[2])
topping_count = sum([row[3].count(',') + 1 for row in data if 'Burrito' in row[2]])
round(topping_count / float(burrito_count), 2)
```




    5.4



### Part 6: Create a dictionary. Let the keys represent chip orders and the values represent the total number of orders.

Expected output: `{'Chips and Roasted Chili-Corn Salsa': 18, ... }`

Note: Please take the `quantity` column into account!

Optional: Learn how to use `defaultdict` to simplify your code.


```python
from collections import defaultdict
dic_chips = defaultdict(int)
for row in data:
    if 'Chips' in row[2]:
        dic_chips[row[2]] += int(row[1])

dict(dic_chips)
```




    {'Chips': 230,
     'Chips and Fresh Tomato Salsa': 130,
     'Chips and Guacamole': 506,
     'Chips and Mild Fresh Tomato Salsa': 1,
     'Chips and Roasted Chili Corn Salsa': 23,
     'Chips and Roasted Chili-Corn Salsa': 18,
     'Chips and Tomatillo Green Chili Salsa': 45,
     'Chips and Tomatillo Red Chili Salsa': 50,
     'Chips and Tomatillo-Green Chili Salsa': 33,
     'Chips and Tomatillo-Red Chili Salsa': 25,
     'Side of Chips': 110}



---

## Bonus: Craft a problem statement about this data that interests you, and then answer it!



```python
#Find the highest  and lowest priced item
High_item_price = max([float(row[4][1:]) for row in data])
Low_item_price = min([float(row[4][1:]) for row in data])
print('High_item_price: ',High_item_price)
print('Low_item_price: ' ,Low_item_price)
   
```

    High_item_price:  44.25
    Low_item_price:  1.09


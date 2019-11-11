
# Selecting Data - Lab


## Introduction 

NASA wants to go to Mars! Before they build their rocket, NASA needs to track information about all of the planets in the Solar System. In this lab, you'll practice querying the database with various `SELECT` statements. This will include selecting different columns and implementing other SQL clauses like `WHERE` to return the data desired.

<img src="./images/planets.png" width="600">

## Objectives
You will be able to:
* Connect to a SQL database using Python
* Retrieve all information from a SQL table
* Retrieve a subset of records from a table using a `WHERE` clause
* Write SQL queries to filter and order results
* Retrieve a subset of columns from a table

## Connecting to the DataBase

To get started import pandas and sqlite3. Then, connect to the database titled `planets.db`. 

Don't forget to instantiate a cursor so that you can later execute your queries.


```python
# connect database and create cursor
import sqlite3 
conn = sqlite3.connect('planets.db')
cur = conn.cursor()
```

## Selecting Data

Here's an overview of the planet's table you'll be querying.

|name   |color |num_of_moons|mass|rings|
|-------|-------|-------|-------|-------|
|Mercury|gray   |0      |0.55   |no     |
|Venus  |yellow |0      |0.82   |no     |
|Earth  |blue   |1      |1.00   |no     |
|Mars   |red    |2      |0.11   |no     |
|Jupiter|orange |67     |317.90 |no     |
|Saturn |hazel  |62     |95.19  |yes    |
|Uranus |light blue|27  |14.54  |yes    |
|Neptune|dark blue|14   |17.15  |yes    |

Write SQL queries for each of the statements below using the same pandas wrapping syntax from the previous lesson.

## Select just the name and color of each planet


```python
import pandas as pd
cur.execute("""SELECT name, color FROM planets;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [x[0] for x in cur.description]
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>color</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Mercury</td>
      <td>gray</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Venus</td>
      <td>yellow</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Earth</td>
      <td>blue</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Mars</td>
      <td>red</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Jupiter</td>
      <td>orange</td>
    </tr>
    <tr>
      <td>5</td>
      <td>Saturn</td>
      <td>hazel</td>
    </tr>
    <tr>
      <td>6</td>
      <td>Uranus</td>
      <td>light blue</td>
    </tr>
    <tr>
      <td>7</td>
      <td>Neptune</td>
      <td>dark blue</td>
    </tr>
  </tbody>
</table>
</div>



## Select all columns for each planet whose mass is greater than 1.00



```python
cur.execute("""SELECT * from planets WHERE mass > 1.00;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [x[0] for x in cur.description]
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>name</th>
      <th>color</th>
      <th>num_of_moons</th>
      <th>mass</th>
      <th>rings</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>5</td>
      <td>Jupiter</td>
      <td>orange</td>
      <td>68</td>
      <td>317.90</td>
      <td>0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>6</td>
      <td>Saturn</td>
      <td>hazel</td>
      <td>62</td>
      <td>95.19</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2</td>
      <td>7</td>
      <td>Uranus</td>
      <td>light blue</td>
      <td>27</td>
      <td>14.54</td>
      <td>1</td>
    </tr>
    <tr>
      <td>3</td>
      <td>8</td>
      <td>Neptune</td>
      <td>dark blue</td>
      <td>14</td>
      <td>17.15</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



## Select the name and mass of each planet whose mass is less than or equal to 1.00


```python
cur.execute("""SELECT * from planets WHERE mass <= 1.00;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [x[0] for x in cur.description]
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>name</th>
      <th>color</th>
      <th>num_of_moons</th>
      <th>mass</th>
      <th>rings</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1</td>
      <td>Mercury</td>
      <td>gray</td>
      <td>0</td>
      <td>0.55</td>
      <td>0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2</td>
      <td>Venus</td>
      <td>yellow</td>
      <td>0</td>
      <td>0.82</td>
      <td>0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>3</td>
      <td>Earth</td>
      <td>blue</td>
      <td>1</td>
      <td>1.00</td>
      <td>0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4</td>
      <td>Mars</td>
      <td>red</td>
      <td>2</td>
      <td>0.11</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



## Select the name and color of each planet that has more than 10 moons


```python
cur.execute("""SELECT name, color FROM planets WHERE num_of_moons > 10;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [x[0] for x in cur.description]
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>color</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Jupiter</td>
      <td>orange</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Saturn</td>
      <td>hazel</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Uranus</td>
      <td>light blue</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Neptune</td>
      <td>dark blue</td>
    </tr>
  </tbody>
</table>
</div>



## Select the planet that has at least one moon and a mass less than 1.00


```python
cur.execute("""SELECT * FROM planets WHERE num_of_moons > 0 and mass < 1.00;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [x[0] for x in cur.description]
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>name</th>
      <th>color</th>
      <th>num_of_moons</th>
      <th>mass</th>
      <th>rings</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>4</td>
      <td>Mars</td>
      <td>red</td>
      <td>2</td>
      <td>0.11</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



## Select the name and color of planets that have a color of blue, light blue, or dark blue


```python
cur.execute("""SELECT name, color FROM planets WHERE color = 'blue' OR color = 'light blue' OR color = 'dark blue';""")
df = pd.DataFrame(cur.fetchall())
df.columns = [x[0] for x in cur.description]
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>color</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Earth</td>
      <td>blue</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Uranus</td>
      <td>light blue</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Neptune</td>
      <td>dark blue</td>
    </tr>
  </tbody>
</table>
</div>



## Select the name, color, and number of moons for the 4 largest planets that don't have rings and order them from largest to smallest


```python
cur.execute("""SELECT name, color, num_of_moons FROM planets 
               WHERE rings = 0
               ORDER BY mass DESC
               LIMIT 4;""")
df = pd.DataFrame(cur.fetchall())
df.columns = [x[0] for x in cur.description]
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>color</th>
      <th>num_of_moons</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>Jupiter</td>
      <td>orange</td>
      <td>68</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Earth</td>
      <td>blue</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Venus</td>
      <td>yellow</td>
      <td>0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Mercury</td>
      <td>gray</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



## Summary

Congratulations! NASA is one step closer to embarking upon its mission to Mars. In this lab, You practiced writing `SELECT` statements that query a single table to get specific information. You also used other clauses and specified column names to cherry-pick the data we wanted to retrieve. 

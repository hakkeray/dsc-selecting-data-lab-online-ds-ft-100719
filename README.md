{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Selecting Data - Lab\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Introduction \n",
    "\n",
    "NASA wants to go to Mars! Before they build their rocket, NASA needs to track information about all of the planets in the Solar System. In this lab, you'll practice querying the database with various `SELECT` statements. This will include selecting different columns and implementing other SQL clauses like `WHERE` to return the data desired."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "<img src=\"./images/planets.png\" width=\"600\">"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Objectives\n",
    "You will be able to:\n",
    "* Connect to a SQL database using Python\n",
    "* Retrieve all information from a SQL table\n",
    "* Retrieve a subset of records from a table using a `WHERE` clause\n",
    "* Write SQL queries to filter and order results\n",
    "* Retrieve a subset of columns from a table"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Connecting to the DataBase\n",
    "\n",
    "To get started import pandas and sqlite3. Then, connect to the database titled `planets.db`. \n",
    "\n",
    "Don't forget to instantiate a cursor so that you can later execute your queries."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [],
   "source": [
    "# connect database and create cursor\n",
    "import sqlite3 \n",
    "conn = sqlite3.connect('planets.db')\n",
    "cur = conn.cursor()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Selecting Data"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Here's an overview of the planet's table you'll be querying.\n",
    "\n",
    "|name   |color |num_of_moons|mass|rings|\n",
    "|-------|-------|-------|-------|-------|\n",
    "|Mercury|gray   |0      |0.55   |no     |\n",
    "|Venus  |yellow |0      |0.82   |no     |\n",
    "|Earth  |blue   |1      |1.00   |no     |\n",
    "|Mars   |red    |2      |0.11   |no     |\n",
    "|Jupiter|orange |67     |317.90 |no     |\n",
    "|Saturn |hazel  |62     |95.19  |yes    |\n",
    "|Uranus |light blue|27  |14.54  |yes    |\n",
    "|Neptune|dark blue|14   |17.15  |yes    |"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Write SQL queries for each of the statements below using the same pandas wrapping syntax from the previous lesson."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Select just the name and color of each planet"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 20,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>name</th>\n",
       "      <th>color</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td>0</td>\n",
       "      <td>Mercury</td>\n",
       "      <td>gray</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>1</td>\n",
       "      <td>Venus</td>\n",
       "      <td>yellow</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2</td>\n",
       "      <td>Earth</td>\n",
       "      <td>blue</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>3</td>\n",
       "      <td>Mars</td>\n",
       "      <td>red</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>4</td>\n",
       "      <td>Jupiter</td>\n",
       "      <td>orange</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>5</td>\n",
       "      <td>Saturn</td>\n",
       "      <td>hazel</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>6</td>\n",
       "      <td>Uranus</td>\n",
       "      <td>light blue</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>7</td>\n",
       "      <td>Neptune</td>\n",
       "      <td>dark blue</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "      name       color\n",
       "0  Mercury        gray\n",
       "1    Venus      yellow\n",
       "2    Earth        blue\n",
       "3     Mars         red\n",
       "4  Jupiter      orange\n",
       "5   Saturn       hazel\n",
       "6   Uranus  light blue\n",
       "7  Neptune   dark blue"
      ]
     },
     "execution_count": 20,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "import pandas as pd\n",
    "cur.execute(\"\"\"SELECT name, color FROM planets;\"\"\")\n",
    "df = pd.DataFrame(cur.fetchall())\n",
    "df.columns = [x[0] for x in cur.description]\n",
    "df"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Select all columns for each planet whose mass is greater than 1.00\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>id</th>\n",
       "      <th>name</th>\n",
       "      <th>color</th>\n",
       "      <th>num_of_moons</th>\n",
       "      <th>mass</th>\n",
       "      <th>rings</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td>0</td>\n",
       "      <td>5</td>\n",
       "      <td>Jupiter</td>\n",
       "      <td>orange</td>\n",
       "      <td>68</td>\n",
       "      <td>317.90</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>1</td>\n",
       "      <td>6</td>\n",
       "      <td>Saturn</td>\n",
       "      <td>hazel</td>\n",
       "      <td>62</td>\n",
       "      <td>95.19</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2</td>\n",
       "      <td>7</td>\n",
       "      <td>Uranus</td>\n",
       "      <td>light blue</td>\n",
       "      <td>27</td>\n",
       "      <td>14.54</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>3</td>\n",
       "      <td>8</td>\n",
       "      <td>Neptune</td>\n",
       "      <td>dark blue</td>\n",
       "      <td>14</td>\n",
       "      <td>17.15</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   id     name       color  num_of_moons    mass  rings\n",
       "0   5  Jupiter      orange            68  317.90      0\n",
       "1   6   Saturn       hazel            62   95.19      1\n",
       "2   7   Uranus  light blue            27   14.54      1\n",
       "3   8  Neptune   dark blue            14   17.15      1"
      ]
     },
     "execution_count": 21,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "cur.execute(\"\"\"SELECT * from planets WHERE mass > 1.00;\"\"\")\n",
    "df = pd.DataFrame(cur.fetchall())\n",
    "df.columns = [x[0] for x in cur.description]\n",
    "df"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Select the name and mass of each planet whose mass is less than or equal to 1.00"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 22,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>id</th>\n",
       "      <th>name</th>\n",
       "      <th>color</th>\n",
       "      <th>num_of_moons</th>\n",
       "      <th>mass</th>\n",
       "      <th>rings</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td>0</td>\n",
       "      <td>1</td>\n",
       "      <td>Mercury</td>\n",
       "      <td>gray</td>\n",
       "      <td>0</td>\n",
       "      <td>0.55</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>1</td>\n",
       "      <td>2</td>\n",
       "      <td>Venus</td>\n",
       "      <td>yellow</td>\n",
       "      <td>0</td>\n",
       "      <td>0.82</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2</td>\n",
       "      <td>3</td>\n",
       "      <td>Earth</td>\n",
       "      <td>blue</td>\n",
       "      <td>1</td>\n",
       "      <td>1.00</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>3</td>\n",
       "      <td>4</td>\n",
       "      <td>Mars</td>\n",
       "      <td>red</td>\n",
       "      <td>2</td>\n",
       "      <td>0.11</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   id     name   color  num_of_moons  mass  rings\n",
       "0   1  Mercury    gray             0  0.55      0\n",
       "1   2    Venus  yellow             0  0.82      0\n",
       "2   3    Earth    blue             1  1.00      0\n",
       "3   4     Mars     red             2  0.11      0"
      ]
     },
     "execution_count": 22,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "cur.execute(\"\"\"SELECT * from planets WHERE mass <= 1.00;\"\"\")\n",
    "df = pd.DataFrame(cur.fetchall())\n",
    "df.columns = [x[0] for x in cur.description]\n",
    "df"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Select the name and color of each planet that has more than 10 moons"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 23,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>name</th>\n",
       "      <th>color</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td>0</td>\n",
       "      <td>Jupiter</td>\n",
       "      <td>orange</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>1</td>\n",
       "      <td>Saturn</td>\n",
       "      <td>hazel</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2</td>\n",
       "      <td>Uranus</td>\n",
       "      <td>light blue</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>3</td>\n",
       "      <td>Neptune</td>\n",
       "      <td>dark blue</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "      name       color\n",
       "0  Jupiter      orange\n",
       "1   Saturn       hazel\n",
       "2   Uranus  light blue\n",
       "3  Neptune   dark blue"
      ]
     },
     "execution_count": 23,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "cur.execute(\"\"\"SELECT name, color FROM planets WHERE num_of_moons > 10;\"\"\")\n",
    "df = pd.DataFrame(cur.fetchall())\n",
    "df.columns = [x[0] for x in cur.description]\n",
    "df"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Select the planet that has at least one moon and a mass less than 1.00"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 24,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>id</th>\n",
       "      <th>name</th>\n",
       "      <th>color</th>\n",
       "      <th>num_of_moons</th>\n",
       "      <th>mass</th>\n",
       "      <th>rings</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td>0</td>\n",
       "      <td>4</td>\n",
       "      <td>Mars</td>\n",
       "      <td>red</td>\n",
       "      <td>2</td>\n",
       "      <td>0.11</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "   id  name color  num_of_moons  mass  rings\n",
       "0   4  Mars   red             2  0.11      0"
      ]
     },
     "execution_count": 24,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "cur.execute(\"\"\"SELECT * FROM planets WHERE num_of_moons > 0 and mass < 1.00;\"\"\")\n",
    "df = pd.DataFrame(cur.fetchall())\n",
    "df.columns = [x[0] for x in cur.description]\n",
    "df"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Select the name and color of planets that have a color of blue, light blue, or dark blue"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 25,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>name</th>\n",
       "      <th>color</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td>0</td>\n",
       "      <td>Earth</td>\n",
       "      <td>blue</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>1</td>\n",
       "      <td>Uranus</td>\n",
       "      <td>light blue</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2</td>\n",
       "      <td>Neptune</td>\n",
       "      <td>dark blue</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "      name       color\n",
       "0    Earth        blue\n",
       "1   Uranus  light blue\n",
       "2  Neptune   dark blue"
      ]
     },
     "execution_count": 25,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "cur.execute(\"\"\"SELECT name, color FROM planets WHERE color = 'blue' OR color = 'light blue' OR color = 'dark blue';\"\"\")\n",
    "df = pd.DataFrame(cur.fetchall())\n",
    "df.columns = [x[0] for x in cur.description]\n",
    "df"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Select the name, color, and number of moons for the 4 largest planets that don't have rings and order them from largest to smallest"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 27,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>name</th>\n",
       "      <th>color</th>\n",
       "      <th>num_of_moons</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <td>0</td>\n",
       "      <td>Jupiter</td>\n",
       "      <td>orange</td>\n",
       "      <td>68</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>1</td>\n",
       "      <td>Earth</td>\n",
       "      <td>blue</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>2</td>\n",
       "      <td>Venus</td>\n",
       "      <td>yellow</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <td>3</td>\n",
       "      <td>Mercury</td>\n",
       "      <td>gray</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "      name   color  num_of_moons\n",
       "0  Jupiter  orange            68\n",
       "1    Earth    blue             1\n",
       "2    Venus  yellow             0\n",
       "3  Mercury    gray             0"
      ]
     },
     "execution_count": 27,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "cur.execute(\"\"\"SELECT name, color, num_of_moons FROM planets \n",
    "               WHERE rings = 0\n",
    "               ORDER BY mass DESC\n",
    "               LIMIT 4;\"\"\")\n",
    "df = pd.DataFrame(cur.fetchall())\n",
    "df.columns = [x[0] for x in cur.description]\n",
    "df"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## Summary"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Congratulations! NASA is one step closer to embarking upon its mission to Mars. In this lab, You practiced writing `SELECT` statements that query a single table to get specific information. You also used other clauses and specified column names to cherry-pick the data we wanted to retrieve. "
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.6.9"
  },
  "toc": {
   "base_numbering": 1,
   "nav_menu": {},
   "number_sections": true,
   "sideBar": true,
   "skip_h1_title": false,
   "title_cell": "Table of Contents",
   "title_sidebar": "Contents",
   "toc_cell": false,
   "toc_position": {},
   "toc_section_display": true,
   "toc_window_display": false
  },
  "varInspector": {
   "cols": {
    "lenName": 16,
    "lenType": 16,
    "lenVar": 40
   },
   "kernels_config": {
    "python": {
     "delete_cmd_postfix": "",
     "delete_cmd_prefix": "del ",
     "library": "var_list.py",
     "varRefreshCmd": "print(var_dic_list())"
    },
    "r": {
     "delete_cmd_postfix": ") ",
     "delete_cmd_prefix": "rm(",
     "library": "var_list.r",
     "varRefreshCmd": "cat(var_dic_list()) "
    }
   },
   "types_to_exclude": [
    "module",
    "function",
    "builtin_function_or_method",
    "instance",
    "_Feature"
   ],
   "window_display": false
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}

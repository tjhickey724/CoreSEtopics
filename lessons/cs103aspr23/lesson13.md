# Jupyter lab and Data Analysis

Today we show how to use the csv package to read excel files into lists of dictionaries
and how to visualize that data using the matplotlib package.

Here is the jupyter notebook we'll use:

``` python
# import packages
import csv
import matplotlib.pyplot as plt

# read data as a list of dictionaries
with open("UN_world_data.csv","r",encoding='utf-8') as infile:
    data = list(csv.DictReader(infile))
print(len(data))

# get the list of years (and convert from string to int
years = [int(d['Year']) for d in data]
print(years)

# get the total world population from 1950 to 2020
total_pop = 'Total Population, as of 1 January (thousands)'
population = [d[total_pop] for d in data]
print(population)

# the numerical data uses spaces to make it easier for humans to read
# we need to clean this data so it can be converted to an integer or float
def clean(num):
    return float(num.replace(" ",""))
clean(" 1 234 567")

# get the population data as a list of floats
population = [clean(d[total_pop]) for d in data]
print(population)

# plot the population data in a figure 20 wide and 10 deep
# add a title, axis labels, and specify the axis dimensions
plt.figure(figsize=(20,10))
plt.plot(years,population,label='population')
plt.grid()
plt.axis([1950,2030,0,1e7])
plt.title("World Population",fontsize=30)
plt.xlabel("year")
plt.ylabel('population')

# do the same with world fertility
fertility_rate='Total Fertility Rate (live births per woman)'
fertility = [clean(d[fertility_rate]) for d in data]
print(fertility)
plt.figure(figsize=(20,10))
plt.plot(years,fertility)
plt.axis([1950,2030,0,6])
plt.grid()

# and life expectancy
life_expectancy_label = 'Life Expectancy at Birth, both sexes (years)'
life_expectancy = [clean(d[life_expectancy_label]) for d in data]
print(life_expectancy)
plt.figure(figsize=(20,10))
plt.plot(years,life_expectancy)
plt.grid()
plt.axis([1950,2030,0,80])
```

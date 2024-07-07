## Pandas and Seaborn

### Pandas
Using in-built pandas plotting tools can do some simple drawing:

For example, drawing trigonometric functions
```Python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

t = np.arange(0.0, 2.0, 0.01)
s = np.sin(2*np.pi*t)
fig, ax = plt.subplots()
ax.plot(t, s)
plt.show ()  # Should add the “plt.show()” statement at the end of the drawing code.

```
![[../../../assets/Data Analysis/Figure_1.png|450]]

Or just drawing a Line Graph
```Python
rain = pd.read_csv("https://milliams.com/courses/data_analysis_python/rain.csv")
rain.plot()
```
![[../../../assets/Data Analysis/Figure_2.png|450]]

But to do more graphs, we will use Seaborn.

### Seaborn
seaborn is a third-party library which provides an easy-to-use interface for plotting tabular data which integrates with pandas really well.
Should import:
```Python
import seaborn as sns
import matplotlib.pyplot as plt
```

Use the official [built-in data](https://github.com/mwaskom/seaborn-data) as an example.
```Python
mpg=sns.load_dataset('mpg')  # import mpg.csv
mpg
```
![[../../../assets/Data Analysis/mpg.png|700]]

#### Displot
```Python
m=sns.load_dataset('mpg')
w1=m['weight'].dropna()
sns.displot(w1, kde=True)
```
![[../../../assets/Data Analysis/Figure_3.png|450]]

In this code, w1=m['weight'].dropna() is used to remove missing values (if there are) from 'weight' , caz distplot can not handle missing data；   
kde=True means show density curve (Does not display by default);

Another parameter - bins, can controls the number of distributed rectangles:
```Python
sns.displot(w1, bins=100, kde=True)
```
![[../../../assets/Data Analysis/Figure_4.png|450]]


To refine the image, here are some more parameters:
```Python
sns.set(style="white", color_codes=True)
sns.displot(w1,rug=True, kde=True,
            bins=30,
            color='green',
            edgecolor='m',
            label='Hello'
            )
plt.xlabel('Weight')
plt.ylabel('Nums')
plt.title('Weight distribution')
plt.show()
```
![[../../../assets/Data Analysis/Figure_5.png|450]]

##### Hue, row and col
According to the col, it can drawing different graphs:
```Python
sns.set(style="darkgrid", color_codes=True)
m=sns.load_dataset('mpg')
sns.displot(data=m, x='weight',
            bins=30,
            color='green',
            edgecolor='m',
            col='origin'
            )
plt.show()
```
![[../../../assets/Data Analysis/Figure_6.png|450]]

Use hue, can drawing different boxes in one graphs:
```Python
sns.set(style="darkgrid", color_codes=True)
m=sns.load_dataset('mpg')
sns.displot(data=m, x='weight',
            bins=30,
            edgecolor='m',
            hue='origin'
            )
plt.show()
```
![[../../../assets/Data Analysis/Figure_6.1.png|450]]

Since the two histograms were not comparable because of the different number of observations between the two groups, we could use the stat option to plot the density instead of the count to address this issue.
```Python
sns.set(style="darkgrid", color_codes=True)
m=sns.load_dataset('mpg')
sns.displot(data=m, x='weight',
            edgecolor='m',
            hue='origin',
           
            stat="density",  # draw density
            common_norm=False  # normalize every histogram 
            )
plt.show()
```
![[../../../assets/Data Analysis/Figure_6.2.png|450]]


##### Kde 
A more comparable way is using kernel density estimation (kde):   
The function is 'sns.kdeplot()'
```Python
sns.set(style="white")
m=sns.load_dataset('mpg')
w1=m['weight'].dropna()
sns.kdeplot(w1, shade=True, color='y')
plt.show()
```
![[../../../assets/Data Analysis/Figure_7.png|450]]

Two curves in one graph:
```Python
sns.set(style="white", palette="muted", color_codes=True)
m=sns.load_dataset('mpg')
ax1=sns.kdeplot(m['model_year'][m['origin']=='usa'],color='r', label='usa')
ax2=sns.kdeplot(m['model_year'][m['origin']!='usa'],color='b', label='not usa')
plt.legend()  # Show label
plt.show()
```
![[../../../assets/Data Analysis/Figure_8.png|450]]

Or just use 'kind="kde"' in displot:
```Python
sns.set(style="darkgrid", color_codes=True)
m=sns.load_dataset('mpg')
sns.displot(data=m, x='weight',
            kind="kde",
            hue='origin',
            common_norm=False
            )
plt.show()
```
![[../../../assets/Data Analysis/Figure_9.png|450]]

-----

#### Relplot
This is a graph-level function that uses scatter plots and line plots to represent statistical relationships.
```Python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
sns.set(style="darkgrid")

car = sns.load_dataset('mpg')
sns.relplot(data=car, x='weight', y='horsepower', 
            kind='scatter'  # By default, it is set to scatter
           )
plt.show()
```
![[../../../assets/Data Analysis/Figure_10.png|450]]

If we want to add a new variable, for example, we wanna see whether the heavy vehicle has a larger displacement:
```Python
sns.relplot(data=car,
            x='weight', y='horsepower',
            size='displacement'
           )
plt.show()
```
![[../../../assets/Data Analysis/Figure_11.png|450]]

Still can use hue, hue parameter to encode the color. The color varies according to the type of variable passed to this parameter. 
```Python
sns.relplot(data=car, x='weight', y='horsepower',
            size='displacement',
            hue='acceleration'
           )
plt.show()
```
![[../../../assets/Data Analysis/Figure_12.png|450]]

If we pass the origin column, which is a category variable, it will have three color markers instead of a continuous (light to dark) hue:
```Python
sns.relplot(data=car, x='weight', y='horsepower',
            size='displacement',
            hue='origin'
           )
plt.show()
```
![[../../../assets/Data Analysis/Figure_13.png|450]]

> Note: you could custom colors by adding like palette='crest', see: http://seaborn.pydata.org/tutorial/color_palettes.html

In larger data sets, it may be difficult to distinguish colors in point groups. For clarity, we add the style of the pots to the drawing:
```Python
sns.relplot(data=car, x='weight', y='horsepower',
            size='displacement',
            hue='origin',
            style='origin',
            alpha=0.8  # Sets the transparency of the pot
           )
plt.show()
```
![[../../../assets/Data Analysis/Figure_14.png|450]]

Same as the displot, use col can create sub-chart which will make the information be easier to read:
```Python
sns.relplot(data=car,
            x='weight', y='horsepower',
            size='displacement',
            hue='acceleration',
            palette='crest',
            col='origin',
            col_wrap=2,  # col_wrap tells how many columns there are in a row
            alpha=0.8
           )
plt.show()
```
![[../../../assets/Data Analysis/Figure_15.png|450]]

-----

#### Catplot
When having categorical variables in data, you usually want to compare between the categories.

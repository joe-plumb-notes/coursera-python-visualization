# coursera-python-visualization

## Week One
- Alberto Cario's chart wheel details different dimensions of charts. Largely, scientists and data heads have a different approach to visualization than journos or artists.
- Tufte's data-ink ration is important to consider. Is it giving you info? If not, get rid of it. Things like: backgrounds, colour, needless keys, lines, boarders, etc
- Crowdflower / Amazon Mechanical Turk : Crowdsource feedback
- Sparklines are small data visuals that can be embedded alongside data/text to provide a quick glance understanding to the reader. They are used for data where trends or distribution characteristics are important. Sparktweets are a thing too. Using unicode chars to tweet sparklines of data (e.g. sleep time)
- Liefactor - size of the ffect shown in the graphic vs size of the effect in the data. 
- Maybe check out Alnberto Cairo 'The Truthful Art', in which a 5 part framework for considering information graphics is presented. This is made up of:
  1. Truthful: we must be honest when we clean and summarize data. Practice self-deception - be sceptical, as we are often searching for patterns, that our focus on finding a pattern can lead us to present data inauthentically. There is an obligation to audience, as techniques we use to shine light on particular pieces of data, but if we omit the ability for the reader to explore the phenomena more fully then we run the risk of generating doubt and distrust.
	2. Functional: This is a continuum. Direct data labeling, and reducing ink can make a vis more functional.
	3. Beauty : making a vis more beautiful can make it more memorable. 
	4. Insightful : Shouldn't replicate data in tables, but deliver data to the 'aha' moment. 
	5. Enlightening : composed of the previous four, adding social responsibility dimension. 

## Week Two
Matplotlib - we'll get hands on with the visualisation library.
### Architecture
- Ipython has some special support for matplotlib, which can be enabled using '%matplotlib notebook'.
- When you run this, matplotlib is being configured to render in the browser. This configuration is called a backend (abstraction layer which knows how to interact with the operating environment), and matplotlib has a number of different backends available. Many interactive backends, as well as hard copy backends which support rendering SVGs/pngs.
   Backend for notebook = NbAgg.
   Variables are accessed using Get and Set.
- Next layer, artist layer
   Abstraction around drawing and layout. Set up containers, which include a subplot with one or more axes. Axes is the most common thing to interact with.
  Primatives and collections - base drawing items, and collections such as a path to connect items. 
- Scripting layer
   Simplify and speed up interaction, by doing magic for us. Makes us effective with mpl. We use pyplot in this course. Coreagraphs the two other layers.
Pyplot in matplotlib is a procedural method for building a visualization (we tell what actions we want to take to order our data). There are also declarative forms, e.g. d3.js, HTML.

Links:
- [matplotlib architecture explained](http://www.aosabook.org/en/matplotlib.html)
- [10 rules for better figures](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003833)

_There's an xkcd library for matplotlib!_

### Basic plotting
- pyplot module is part of the scripting architecture.
- Passing in `(x, y)` pairs to begin, first one does not show as it defaults to a line chart and there is no second point to draw a line to. Passing in `(x, y, '.')` tells how to render the data point.
- Subsequent calls to `plt` updates the visualization, which is a particular feature of the interactive back-end used here.
- pyplot scripting interface manages a lot of objects for you, keeps track of the latest figure, sub-plots, and axis objects, and hides some if this behind methods of its own.
- more verbose approach is to interact with the mpl object API (like interacting with the artist layer directly)
- define a new plot, plot a point
```
# create a new figure
plt.figure()

# plot the point (3,2) using the circle marker
plt.plot(3, 2, 'o')
```
- get plot axis, and define point on axis
```
# get the current axes
ax = plt.gca()

# Set axis properties [xmin, xmax, ymin, ymax]
ax.axis([0,6,0,10])
```
- Other example code and bits and bobs available in the notebook.

### Scatterplots
Few things to remember:
- `gcf` gets the current figure and assigns to a variable, `gca` gets the current axis (we saw this above)
- pyplot does keep track of the axis object for us, but we can get to them and change them when/if we need.
- pyplot mirrors the API of the axis objects, i.e. pyplot.plot is calling the axis plot functions underneath.
- Function declaration for most functions in mpl end with an open set of kw arguments, which control a lot of different things. 
- separation of data points into lists is common.
- each data point is _not_ a separate instance of an object, so list comprehensions, zip, lambdas, and list unpacking are all useful.
- _zip takes a number of iterables, and creates tuples out of them, matching elements on index._ zip's lazy evaluation (because it's a generator) means we need to use the list function if we want to see the results of iterating over a zip.
- must be happy converting data to and from list / tuple form.
```
zip_generator = zip([1,2,3,4,5], [6,7,8,9,10])

print(list(zip_generator))
# the above prints:
# [(1, 6), (2, 7), (3, 8), (4, 9), (5, 10)]
```
#### Axes
- generally labelled. 
- we can make calls direction on pyplot to change/add labels, e.g. `plt.xlabel('Label for x-axis')`, `plt.title('Title')`
- `plt.legend()`, `frameon=False` will remove the frame, `title` will set legend title.
` plt.legend(loc=4, frameon=False, title='Legend')`

### Line plots
- Again, created with the `plt.plot()` function, and plots a number of different series of data points, connecting these with a line. Broadcasting in np to create data more effeciently.
```
import numpy as np

linear_data = np.array([1,2,3,4,5,6,7,8])
exponential_data = linear_data**2

plt.figure()
# plot the linear data and the exponential data
plt.plot(linear_data, '-o', exponential_data, '-o')
```
- `'-o'` gives dots, `'-s'` gives squares
- When plotting lines, we only give y-axes values, as the plot fucntion figures we want to use the index of the series as the x value. The plot also identifies the two series of data, and colours them differently (dots and lines). This is different from the scatter, where we had to define each point separately.
- Can `fill_between` objects in the visualisation. Typically seen in line chats, but can be used elsewhere too.
#### Time Series
- Often, will be looking to plot time series, i.e. datetime on the x axis.
- can use np.arange to build datetime array, however it is in a different format (np) than the standard py datetime. Can use map and pandas to convert from one to the other 
```
import pandas as pd

plt.figure()
observation_dates = np.arange('2018-01-01', '2018-01-09', dtype='datetime64[D]')
observation_dates = list(map(pd.to_datetime, observation_dates))
plt.plot(observation_dates, linear_data, '-o',
	 observation_dates, exponential_data, '-o')
```
- NB: wrapping the map function in list(), as mpl cannot read the iterator map() returns. This is a simple workaround, but not a very memory efficient way of handling the data. See [difference between iterator and a list](https://stackoverflow.com/questions/25653996/what-is-the-difference-between-list-and-iterator-in-python). This works in matplotlib 2.0.0, , but not 2.1.0.. I got around this by using the `plt.plot_date()` function.
- The dates overlap when the above is rendered, so could think about removing the year (as all dates are in the same year) and adding this in the chart axis title.
- Can rotate labels to make them readable, and add LaTex in the text to have more control (put between $'s)
```
# rotate the tick labels for the x axis
for item in x.get_ticklabels():
    item.set_rotation(45)

ax.set_title("Exponential ($x^2$) vs. Linear ($x$) performance")
```
### Bar Charts
- Plotting bar charts requires the x component (i.e. where it sits on the x axis), and the height of the bar. We can also pass the width.
```
xvals = range(len(linear_data))
plt.bar(xvals, linear_data, width = 0.3)
```
- Lots of different types of bars, can add error bars, have stacked or horizontal bars .. 
### De-junkifying a plot
#### 1.
```#TODO: remove all the ticks (both axes), and tick labels on the Y axis
plt.tick_params(axis='y', which='both', bottom='off', top='off', labelbottom='off')
plt.yticks([])
```
Solution given:
`plt.tick_params(top='off', bottom='off', left='off', right='off', labelleft='off', labelbottom='on')` - good that you can change ticks for both sides using this function.

#### 2.
```
# TODO: remove the frame of the chart
ax = plt.gca()
ax.set_frame_on(False)
```
Used [this](https://shocksolution.com/2011/08/17/removing-an-axis-or-both-axes-from-a-matplotlib-plot/)
Solution given:
```
# remove the frame of the chart
for spine in plt.gca().spines.values():
    spine.set_visible(False)
```
I guess this is better because it's directly calling the spine elements in the ax object

#### 3.
```
# TODO: make one bar, the python bar, a contrasting color
plt.bar(pos, popularity, align='center', color=['#639fff', '#d6e5fc', '#d6e5fc', '#d6e5fc', '#d6e5fc'])
# TODO: soften all labels by turning grey
plt.ylabel('% Popularity').set_color('grey')
[i.set_color('grey') for i in plt.gca().get_xticklabels()]
```
Solution given:
```
# change the bar colors to be less bright blue
bars = plt.bar(pos, popularity, align='center', linewidth=0, color='lightslategrey')
# make one bar, the python bar, a contrasting color
bars[0].set_color('#1F77B4')

# soften all labels by turning grey
plt.xticks(pos, languages, alpha=0.8)
plt.ylabel('% Popularity', alpha=0.8)
plt.title('Top 5 Languages for Math & Data \nby % popularity on Stack Overflow', alpha=0.8)

```
So I did two-birds-one-stone approach for the bars .. don't know why separating these colour jobs is better.. also setting the label alpha did not change them grey imo.. they look the same.
#### 4.
```
# TODO: direct label each bar with Y axis values
for bar in bars:
  plt.gca().text(bar.get_x() + bar.get_width() /2, # get current axes, write text, for each bar. get_x() is a function of the 							   # rectangle object, returning left coord of the rectangle, then add width/2
                 bar.get_height() - 5,             # i.e. center it. plt.text(x, y, text), essentially
                 str(int(bar.get_height())) + '%', 
                 ha='center', color='w', fontsize='12')
```
This is the solution given - I tried a few ways using [this](https://matplotlib.org/examples/api/barchart_demo.html) but couldn't get it, so looked to understand how the ax.text part works. I think the example is slightly confusing as well because it uses the `fig, ax = plt.subplots()` approach which breaks the existing vis, but is clearly not necessary as we can call the current axes info using `plt.gca()`, as in the above answer.

### Assignment 2
I created this chart showing highest and lowest recorded temperatures, and then outliers in the following year:
![Data Visualization](./img/weathervis_final.png)

## Week 3
### Subplots
- Multiple plots in the same figure. 
```
plt.figure() # Still required
plt.subplot([number of rows], [number of columns], [plot number i.e. set current axis])
```
- A conceptual grid os overlayed on the figure, and subplot allows different axis to be created in different portions.
- Plot to the subplot by calling plt.plot()
- call subplot again with different last parameter to change current plot.
- Run the risk of misleading the reader by having differing x,y axis in subplots - these can be shared using sharey/sharex when you set the second subplot as the current axis
```
ax1 = plt.subplot(1,2,1)
plt.plot(linear_data, '-o')
ax2 = plt.subplot(1,2,2, sharey=ax1)
plt.plot(exponential_data, '-x')
```
- mpl plots start at 1, not 0, so be aware if iterating through a matrix/list to create subplots, start at 1.
- can create a grid of subplots easily:
```
fig, ((ax1,ax2,ax3), (ax4,ax5,ax6), (ax7,ax8,ax9)) = plt.subplots(3, 3, sharex=True, sharey=True) # Note subplotS
ax5.plot(linear_data, '-')
```
- This approach turns off x and y labels apart from subplots that boarder these edges. Turning the labels back on by iterating through the axis objects. Do this by iterating through the axes labels you have, or iterating through all in the figure:
```
for ax in plt.gcf.get_axes():
	for label in ax.get_xticklabels() + ax.get_yticklabels():
		label.set_visible(True)
```
- Interactive notebook backend doesn't always redraw, this can be forced using `plt.gcf().canvas.draw()`
### Histograms


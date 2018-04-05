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
![Data Visualization](/img/weathervis_final.PNG)

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
- Interactive notebook backend doesn't always re-draw, this can be forced using `plt.gcf().canvas.draw()`

### Histograms
- A bar chart which shows frequency of a given phenomena, e.g. probability distributions (random, uniform, normal, chi squared)
- Probability functions can be visualised as a curve (y axis (limited between 0 and 1) holds the probability a given value would occur, and x value is the value itself). This is called a _probability density_ function. X axis values are labeled in terms of the distribution function - in normal distribtuion, this is usually in terms of standard deviations. _A histogram is just a bar chart where the x-axis is a given observation and the y-axis is the frequency with which that observation occurs._ We can plot a probability distibution by sampling from it.
- Sampling : picking a number from the distribution. More sampling, clearer representation of the distribution.
- Can sample using numpy and create good normal distributions. When doing this, mpl defaults to 10 bins for a histogram, can up this to get clearer visual representation of the distribution.
```
fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(2, 2, sharex=True)
axs = [ax1, ax2, ax3, ax4]

for n in range(0, len(axs)):
	sample_size = 10**(n+1)
	sample = np.random.normal(loc=0.0, scale=1.0, size=sample_size)
	axs[n].hist(sample, bins=100) # This is where we set the bins to be more than default
	axs[n].set_title('n={}'.format(sample_size))
```
- How many bins should you use? Unclear. Binning aids decision making, as we get to see an aggregated view. Similar to using aggregate statistics like standard deviation, mean etc. 
#### Gridspec
- Allows you to map axes over multiple cells in a grid.
e.g.
```
plt.figure()
Y = np.random.normal(loc=0.0, scale=1.0, size=10000)
X = np.random.random(size=10000)
plt.scatter(X,Y)
```
This is very unclear and difficult to determine whether there is any distribution
```
import matplotlib.gridspec as gridspec

plt.figure()
# Set up grids and spaces for visualisations
gspec = gridspec.Gridspec(3, 3)

top_histogram = plt.subplot(gspec[0, 1:])
side_histogram = plt.subplot(gspec[1:, 0])
lower_right = plt.subplot(gspec[1:, 1:])
# Plot data
Y = np.random.normal(loc=0.0, scale=1.0, size=10000)
X = np.random.random(size=10000)
lower_right = scatter(X, Y)
top_histogram.hist(X, bins=100)
s = side_histogram.hist(Y, bins=100, orientation=horizontal)
```
Can clear visualisations from gridspec to prevent having to do all the work again
```
top_histogram.clear()
top_histogram.hist(X, bins=100, normed=True)
side_histogram.invert_xaxis() # Can also flip/rotate
```
Can hard-code the axis values too
```
for ax in [top_histogram, lower_right]:
	ax.set_xlim(0,1)
for ax in [side_histogram, lower_right]:
	ax.set_ylim(-5, 5)
```
### Boxplots
- Aggregate statistics of various samples concisely: median, min and max, and the iqr (inter quertile randge)
- IRQ : first, second, third and fourth quartiles. NMeasure of variabilitiy of the data.
- `whis = 'range'`, tells boxplot to set the whiskers to max and min values.
- to plot multiple boxes, send each column in as a list
` _ = plt.boxplot([ df['normal'], df['random'], df['gamma'] ], whis='range')`
- can overlay an axis on top of another using a toolkit that typically ships with mpl, in the example `import mpl_toolkits.axes_grid1.inset_locator as mpl_il` is used.
- Rendering a boxplot without the whis argument plots the whiskers half way between the IQR (top of box - bottom of box * 1.5). This is a way to detect outliers - points plotted beyond the whiskers are called flyers.
- Confidence interval can also be plotted, most commonly by adding notches to the box to represent 95% confidence interval. 
- Paper on (selecting the correct number of bins.)[http://users.stat.umn.edu/~gmeeden/papers/hist.pdf]

### Heatmaps
- Visualising 3d data and take advantage of proximities. (Lat, Long, another measure)
- Break down when there is no continuous relationship between dimensions, e.g. catagorical data. (People look for patterns where there are none.)
- plot using `plt.hist2d(X,Y, bins=25)`
- Colour bar for legend `plt.colorbar()`

### Animation
- mpl does have support for animation and interactivity. These heavily depend on support from the backend. nb backend does support some level of interactivity. 
- the animation is created by calling FuncAnimation - the animation is built by iteratively calling a function you define: this function will either clear the axis and redraw the next frame, or return a list of objects that needs to be re-drawn.
```
import matplotlib.animation as animation

n = 100
x = np.random.randn(n)
```
```
# create the function that will do the plotting, where curr is the current frame
def update(curr):
    # check if animation is at the last frame, and if so, stop the animation a
    if curr == n: 
        a.event_source.stop()
    plt.cla()
    bins = np.arange(-4, 4, 0.5)
    plt.hist(x[:curr], bins=bins)
    plt.axis([-4,4,0,30])
    plt.gca().set_title('Sampling the Normal Distribution')
    plt.gca().set_ylabel('Frequency')
    plt.gca().set_xlabel('Value')
    plt.annotate('n = {}'.format(curr), [3,27])
```
```
fig = plt.figure()
a = animation.FuncAnimation(fig, update, interval=100)
```
- can export these animated files using ffmpeg (can take some time to set up), but means you can export these from jupyter environment/

### Interactivity
- Similar to animation, closer to the artist layer. Have to reference the canvas object of the current figure.
- Canvas handles drawing events, and events (mouse move, button press etc) are core component of CS.
- connect event definition to event listener. 
`plt.gcf().canvas.mpl_connect('button_press_event', onclick)`
- pick event is the most important for us, allows you to present additional information when data is clicked on.

### Assignment 3
I created this simple bar chart which displays a y value, and colours the bars depending on relative distance from the sample mean:
![Data Visualization](/img/assignment3.png)

## Week 4

### Plotting with Pandas
- Pandas uses pml under the hood, and proivdes functions to visualize data.
- mpl has pre-defined styles we can use, to change the look of the plots. Changing this for mpl will change the charts for pandas too.
- we can use `plt.style.available` to see the styles provided.
- built-in pandas visualization helps with fast plotting of series and df to aid data exploration. 
- `df.plot();` is a simple wrapper around `plt.plot`. Colours are different, because of the styles we used. `;` surpresses unwanted output in a notebook.
- select which plot you want by passing it in the `kind` parameter.
- can also use `df.plot.[kind]()`, e.g. `df.plot.scatter('A', 'B', c = 'B', s=df['B'], colormap='viridis')`
```
ax = df.plot.scatter('A', 'C', c='B', s=df['B'], colormap='viridis')
ax.set_aspect('equal')
```
- setting aspect ratio to equal allows you to see the true relationship between the axes
- can easily draw box plots `df.plot.box();`, histograms `df.plot.hist(alpha=0.7);`, and kernel density estimate plots `df.plot.kde();`, which are useful for visualizing an estimate of a varialbles probability density function. These are particularly useful when you want to derive a smooth continuous function from a given sample.
- `pd.tools.plotting` are a set of built in tools that assist with visualizing large data sets or high dimensional data, e.g. scatter matrix, e.g. `pd.tools.plotting.scatter_matrix([df]);`
- can manually build this - 4x4 subplot of histograms and scatters..
- `parallel_coordinates` plots are a common way to visualise high dimensional / multi-variate data. 
- each variable in the data set corresponds to an equally spaced parallel vertical line. The values of each variable are then connected by lines between for each individual observation. 

### Seaborn
- mpl wrapper, to make visuals more appealing, and makes complicated plots simpler to create.
- `import seaborn as sns`
- 

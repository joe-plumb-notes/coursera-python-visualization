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
- 

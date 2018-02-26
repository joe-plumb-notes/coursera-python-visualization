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




 



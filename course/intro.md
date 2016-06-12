# Analyzing and Presenting Spatial data

Welcome! Over the course of the next four days we're going take a look at a wide range of tools and techniques for working with spatial data in the humanities. By the end of the week, it's my hope that everyone will be able to build effective spatial humanities projects using off-the-shelf tools, and also know how to get started with more complex or unusual projects that call for some custom data preparation or interface design.

In a conceptual sense, the course is laid out along a conceptual axis that I think provides a useful way to think about the range of GIS practices in the humanities. On one end is a set of methodologies that might be thought of as focusing deliberately on "small data" - Drucker and Nowviskie's notion of "graphesis," a mode of knowing in the humanities that arises from very close consideration of spatially-inflected materials and information. At the other end of the spectrum is a set of practices that more closely resemble GIS as practiced in the sciences, the social sciences, government, and industry - a focus on large amounts of data, with an eye towards building data models that are simple and tractable at very large scales. In many ways this echoes the distinction in computational text analysis between traditional "close reading" and "distant reading," which makes it possible to ask questions at a much larger scale, but often with the tradeoff that the questions themselves tend to become somewhat more specific, structured, and empirical - which, depending on the goals of the project, can be either a good or a bad thing. both ends of this continuum have their advantages, and neither offers a complete set of solutions - indeed, as we'll see over the course of the next few days, both approaches (and everything in between) have real shortcomings, and I tend to think it's best to think of them as complements - two different hammers in the toolbox, suitable for different projects and even at different stages of the same project.

While we're thinking about different ways of slicing and dicing spatial projects in the humanities, another question is whether a project makes use of existing tools - frameworks like Neatline, CartoDB, ESRI StoryMap, Odyssey.js, etc - or if it's programmed from scratch. Again, there's no right or wrong answer here. The tools are enormously useful - when you have a project that fits well into the capabilities of one of these existing frameworks, it's often possible to go from nothing to a more-or-less complete project in a really short amount of time - just days or weeks, where programming the equivalent functionality from scratch would take months or even years. But, the tools also impose a particular set of assumptions and constrains onto a project - the type of information that can be modeled, how it's presented, the range of things that the reader of the project is able to do with the data, etc. Many times these constrains are a small price to pay for the gains in productivity, ease-of-use, and sustainability. But other times the constraints can be stifling - some projects just don't fit neatly within the set of imagined use cases the that tools cater towards, and when this is the case it's often actually faster to just program something from scratch instead of trying to coax an existing framework to do something that it wasn't really designed to do. In the same way that we'll run the whole gamut from small data to big data, we'll also work at different points along this axis, from the most off-the-shelf - say, Google Fusion Tables - to the most bespoke - writing custom data extraction scripts in Python and using of Javascript libraries like Leaflet and d3 to make custom visualizations.

Last, before we dive in, a quick note about the technical details of the course. More often than not, I think it makes sense to walk through the whole process of getting set up with the computing environment necessary to do the task at hand - installing software, getting set up with code editors, configuring the development environments, etc. As opposed to, say, working with pre-configured virtual machines or doing everything through centrally-hosted web services. (Though, we'll do some of that too.) This setup process can be annoying at times, since things invariably go wrong - differences between operating systems, different versions of the same operating systems, and different configurations of software all conspire to make it difficult to draw up a single list of steps that will work without fail on all systems. But, in the long run, I think this is actually time well-spent. Debugging software configuration problems is sort of an unavoidable tax levied on pretty much any kind of computational work - it can be postponed, but not avoided. (I certainly spend a huge amount of time doing it myself.) And, at the end of the process - there's something liberating about having full control over the means of production in these kinds of projects. When you know how all the pieces fit together, it's easier to know where to look and what questions to ask when things go wrong. It also decouples you somewhat from capabilities and bandwidth of the IT services available at your institution, which can make it easier to get up and running with new projects without having to go through formal channels for support.

# Course outline

**Part 1: Neatline, Graphesis**

We'll start out by looking at Neatline, a digital mapping framework developed at the University of Virginia between 2012 and 2015. Neatline was designed around Drucker's notion of "graphesis," and is designed to make it possible to create hand-crafted, richly humanistic interactions with maps and spatial information. We'll start out by creating a digital edition of Charles Minard's famous infographic of Napoleon's 1812 invasion of Russia - we'll use QGIS to georeference a digital scan of the original image, load the prepared image into Geoserver (an open-source geospatial tile server that makes it possible to publish georeferenced images on the web), import the web-accessible image into Neatline as an overlay, and then finally use Neatline's vector annotation tools to create a set of interactive overlays on top of Minard's original map.

**Part 2: Data extraction, cleaning, and formatting**

Building on the (painstaking, slow) process of manually picking out place references from Whitman - in the second day we'll use a Python package called Polyglot to do named entity extraction (NER) on a set of novels downloaded from the Project Gutenberg corpus. Once we've pulled out a list of toponyms from the raw text, we'll use the Data Science Toolkit to geocode each of place names, which will assign a regular longitude / latitude coordinate to each place name and make it possible to plot them on a map.

**Part 3: Spatial data visualization**

Next, working with the data extracted from the novels, we'll take the lists of toponyms and use a series of off-the-shelf tools that are well-suited for visualizing this type of well-structured spatial data - starting with simple solutions like geojson.io and Google Fusion Tables and then spending more time with CartoDB, a full-featured framework (similar to Neatline in some respects) that makes it possible to create custom layers, query the data in interesting ways, and publish maps to the web.

**Part 4: Web map programming**

Last, we'll end by dipping back into the code and learning the basics of building custom web mapping applications using HTML, CSS, and Javascript, which can come in handy when the existing tools don't do exactly what you want. After plotting the locations extracted from a novel on an interactive map, we'll build a time slider that makes it possible to visualize the progression of the toponyms over the course of the "novel time" of the text.
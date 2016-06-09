# Analyzing and Presenting Spatial data

Welcome! Over the course of the next four days we're going take a look at a wide range of tools and techniques for working with spatial data in the humanities. By the end of the week, it's my hope that everyone will be able to build effective spatial humanities projects using off-the-shelf tools, and also know how to get started with more complex or unusual projects that call for some custom data preparation or interface design.

In a conceptual sense, the course is laid out along a kind of conceptual axis that I think provides a useful way to think about the range of GIS praxis in the humanities. On one end is a set of methodologies that might be thought of as deliberately "small data" - hand-crafted data, Drucker and Nowviskie's notion of "graphesis," a mode of knowing in the humanities that arises from very close consideration of spatially-inflected materials and information. At the other end of the spectrum is a set of practices that more closely resemble GIS as practiced in the sciences, the social sciences, government, and industry - a focus on large amounts of data, with an eye towards building data models that are simple and tractable at very large scales. In many ways this echoes the distinction in computational text analysis between traditional "close reading" and "distant reading," which makes it possible to ask questions at a much larger scale, but often with the tradeoff that the questions themselves tend to become somewhat more specific, structured, and empirical - which, depending on the goals of the project, can be either a good or a bad thing. both ends of this continuum have their advantages, and neither offers a complete set of solutions - indeed, as we'll see over the course of the next few days, both approaches (and everything in between) have real shortcomings, and I tend to think it's best to think of them as complements - two different hammers in the toolbox, suitable for different projects and even at different stages of the same project.

While we're thinking in dialectical terms, another useful way of slicing and dicing spatial humanities projects is along whether they make use of existing tools - frameworks like Neatline, CartoDB, ESRI StoryMap, Odyssey.js, etc - or if they're programmed from scratch. again, there's no right or wrong answer here. the tools are enormously useful - when you have a project that fits well into the capabilities of one of these existing frameworks, it's often possible to go from nothing to a more-or-less complete project in a really short amount of time - just days or weeks, where programming the equivalent functionality from scratch would take months or even years. but, the tools also impose a particular set of assumptions and constrains onto a project - the type of information that can be modeled, how it's presented, the range of things that the reader of the project is able to do with the data, etc. many times these constrains are a small price to pay for the gains in productivity, ease-of-use, and sustainability. but other times the constraints can be stifling - some projects just don't fit neatly within the set of imagined use cases the that tools cater towards, and when this is the case it's often actually faster to just program something from scratch instead of trying to coax an existing framework to do something that it wasn't really designed to do. In the same way that we'll run the whole gamut from small data to big data, we'll also work at different points along this axis, from the most off-the-shelf - say, Google Fusion Tables - to the most bespoke - writing custom data extraction scripts in Python and using of Javascript libraries like Leaflet and d3 to make custom visualizations.

 Last, before we dive in, a quick note about the technical details of the course. More often than not, I think it makes sense to walk through the whole process of getting set up with the computing environment necessary to do the task at hand - installing software, getting set up with high-quality code editors, configuring the development rigs that make it easy to write custom code, etc. As opposed to, say, working with pre-configured virtual machines or doing everything through centrally-hosted web services. (Though, we'll do some of that too.) This setup process can be annoying at times, since things invariably go wrong - different operating systems, different versions of the same operating systems, different combinations of software installed on different machines - all conspire to make it difficult to draw up a single list of steps that will work without fail on all systems. But, in the long run, I think this is time well-spent - debugging software configuration problems is sort of an unavoidable tax levied on pretty much any kind of computational work, I think. (I certainly spend a huge amount of time doing it myself.) And, at the end of the process - there's something liberating about having as close to full control as possible over the means of production in these kinds of projects. When you know how all the pieces fit together, it's easier to know where to look and what questions to ask when things go wrong. It also decouples you somewhat from capabilities and bandwidth of the IT services available at your institution, which can make it easier to get up and running with new projects without having to go through formal channels for support.

## Course outline

**Part 1: Graphesis, Minard**

We'll start out by looking at Neatline, a digital mapping framework developed at the University of Virginia between 2012 and 2015. Neatline was designed around Drucker's notion of "graphesis," and is designed to make it possible to create hand-crafted, richly humanistic interactions with maps and spatial information. We'll start out by creating a digital edition of Charles Minard's famous infographic of Napoleon's 1812 invasion of Russia - we'll use QGIS to georeference a digital scan of the original image, load the prepared image into Geoserver (an open-source geospatial tile server that makes it possible to publish georeferenced images on the web), import the web-accessible image into Neatline as an overlay, and then finally use Neatline's vector annotation tools to create a set of interactive overlays on top of Minard's original map.

**Part 2: Spatial texts, Whitman**

Next, we'll look at a set of extensions for Neatline that were developed to make it possible to create interactive spatial editions of text documents, a requirements that came up again and again in the process of using Neatline with faculty and students. We'll each take a stanza of "Salut au Monde" - one of Whitman's largest and most intricate "spatial catalogues" from _Leaves of Grass_ - and create a set of Neatline exhibits that plot out the locations (often delightfully fuzzy and imprecise) mentioned in the poem. At the end, we'll take all of the individual exhibits and merge them together into a single exhibit for the whole poem.

**Part 3: Data extraction, cleaning, and formatting**

Building on the (painstaking, slow) process of manually picking out place references from Whitman - in the second day we'll use a Python package called Polyglot to do named entity extraction (NER) on a set of novels downloaded from the Project Gutenberg corpus. Once we've pulled out a list of toponyms from the raw text, we'll use the Data Science Toolkit to geocode each of place names, which will assign a regular longitude / latitude coordinate to each place name and make it possible to plot them on a map.

**Part 4: Spatial data visualizations**

Next, working with the data extracted from the novels, we'll take the lists of toponyms and use a series of off-the-shelf tools that are well-suited for visualizing this type of well-structured spatial data - starting with simple solutions like geojson.io and Google Fusion Tables and then spending more time with CartoDB, a full-featured framework (similar to Neatline in some respects) that makes it possible to create custom layers, query the data in interesting ways, and publish maps to the web.

**Part 5: Web map programming**

Last, we'll end by dipping back into the code and learning the basics of building custom web mapping applications using HTML, CSS, and Javascript, which can come in handy when the existing tools don't do exactly what you want. After plotting the locations extracted from a novel on an interactive map, we'll build a time slider that makes it possible to visualize the progression of the toponyms over the course of the "novel time" of the text.

---

## Georeference Minard's map in QGIS

So, to get started with our Neatline edition of Mianrd's Napoleon infographic.

1. First, grab the raw image of the inforgraphic from Wikipedia - https://commons.wikimedia.org/wiki/File:Minard.png. Click on the preview to get the full-size version, and then right click, choose "Save Image As," and save it somewhere easy to find.

1. Next, we'll download QGIS ("Quantum GIS"), an open-source GIS suite that provides much of the functionality offered more heavyweight (and expensive) solutions like ArcGIS. Go to http://www.qgis.org, click **Download Now**, and select the downloader for your operating system.

1. **MAC** If you're on Mac, you'll get taken to KyngChaos downloads page. Download the **QGIS 2.14.3-1** file and open up the DMG archive. Inside, you'll see four different `.pkg` files, labeled 1 to 4. QGIS is the final one, but you'll need to install each of these in order to get all of the required dependencies.

1. **WINDOWS** TODO

1. Once the install finishes, open up QGIS. First, we need to add a modern-geography base layer to QGIS as a baseline to georeference against. The easiest way to do this is to install a plugin for QGIS called QuickMapServices. Open the plugins manager by clicking on **Plugins > Manage and Install Plugins**.

1. Search for "QuickMapServices," click the row, and then click **Install**.

1. Close out of the plugins manager and click on the **Web > QuickMapServices > OSM > OSM Mapnick**. This will add a basic OpenStreetMap base layer to the project.

1. Now, the georeferencing. Click on **Raster > Georeferencer > Georeferencer**.

1. Click on the right-most button in the main menu bar with the green "+" icon and select the image of Minard's infographic that we downloaded from Wikipedia. In the "Coordinate Reference System Selector" window that appears, leave the defaults as is (WGS 84 / Pseudo Mercator) and click OK.

1. Now, to georectify the image, we just have to lay down a set of control points that link locations on Minard's map with spatial coordinates on the modern base layer. Click on the "Add Point" button, and then click once on the image just to the left of the "Kowno" label on the left side of the image.

1. In the window that appears, click on **From map canvas**. The georectifier will disappear, showing the OpenStreetMap in the QGIS project. Hold down the space bar and move the cursor to drag the map over to Kaunas in the middle of modern-day Lithuania. Click once on the middle of the city to lay down a corresponding point on the map.

1. Repeat this process for Moscow, on the other side of the image - click once right on the meeting point between the beige and black flowchart segments, click **From map canvas**, find Moscow on the base layer, and click on it to lock in the association. Now, with two points, we've given the georeferenced enough information to position, scale, and rotate Minard's image so that it roughly lines up with the corresponding geography on the base layer. Before we export the rectified image, though, let's create a directory to house the various files we'll be working with on this project.

1. **MAC** Click on the search button at the top right of the screen, search for "terminal," and open up the Terminal application. I generally like to organize things into a top-level `Projects` directory in my user's home directory. If you don't already have something like this, go ahead and create this directory with: `mkdir Projects`. Then, change down into the new directory with `cd Projects`, and create a sub-directory called `minard` with `mkdir minard`.

1. **WINDOWS** TODO

Click on the yellow geat button to open the "Transformation Settings" dialog.

1. In "Transformation type," choose "Helmert."

1. Click the "..." button next to "Output Raster," find the `minard` dierctory that we created above, and enter `minard` as the name for the exported file.

1. Click **OK** to save the settings, and click the "play" button to export the file.

1. Now, we can open up the exported image in QGIS and check the rectification. Go back to regular QGIS and click the "Add Raster Layer" button in the vertical toolbar on the left. Find the `~/Projects/minard` directory and select the new `minard.tif` produced by the Georeferencer. Click **Open**, and the rectified image will appear on the map.

So - what's up with those ugly black borders around the image? When an image is georeferenced, it often ends up getting rotated away from its original orientation, as was the case here. In the final image, QGIS will fill in the gaps between the image and the containing rectangle with "0-value" pixels, which, annoyingly, results in the black borders around the image when it gets displayed on a map. We can fix this, though, by using a command-line utility called GDAL to replace these black pixels with transparent pixels, which gets rid of the borders. GDAL is great - it's kind of like a swiss-army knife for working with spatial data, and comes in handy in lots of different contexts.

1. Use this `gdalwarp` command to strip off the black borders:

`gdalwarp -srcnodata 0 -dstalpha minard.tif minard-noborders.tif`

1. Last, rename the final file so that includes your last name, which will make it easier to keep track of when we upload it to the web in the next step.

`cp minard-noborders.tif minard-[your last name].tif`

Now we've got a georectified and cleaned-up GeoTIFF, which can be brought into Neatline for annotation and display.

## Upload the georectified map to Geoserver

Before we can import the map into Neatline, though, we first need to upload it to a special type of web server that dynamically splits the image up into smaller "tiles," whch can be laid on top of a regular base layer. We'll be using Geoserver, an open-source tiling server that sits behind the geospatial data repositories at places like UVa, Stanford, Harvard, and elsewhere. During development, Geoserver can be downloaded and run locally on your computer, but, so that we can publish our images onto the web, we're going to be using an instance of Geoserver hosted by AcuGIS, a company that provides hosting services for a range of GIS software packages (including Omeka and Neatline).

1. First, we need to upload the file to the server that's running Geoserver. Go to http://cpanel.dclure.gis-cdn.net and log in with **dcluregi / HILT2016**.

1. Click on **File Manager**. Find the row for the directory called "hilt" and double click on it.

1. Click **Upload** and then **Select File** and find your `minard-[name].tif` file. Once it finishes uploading, click the **Go Back to "/home/dcluregi/hilt"** link.

Now that the file is uploaded to the server, we can register the layer in Geoserver.

1. Go to http://dclure.gis-cdn.net/geoserver and log in with **hilt / spatialdata**.

1. Click on **Workspaces** and then **Add new workspace**.

1. Enter your last name into the "Name" field, and put some kind of URL into "Namespace URI" - a personal website, department homepage, etc, (Doesn't matter what - just has to be a valid URL.) Click **Submit** to create the new workspace.

1. Click on **Stores**, **Add new store**, and then **GeoTIFF**.

1. Select the workspace you just created and `minard` in the "Data Source Name" field.

1. Click **Browse...** next to the "URL" field, and then select **Home directory** from the dropdown at the top of the dialog. Click on the "hilt" directory, and then on the listing for your file. Click **Save** to create the new store.

1. From the next screen, click **Publish**.

1. Scroll down to the "Coordinate Reference Systems" field set and click **Find...** next to "Declared SRS." Search for "900913" (hint - this looks like "Google," if you squint your eyes, which isn't a coincidence!) and click the **900913** link under "Code" for the "WGS84 / Google Mercator" listing.
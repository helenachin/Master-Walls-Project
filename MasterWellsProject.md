# Master Wells Project
---
## Industrial Problem
Often companies have several databases of where they think their Wells are located.
These could be from CAD, ArcGIS, Decision Space Geographics, [Petrel](https://www.software.slb.com/products/petrel), [OpenWells](https://www.landmark.solutions/OpenWells),
[COMPASS](https://www.landmark.solutions/COMPASS-Directional-Well-Path-Planning), Drilling Info, HSI, State Regulatory, etc. All these might not agree on the
Well location.  Therefore, a software tool which could build a Master Well Database
from numerous different databases will be designed.  The wireframe of the software
and how it works are explained below.
![Image of the wireframe of the software](https://github.tamu.edu/cherry-chen/Geog676/blob/master/homework/ePortfolio/Wireframes/Wireframe1.png)

---
## Step 1: Choose software to develop
Recently, **Python** is considered as one of the most popular and widespread used
languages due to its highly readable syntax, availability, and compatibility. And the
software tool will be built on Python 3.6.X version.

This software tool also works with ArcGIS. ESRI has a Python library called **arcpy**
which is the Python interface for ArcGIS. We use this module to do all the GIS work we
wish to perform. It is this module that enables Python to do GIS related operations
without us having to create our own code to do such things. And since it's made and
supported by ESRI it lends a lot of credibility to the module.

The hosted service **GitHub.com** is free for open source projects and it has
fundamentally improved open source collaboration. In this project, the GitHub can be
used to share codes with other developers and practitioners in the related field to
comment and test the codes and the product. Therefore, we can *update the status and
report through these opinions.* 

In a nutshell, the software tool will be built on python with ArcGIS pro, and sufficiently utilize GitHub.

[GitHub Setup Instruction](https://github.tamu.edu/TAMU-GEOG-676-GIS-Programming/Content/blob/master/homework/01.md)

---
## Step 2: Load Several Datasets
Before loading the dataset through python, we need to make sure that each dataset at
least includes **API well number** and **coordinates systems(X, Y)**. An **API well number** or
API number is a "unique, permanent, numeric identifier" assigned to each well drilled
for oil and gas in the United States. The coordinate system is used to display the
exact location of the wells on the map.

The best format of the dataset would be **a plain text file (txt.)**  because it is easy to
handle and read by python. However, other popular formats of datasets like CSV,
JSON, they would be better supported with some modules installed like pandas. The example code below shows how to use python to load the data.

![Read data from the provided text file](https://github.tamu.edu/cherry-chen/Geog676/blob/master/homework/ePortfolio/Wireframes/dataread.png)

In this step, we are trying to collect all the points with their coordinates from different
datasets for the same API well number.   First, we will use Object Oriented
Programming to create a function which is used to add the new points into the
database for a specific well if they found it after looping through a new dataset.
Eventually, we will obtain a database encoded by specific API well number. The flow chart illustrates we will find the the well we are intersted every time we loop through a new data resource and then we put all the locations together which belong to the same well identification.

![The flow chart show how step2 works](https://github.tamu.edu/cherry-chen/Geog676/blob/master/homework/ePortfolio/Wireframes/Wireframe2.png)

[Code Example for step 2](https://github.tamu.edu/cherry-chen/Geog676/blob/master/homework/week02/2.py)

---
## Step 3: Inverse coordinates of different well locations to determine if any degree or their accuracy
In this step, we will loop through a specific API well number database to check the
accuracies of different well locations. The **accuracy** is a measure of the degree of
closeness of a measured or calculated value to its actual value. Since we don't know
the actual value, we will use **standard deviation** to check the **accuracy and precision** of
the location points for each specific well number database.

**Method 1**: We will get a separate list of X coordinates or Y coordinates. Then do some
arithmetics like standard deviation for this list.

[Code Example for Method 1](https://github.tamu.edu/cherry-chen/Geog676/blob/master/homework/week01/1.py)

**Method 2**: Use Numpy to retrieve standard deviation more easily. A Python NumPy
array is designed to deal with large arrays. Therefore, we consider a pair of
coordinates(X, Y) as an array.

Code Examples:
```python
import numpy
arr = numpy.array([(471316.383, 5000448.782), (470402.493, 5000049.216)],
                  numpy.dtype([('X', '>f8'),('Y', '>f8')]))
np. std(arr, axis = 0)
```

---
## Step 4: Write to master well geodatabase
In the last step, we basically analyze the accuracy and precision of each API well
database. Now we want to review the data points on the actual maps, arcpy is needed.
First, we need to create a master well geodatabase which is better to manage. Then we
will project the XY coordinates on the map. The example codes are presented below:

![Codes example to create a geodatabase](https://github.tamu.edu/cherry-chen/Geog676/blob/master/homework/ePortfolio/Wireframes/geodatabase.png)

[More Code Example for Step 4](https://github.tamu.edu/cherry-chen/Geog676/blob/master/homework/week03/3.py)

---
## Step 5: Build a check that determines if the coordinate was NAD83 vs. NAD27
One of the largest issues, when we process the well locations, is the position errors
resulting from different datums, especially like NAD 27 and NAD83. **NAD27** has
served the oil and gas industry very well for the last 80 years and it is still the survey
reference system used by virtually some oil and gas companies. Living in a changing
world, we cannot ignore that the movement from our reference system of the past to
a reference system of the future has happened. **NAD83** has become the trend because
it corrects some of the inherent distortions in the NAD27 survey network that have
accumulated over time and distance. Also, it has the advantage of being the native
datum used by modern satellite surveying technologies such as the Global
Positioning System (GPS). For those locations containing the coordinates information
we just need to use python to access, while for those with undefined coordinates, we
might use **buffer** tool to build a check process.

Actually, data in the UTM coordinate system referenced to the NAD 1983 datum is
approximately 200 meters north of the same data referenced instead to the NAD 1927
datum. Thus, **a 200-meter difference** in the northing is diagnostic to check whether
the coordinate was NAD83 or NAD27. The whole process is shown below.

![The Check Process](https://github.tamu.edu/cherry-chen/Geog676/blob/master/homework/ePortfolio/Wireframes/The%20Check%20Process.png)

[More Code Example for Step 5](https://github.tamu.edu/cherry-chen/Geog676/blob/master/homework/week04/4.py)

---
## Step 6: Build a graduated color Wells locations to indicate confidence of Master Well Location
After we differentiate different locations with their coordinates systems, it is better we
could convert them into the same coordinate systems. Next, we could generate a
graduated color well locations by selecting the fields of X coordinates or Y coordinates
with **quantile classification**. This graduated color map work as a map help us finalize
the more accurate location points.

![An example of the graduated color points](https://github.tamu.edu/cherry-chen/Geog676/blob/master/homework/ePortfolio/Wireframes/Graduatedcolorexample.jpg)

[More Code Example for Step 6](https://github.tamu.edu/cherry-chen/Geog676/blob/master/homework/week05/5.py)

---
## Step 7: Use aerial images to check the Master Well Database
The aim of this step is to allow users to select the Master Well Location from different
aerial images. To obtain aerial images taken from different years, we might need to
create a **composite raster** that combines raster images of various wavelength.

![An example of the composite raster](https://github.tamu.edu/cherry-chen/Geog676/blob/master/homework/ePortfolio/Wireframes/compositeraster.jpg)

[Code Example for Step 7](https://github.tamu.edu/cherry-chen/Geog676/blob/master/homework/week06/6.py)

---
## Other Plans

---
### Return on Investment(ROI)
(ROI(Return on Investment) measures the gain or loss generated on an investment
relative to the amount of money invested.In this software development in a specific
problem, we plan to identify the benefits and costs it might bring.

**Benefits**
- More accurate location of the well and thus reduce chances of dry holes. Usually, a
dry-hole will result in a great loss of money since oil drilling is expensive. According
to the Arizona Geological Survey, Oil drilling in Arizona costs between **$400,000 to $1,000,000**, depending on the depth of the hole and its location.
- The wells are easier to be maintained if we can locate them more precisely.  For
maintenance work, we need to clean, treat, repair, test, inspect and do other
activities.

**Costs**
- Development: It includes the cost of time and labor. In 2017, a simple mobile
application with a well-defined, limited set of features usually costs anywhere
between **$20,000 and $80,000**. A moderately complex application, such as an
enterprise application with web and mobile functionality or a customer-facing
service application, will range from **$80,000 to $150,000**. And the most complex
software can cost **up to $1 million** to develop; this includes data-driven applications,
external apps that support mobile devices, social media, or reporting, and
enterprise software with complex business logic.
- Ongoing maintenance: Over the first 2 years of the application being released, half
of the initial cost is spent on new features, bug fixing, optimization, and general
updates.

---
### Interface with Users
Since this project is closely connected with ArcGIS Pro, we will use the interface of the
customized toolbox. Another reason is that future users would be more familiar with
this interface.

![An example of the interface of the ArcGIS tool](https://github.tamu.edu/cherry-chen/Geog676/blob/master/homework/ePortfolio/Wireframes/interface.png)

---
### Requirement gathering
Requirement gathering refers to gather the requirements for the software from the
expected users. In this project, I assume that the projects can benefit the companies
experts in the gas or oil industry. Therefore, we need to get in touch with at least 5 GIS
experts from this kind of company.

Before the project starts, we can interview the future customers and ask their opinions
about the problem and their anticipated function of the GIS software. After we had
some ideas,  requirements should be collected again with initial prototypes we create.

---
### Maintenance of the software
- Align with the **current coordinates**. For example, most of the wells locations are still
stored in the NAD27 or NAD83 now, but NAD27 might be not used anymore over
time, and Step 5 needs to be improved then.
- Maintaining **every month** based on the modification request or report problems
from the user.
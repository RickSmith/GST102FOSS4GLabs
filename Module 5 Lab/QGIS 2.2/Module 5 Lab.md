# GST 102: Spatial Analysis
## Lab 5 - Vector Data Analysis - Creating a Site Selection Model
### Objective – Using the QGIS Desktop Graphical Modeler to Perform a Site Selection Analysis

Document Version: 9/22/2014

**FOSS4G Lab Author:**
Kurt Menke, GISP  
Bird's Eye View GIS

**Original Lab Content Author:**
Richard Smith, Ph.D.  
Texas A&M University - Corpus Christi

---

Copyright © National Information Security, Geospatial Technologies Consortium (NISGTC)

The development of this document is funded by the Department of Labor (DOL) Trade Adjustment Assistance Community College and Career Training (TAACCCT) Grant No.  TC-22525-11-60-A-48; The National Information Security, Geospatial Technologies Consortium (NISGTC) is an entity of Collin College of Texas, Bellevue College of Washington, Bunker Hill Community College of Massachusetts, Del Mar College of Texas, Moraine Valley Community College of Illinois, Rio Salado College of Arizona, and Salt Lake Community College of Utah.  This work is licensed under the Creative Commons Attribution 3.0 Unported License.  To view a copy of this license, visit http://creativecommons.org/licenses/by/3.0/ or send a letter to Creative Commons, 444 Castro Street, Suite 900, Mountain View, California, 94041, USA.  

This document was original modified from its original form by Kurt Menke and continues to be modified and improved by generous public contributions.

---

### 1 Introduction

In this lab you will learn how to streamline a workflow with a model. Using the QGIS Desktop Graphical Modeler you will string tools together using the output of one operation as the input to the next. You can later edit model parameters and share it with others.   
This lab includes the following tasks:

+ Task 1 Exploring the Data
+ Task 2 Creating the Model - Part 1
+ Task 3 Creating the Model - Part 2
+ Task 4 Creating the Model - Part 3

### 2 Objective: Understanding Basic Vector Analysis Using Overlays

The objective of this lab is for the student to learn how to conduct a site selection analysis using the QGIS Desktop Graphical Modeler.

### 3 How Best to Use Video Walk Through with this Lab

To aid in your completion of this lab, each lab task has an associated video that demonstrates how to complete the task.  The intent of these videos is to help you move forward if you become stuck on a step in a task, or you wish to visually see every step required to complete the tasks.

We recommend that you do not watch the videos before you attempt the tasks.  The reasoning for this is that while you are learning the software and searching for buttons, menus, etc…, you will better remember where these items are and, perhaps, discover other features along the way.  With that being said, please use the videos in the way that will best facilitate your learning and successful completion of this lab.

### Task 1 Exploring the Data

2.	Open QGIS Desktop 2.2.0
3.	Browse to the Lab 5 data folder and add the five shapefiles (Airports, Counties, CityBoundaries, Roads, and Water_features) to QGIS Desktop.
4.	Arrange the data layers so that points are on top of lines, which are on top of polygons. Move the CityBoundaries polygon layer above the Counties layer (figure below).

![Site Selection Data Layers](figures/Site_Selection_Data_Layers.png "Site Selection Data Layers") 

Clearly the data layers cover a variety of spatial extents. One likely step will be to Clip them so that they all cover the same spatial extent.

5.	One item to check right away is the coordinate reference system of each layer. This is standard practice in any analysis. Open the Layer Properties for each layer and determine the coordinate reference system for each.

**Question # 1 – What is the coordinate reference system for these layers?**

6.	Since they are all in the same coordinate reference system no re-projecting will be required. 

When building a site selection model you are usually presented with a scenario. The scenario has parameters, which you will have to address to find the solution. The scenario below describes the parameters for the site selection you will do for this lab. The best method to solve a scenario like the one below, is to extract the parameters from the description, and write them down into verbal descriptions. Then from your descriptions you can transcribe them into GIS operations: buffer, intersect, =, <> etc..  

Scenario
A company is looking to lease a helipad at an airport for their company helicopter. This company is situated in Nueces County, Texas and wants the helipad to be within 3 miles of the Corpus Christi city limits, but not in the city limits. The pilots request that the airport or heliport be within a half mile of any source of water. It must also be within a mile from a County road.  As you have some knowledge of GIS you are being asked to find the best solution. 

7.	Complete the table below. The first two entries in Description and Extent have been entered for you. Study the data layers in QGIS Desktop and complete the table. At that point you will have a better understanding of what you have to work with.

Data Layer  --  Description and Extent

Airports  --  Airports, Airfields, Heliports of Texas
City Boundaries  --  City Limits of Populated Areas of Texas
Water Features  --  
Counties  --  
Roads  --  

8.	The next step is to think about the site selection analysis. Read the Scenario carefully and think about the steps required to finish it. The first two GIS Operations have been worked out for you (Table 2). Complete the rest of the GIS Operations from the Scenario description. If there is a selection involved open the attribute table and determine what the field name will be and what value you will be selecting for.

**Scenario Parameter  --  GIS Operations**

Scenario Parameter  --  Select Nueces County from USA Counties and Save as a new shapefile. Attribute column = County. Value = Nueces

Airports of the USA  -- Clip to Nueces County

Must be within 3 miles of Corpus Christi city limits but not inside city limits  --  Select by Attributes Corpus Christi from CityBoundaries of Texas. The attribute column = Name. Value = Corpus Christi. Buffer 3 miles around the Corpus Christi City limits. Use the Difference tool to erase the Corpus Christi boundary from the buffer.

Must be within half a mile of water  ==  

Must be within a mile of a County road.

9.	Save your project as Lab 5.qgs.


### Task 2 Creating the Model - Part 1
Now you understand the data you have to work with. You also know that you will have to determine which airports meet the criteria laid out in the completed table above. The next task is to begin building the model.

1.	Open QGIS Desktop 2.2.0 and open Lab 5/Data/Lab 5.qgs if it is not already.
	
2.	From the menu bar choose Processing -> ToolBox. Notice at the bottom of the ToolBox panel that there is a toggle between a Simplified and Advanced Interface to the Toolbox. All the tools are available, no matter which view you choose. However, this setting will determine the view of geoprocessing algorithms in the Graphical Modeler as well. For purposes of this lab, choose the Advanced interface.

3.	There is a default folder for Models, and if saved there they will show up as models in Toolbox -> Models. You can set the location of this default folder. From the menu bar choose Processing -> Options and configuration. Expand the Models section. Double click the current default folder and an ellipsis button will appear to the right. Click on the ellipsis button, navigate to the Lab 5/Data/MyData folder, and click Select folder (figure below). Click OK to close the Processing options.

![Processing options](figures/Processing_options.png "Processing options")

4.	From the menu bar now choose Processing -> Graphical Modeler (figure below).

![Graphic Modeler](figures/Graphical_Modeler.png "Graphic Modeler")

The Graphical Modeler allows you to create complex models using an intuitive graphic interface. When working with a GIS, most analysis operations are not isolated, but rather part of a chain of operations. Using the Graphical Modeler, that chain of processes can be incorporated into a single process. This allows you to run the entire analysis as a single operation. It also allows you to execute the same model on a different set of inputs! No matter how many steps and different operations it involves, a model is executed as a single operation, thus saving time and effort, especially for larger models.

The left hand panel has two tabs Inputs and Algorithms. The model itself will be designed in the right hand window. 

5.	At the top of the main design window are two text fields: [Enter model name here] and [Enter group name here]. Name the model Helipad Site Selection and the Group GST 102 (figure below).


![Model Name and Group](figures/Model_Name_and_Group.png "Model Name and Group")

6.	Next you will save your model. Click the Save button  . It should now default to the MyData folder. Name it HelipadSelection (figure below). Click Save. It will be saved as a .model file. You should receive the Model was correctly saved message. Click OK.

![Saving the Model](figures/Saving_the_Model.png "Saving_the_Model")

The first analysis step will be to create your study area boundary. This will involve creating a Nueces County shapefile from the USA wide Counties layer.


7.	The first step in creating a model is to define the inputs. All of your data is vector data. Click on the Inputs tab to the Graphical Modeler and double click on Vector layer. The Parameter definition window opens. Here you are simply defining the conceptual parameter. You will not actually connect it to the GIS data layer until you are ready to run the model. Name the parameter Counties lyr.  
NOTE: The lyr will be your naming convention for parameters, which are GIS layers. Other parameters will be attributes. 

The Shape type is Polygon and it is a Required parameter (figure below). Click OK.

![Parameter Definition](figures/Parameter_Definition.png "Parameter Definition")

8.	The Counties lyr parameter will now appear in the design window (figure below).

![Counties lyr Parameter Added to the Graphical Modeler](figures/Counties_lyr_Parameter_Added_to_the_Graphical_Modeler.png "Counties lyr Parameter Added to the Graphical Modeler")

9.	The next input parameter will be a Table field. Double click on Table field. The Parameter name will be CountyName attr and the Parent layer is Counties lyr (figure below).
NOTE: Ending the name with ‘attr’ will be the naming convention for Table field Inputs.

 
Click OK.


![CountiesName attr Parameter Definition](figures/CountiesName_attr_Parameter_Definition.png "CountiesName attr Parameter Definition")
10.	Drag the CountyName attr input so that it is below Counties lyr (figure below). You can arrange the model elements into an attractive and intuitive flow.

![Two Inputs Added](figures/Two_Inputs_Added.png "Two Inputs Added")

11.	Now you will add the Extract by attribute algorithm. Click on the Algorithm tab. It looks a lot like the Toolbox. Double click on QGIS algorithms->Vector selection tools-> Extract by attribute. The Extract by attribute window opens Fill it out as follows (figure below):

	a.	Input layer =  Counties lyr

	b.	Selection attribute = CountyName attr

	c.	Comparison will be ==

	d.	Value (Refer to your notes in the table above)

**NOTE:** It will not need quotes, just type it in as it appears in the attribute table.

e.	Output<OutputVector> = NuecesCounty 

**NOTE:** There is a slight difference here between a Graphical Modeler tool and a standard Toolbox tool. Here the output can be saved as a temporary file that will be used as input to the next algorithm, or it can be saved to a permanent layer that you will specify when you run the model. Typing in anything in this space tells the Modeler that this output will be saved. The text you supply will be the description for the output when executing the model. You will choose the actual name of the shapefile and its location when you execute the model. Since you may want the Nueces County boundary for cartographic reasons you will choose to save it as a layer.

f.	Click OK

![Extract by Attribute Settings](figures/Extract_by_Attribute_Settings.png "Extract by Attribute Settings")

12.	The first complete algorithm has been added! You can drag the parameter boxes  as you’d like and the connecting arrows will follow (figure below). You can also click on the Edit button ![Edit button](figures/Edit_button.png "Edit button")  on any model parameter at any time. This will reopen the algorithm window or model parameter window From there you can make changes if necessary.


![Model with Nueces County Created by Extract by Attribute](figures/Model_with_Nueces_County_Created_by_Extract_by_Attribute.png "Model with Nueces County Created by Extract by Attribute")

13.	Save the model and Close it. From the Toolbox panel, expand Models ->GST 102. There is your model (figure below).

![Helipad_Site_Selection_Model_in_Toolbox](figures/Helipad_Site_Selection_Model_in_Toolbox.png "Helipad_Site_Selection_Model_in_Toolbox")

14.	Your model is obviously not yet complete. However, it is already a geoprocessing tool that you can open, set parameters for and run. Double click on Helipad Site Selection. Set Counties lyr to Counties, CountyName attr to COUNTY and keep Nueces County as a temporary file for now (Don’t fill it in!) (figure below). Click Run. The temporary NuecesCounty layer will be added to QGIS Desktop.

![Helipad Site Selection](figures/Helipad_Site_Selection.png "Helipad Site Selection")

15.	For now Remove this layer from QGIS Desktop and Save your project.

### Task 3 Creating the Model - Part 2

Now that your basic model is set up you will add to it. In this task you will work on geoprocessing the data for the CityBoundaries, Roads and Water_features layers. You will also clip the airports to Nueces County.

1.	Open QGIS Desktop 2.2.0 and open Lab 5/Data/Lab 5.qgs if it is not already.
	
2.	From the menu bar now choose Processing -> Graphical Modeler. Once the Graphical Modeler opens, click the Open Model ![Open Model](figures/Open_Model.png "Open Model")  button and open the Helipad Site Selection model (if it isn't already open). 
	
3.	Now you will add a parameter for Airports. Click the Vector layer input parameter and fill out the Parameter definition as in the figure below.

![Airports Parameter Definition](figures/Airports_Parameter_Definition.png "Airports Parameter Definition")

4.	You will clip Airports with the Nueces County layer. Click on the Algorithms tab and select QGIS Algorithms->Vector overlay tools-> Clip. The Input layer will be Airports lyr and the Clip layer will be the Output from algorithm 0(Extract by attribute) (figure below).
![Clip_Algorithm_Parameters](figures/Clip_Algorithm_Parameters.png "Clip_Algorithm_Parameters")

5.	Your model should now resemble the figure below.

![Clip Algorithm Added to Model](figures/Clip_Algorithm_Added_to_Model.png "Clip Algorithm Added to Model")

Now you will work with the CityBoundaries. This is the most involved parameter because the site must be both within 3 miles of Corpus Christi and beyond the Corpus Christi city limits.

6.	Add a Vector layer Input for CityBoundaries

	a.	Parameter name = CityBoundaries lyr

	b.	Shape type = Polygon

	c.	Required = Yes

	d.	Click OK

	e.	Situate it below the Airport lyr

7.	You will need to do an Extract by attribute on CityBoundaries in order to extract the Corpus Christi boundary. Therefore, you will need another Table field attribute. Click on Inputs and choose Table field.
 
	a.	Parameter name = CityBoundaries attr

	b.	Parent layer = CityBoundaries lyr

	c.	Click OK

	d.	Situate it below the CityBoundaries lyr

8.	Add another Extract by attribute algorithm to the model with the following parameters:

	a.	Input layer =  CityBoundaries lyr

	b.	Selection attribute = CityBoundaries attr

	c.	Comparison will be ==

	d.	Value = Corpus Christi

	e.	Output = leave blank

	f.	Click OK

9.	The next step for processing the CityBoundary site selection parameter is to buffer the Corpus Christi layer by 3 miles. Click on the Algorithms tab and choose QGIS Algorithms->Vector geometry tools-> Fixed distance buffer. Use the following parameters:
	
	a.	Input layer =  Output from algorithm 2(Extract by attribute)

	b.	Distance = 4828.03 (3 miles in meters)

	c.	Segments = 5

	d.	Dissolve result = Yes

	e.	Buffer<OutputVector> - leave blank

	f.	Click OK

10.	The last parameter detail related to the boundary of Corpus Christi is that the airport needs to be located outside the city limits. Therefore, you need to take the 3 mile buffer output, and run a Difference algorithm on it, with the Corpus Christi output as the Difference layer. This will take the buffer and erase from it the portion within the city limits, leaving a 3 mile ring around the city limits. Click on the Algorithms tab and choose QGIS Algorithms->Vector overlay tools-> Difference. Use the following parameters:


	a.	Input layer =  Buffer from algorithm 3(Fixed distance buffer)

	b.	Difference layer = Output form algorithm 2(Extract by attribute)

	c.	Difference<OutputVector> - leave blank

	d.	Click OK.

11.	Your model should now resemble the figure below.
	
![Model Incorporating Corpus Christi](figures/Model_Incorporating_Corpus_Christi.png "Model Incorporating Corpus Christi")

Now you will work on the Roads parameter. First, the County roads will be selected. Then those County roads will be buffered by one mile

12.	. Add a Vector layer input for Roads with the following parameters:

	a.	Parameter name = Roads lyr

	b.	Shape type = Line


	c.	Required = Yes
	d.	Situate it below the CityBoundaries attr input

	e.	Click OK.

13.	Add a Table field input with the following parameters:
	
	a.	Parameter name = Roads attr

	b.	Parent layer = Roads lyr

	c.	Situate it below the Roads lyr

	d.	Click OK

14.	Add another Extract by attribute algorithm to the model with the following parameters:
	
	a.	Input layer =  Roads lyr

	b.	Selection attribute = Roads attr

	c.	Comparison will be ==

	d.	Value = C

	e.	Output = leave blank

	f.	Click OK

15.	Now you will buffer the County roads by one mile. Add another Buffer algorithm and use the following parameters:
16.	
	a.	Input layer =  Output from algorithm 5(Extract by attribute)

	b.	Distance = 1609.34 (1 mile in meters)

	c.	Segments = 5

	d.	Dissolve result = Yes

	e.	Buffer<OutputVector> - leave blank

	f.	Click OK

16.	The model should now resemble the figure below.

![Model Incorporating Roads](figures/Model_Incorporating_Roads.png "Model Incorporating Roads")

The last parameter to address the distance to Water Features which be buffered by 0.5 miles.

17.	Add a Vector layer input for WaterFeatures with the following parameters:

	a.	Parameter name = Water lyr

	b.	Shape type = Line

	c.	Required = Yes

	d.	Situate it below the Roads attr input

	e.	Click OK.

18.	Now you will buffer the Water Features by 0.5 miles. Add another Buffer algorithm and use the following parameters:

	a.	Input layer =  Water lyr

	b.	Distance = 804.67 (0.5 miles in meters)

	c.	Segments = 5

	d.	Dissolve result = Yes

	e.	Buffer<OutputVector> - leave blank

	f.	Click OK

19.	Your model should now resemble the figure below.

![Model Incorporating Water Features](figures/Model_Incorporating_Water_Features.png "Model Incorporating Water Features")

20.	Save the model.


### Task 4 Creating the Model - Part 3

In this final task, you will combine all the individual parameters into one layer representing the combination of all parameters. This will represent the acceptable area for helipad locations. Then you will run a clip algorithm on airports to determine which airports meet all the criteria.
 
1.	Open QGIS Desktop 2.2.0 and open Lab 5/Data/Lab 5.qgs if it is not already.
	
2.	From the menu bar now choose Processing -> Graphical Modeler. Once the Graphical Modeler open click the Open Model ![Open Model](figures/Open_Model.png "Open Model")  button and open the Helipad Site Selection model.
	
3.	You can combine the water buffer, county roads buffer and the 3 mile ring around Corpus Christi using the Intersect algorithm. This will compute the area of overlap between the three layers. Click on the Algorithms tab and choose QGIS Algorithms->Vector overlay tools-> Intersection. Use the following parameters:
	
	a.	Input layer =  Difference from algorithm 4(Difference)

	b.	Intersect layer = Output form algorithm 6(Fixed distance buffer)

	c.	Intersection<OutputVector> - leave blank

	d.	You need to Intersect more than two layers. Click the Multiple selection   button. When the Multiple Selection window opens choose 8:Fixed distance buffer.
 
	e.	Click OK.

	f.	Click OK.

4.	The model will now look like the figure below with the Intersect algorithm taking all three inputs.

![Model with Intersect Added](figures/Model_with_Intersect_Added.png "Model with Intersect Added")

5.	Finally, you will clip the airports with the output of the Intersect giving you the final solution. Click on the Algorithms tab and choose QGIS Algorithms->Vector overlay tools-> Clip. Use the following parameters:

	a.	Input layer =  Clipped from algorithm 1(Clip)

	b.	Clip layer = Intersection from algorithm 8(Intersection)

	c.	Intersection<OutputVector> - Final Solution

6.	The final model should resemble the figure below.
	
**Note:** on the Final Clip Algorithm: At this time, there is a new bug that may prevent the final clip from working in the model. The error will be reported in the Log tab on the final step in red and it will read: *Error executing algorithm 9 [Errno 9] Bad file descriptor See log for more details.* This bug is not present on all installations and all platforms. If you don’t get this you can ignore this note. However, if you receive this error, simply end the model with the Intersection, and save out the output of the Intersection to your MyData folder. You can then perform the final clip manually.


![Final Model](figures/Final_Model.png "Final Model")

7.	Save the Model and Close it.
8.	Congratulations! You have created your first geoprocessing model!
9.	From the Toolbox panel, expand Models ->GST 102. 
10.	Double click on Helipad Site Selection. Fill out all the parameters as shown in the figure below. Click Run when ready.

![Helipad Site Selection Model Inputs](figures/Helipad_Site_Selection_Model_Inputs.png "Helipad Site Selection Model Inputs")

**NOTE on Running Models:** Models can also be run from the Graphical Modeler interface. You can click the Run model ![Run model button](figures/Run_model_button.png "Run model button")  button. You will be prompted to Save the model, then you will be presented with the same model window shown in the figure below.


**NOTE on Debugging Models:** Here we have written out the vast majority of outputs to temporary files. If you get an error or an incorrect result, it can be helpful to save out questionable intermediate datasets. For example, say you are getting an error or an unexpected result from a buffer operation. Try outputting the result of the Extract by attribute dataset that feeds into the buffer algorithm. This will let you determine if the prior step is working correctly. To do this all you have to do is enter a name for the output in question like you did for Nueces County and the Final Solution.

**NOTE on Python:** It is also possible to automatically create Python code that performs the same task as the model itself. The code can then be used to create a console script. You can modify that script to incorporate actions and methods not available in the graphical modeler, such as loops or conditional sentences.

This feature is also a very practical way of beginning to learn Python, how to use processing algorithms from the console, and how to create new algorithms using Python code. You can use it as a learning tool when you start creating your own scripts.

To save your model as Python, go to the Toolbox. Right-click on the model name and select Save as Python script in the context menu that will pop up. A dialog will prompt you to choose where you want to save the script.

![Saving Model as Python](figures/Saving_Model_as_Python.png "Saving Model as Python")

###Conclusion
In this lab you have been exposed to both site selection modeling and the use of Graphical Modeler in building a workflow. Building the model is certainly time consuming. However, the benefits are many, especially if this is a workflow you will have to perform many times. 

By clicking on the Export as image button 
![Graphical Modeler button](figures/Graphical_Modeler_button.png "Graphical Modeler button")  in the Graphical Modeler you can export the model to a graphic. This graphic can then be included in a report or a presentation. This can help you explain the technical workflow to others.

If the client changed their mind on the distance from Corpus Christi, you could simply adjust that buffer distance in the model, and re run it to see how the solution changed. Without the model, you would be starting from scratch in this scenario. 

This model can now be run against similar set of data at a different site such as Seattle or Boston. If you were to run it against another set of data, some of the parameters of the Extract by attribute would have to be edited. However, that is easily done. Models allow you to streamline big workflows. 

### 6 Discussion Questions

1.	How many airports meet the selection criteria?
2.	How does Graphical Modeler make spatial analysis easier?
3.	Export your model as a graphic and turn in with your lab.


### 7 Challenge Assignment

The client has decided that they can expand the Corpus Christi parameter from 3 miles to 3.5 miles. Edit the buffer distance in the model for the Corpus Christi buffer and determine how that changes the number of airports meeting their needs. Make a map of each scenario to show the client. You may want to output some intermediate datasets such as the Intersection or Buffers to be able to show these criteria on  your maps.
	



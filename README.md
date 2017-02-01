## Api Keys:
You will need a .env file in your local environment with api keys defined as the following:

yelp_ConsumerKey: Your Yelp ConsumerKey

yelp_ConsumerSecret =Your Yelp ConsumerSecret 

yelp_Token = Your Yelp Token

yelp_TokenSecret = Your Yelp Token Secret

google_API_KEY = Your Google Maps Webservice API Key

walkscore_api = Your Walk Score API key

You should also have the google maps javascript api enabled for your google key above. You can then insert your key into line 83 of HeatMap.html. This key is used to display the address of the markers in the map. 

````HTML
 <script src="https://maps.googleapis.com/maps/api/js?key=YOURAPIKEY&libraries=places"
  type="text/javascript"></script>
````

## How to Create Heatmaps:

Navigate to the directory that you are working in and enter the following into the command line:

python3 CreateHeatMaps.py

Then, the following prompt will open: 

Enter location in city, state format (NewHaven, CT): 

Enter the city that you wish to create heatmaps for. Note that depending on the size of the city, this may take 
anywhere from 15 minutes to multiple hours. Also, due to restrictions on the above apis, it is only possible to create heatmaps for 2-3 
cities every day.


## How to Visualize Heatmaps:

After CreateHeatMaps.py finishes, a file called 'SoofaDataYOURCITY.js' will be created in the DataFiles folder. Now open
HeatMap.html and go to line 56 (or search for id = "selectcity"). There, you will need to add the following code to see the heatmaps for your city:

```` HTML
 <option value="Cityname">Cityname, State</option>
 ````

 Then, open HeatMap.html with Google Chrome. You will see a menu called 'Cities.' Click on this menu and then click on your city. This will update the map view and you will be able to see the heatmaps for your city. Note: The current heatmap view is relative so once you zoom in/out of the map, the heatmap is regenerated. This means that a if you change how the map looks (zoom/move the map), the heatmap will also change. To change this, go to HeatMap.html and locate

 ````javascript
 var cfg1 = ...
 ````
 Then change cfg1.useLocalExtrema to false.

## How to use HeatMap.html:

You can select from a list of preloaded cities using the 'Cities' dropdown menu. The default city is Cambridge, MA. You can select which layers you want to see on the upper right corner of the map view. You should only be clicking on one layer! If you want to visualize multiple layers together, go to the upper right corner of the map view and click 'Multiple Layers.' You can then go to the 'Add to Multiple Layers' drop down menu and select the layers that you wish to visualize. 

You can also click on the map itself. This will create a draggable marker that you can move around. Dragging this marker will display the various scores for the marker location on the right side of the webpage.

## How Scores are Calculated:

Each score ranges from 0 (low) to 10 (high). Each of the scores are relative and a score of 10 is given to the best location in the selected city. Each city is sampled with a grid of locations. The points of the grid are 500 meters apart from each other in both the x and y direction.

#### Google Scores:
Each of the Google scores measures the the number of particular types of buisnesses/locations that are in a 500 meter radious of a sampled grid point. 
For example, the Google food score measures the number of restaurants in your selected city. If you want to see the full list of location types that go into calculating The Google scores, go to CreateHeatMap.py and look at the following function:
````python
def getGoogleData(...) ...
````

The scores and then normalized to be between 0 and 10. Note that the Google API only returns at most 60 results for each search.


#### Yelp Scores:

The Yelp scores are calculated as follows. For each grid point, we sample 20 buisnesses/locations around the grid point that falls into a particular category (for example restaurants). Then for each of the 20 locations, we add up ratings^2 * # of ratings and the final sum is the score associated with the grid point. Finally, the scores are normalized to be between 0 and 10.

#### Walk Scores:
The walkscores are pulled from the walkscore API. For more information/methodology, see [here](https://www.walkscore.com/methodology.shtml).

## Other Information:

The heatmaps are created using the Heatmap.js library. The map view is created using the leaflet.js library.

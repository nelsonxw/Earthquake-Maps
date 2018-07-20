# Illustrate Earthquake Data on Maps

Link to visualization: https://nelsonxw.github.io/Earthquake-Maps/
### Objective:
+ Retrieve recent earthquake data, including geographic locations, time and magnitudes.
+ Show the earthquake data on a world map with user interactiions.
+ Display the earthquake events in sequence as they took place over a period of time.

### Data Source:
Extracted data from [United States Geological Survey](https://earthquake.usgs.gov/earthquakes/feed/v1.0/geojson.php).  Used d3.json() function and Leaflet's geoJSON function to read in data and created earthquake layer on the map.
Code Snippets:
```javascript
/*read in earthquake data*/
d3.json(earthquakeLink,function(earthquakeData){
	var earthquakeLayer = L.geoJSON(earthquakeData,{
		/*add a layer to create circles for each earthquake*/
		pointToLayer:function(feature,latlng){
			/*change UNIX date format into UTC date and time string*/
			var utcTime = new Date(feature.properties.time);
			var formattedUTCTime = utcTime.toUTCString();
			var marker = L.circleMarker(latlng,{
				/*make different radius size based on the magnitude of earthquakes*/
				radius: feature.properties.mag * 2,
			    /*fill circle color based on the magnitude of earthquakes*/
			    fillColor: getColor(feature.properties.mag),
			    color: "#000",
			    weight: 0.2,
			    fillOpacity: 0.7
				}).bindPopup("<p class='popup-text'> <strong>Location: </strong>" + feature.properties.place + "</p>" +
				"<p class='popup-text'> <strong>Magnitude: </strong>" + feature.properties.mag + "</p>" +
				"<p class='popup-text'> <strong>UTC Time: </strong>" + formattedUTCTime + "</p>")
			return marker;
		}	
	})
	/*read in tectonic plates data and draw the lines*/
	d3.json(tectonicPlatesLink,function(plateData){
	var faultLineLayer = L.geoJSON(plateData,{
		style:{color:"#f97a02"}
		})
	/*add layers to the map*/
	createMaps(earthquakeData,earthquakeLayer,faultLineLayer);
	});
});
```

### Project Output:


+ Geojson data downlaoded from USGS website.
+ A world map with 3 different layers for users to choose from.
+ Allow uers to visualize locations of earthquakes and fault lines globally.
+ Locations of earthquakes are displayed as circles with differnt size and color that correlat to the magnitude of earthquakes.
+ Show popups when user moves mouse over locations of earthquakes.
+ Legends of magnitude scales are included on the map.
+ Animation of earthquakes on the map following the sequence of time.


### Techniques used:
+ Use **HTML** and **CSS** to build index page
+ Use **JavaScript** and **leaflet** and its plugins to build interactive maps and visualizations
+ Use **GitHub** to serve the web site

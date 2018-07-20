# Illustrate Earthquake Data on Maps

Link to visualization: https://nelsonxw.github.io/Earthquake-Maps/
### Objective:
+ Retrieve recent earthquake data, including geographic locations, time and magnitudes.
+ Show the earthquake data on a world map with user interactiions.
+ Display the earthquake events in sequence as they took place over a period of time.

### Data Source:
Extracted data from [United States Geological Survey](https://earthquake.usgs.gov/earthquakes/feed/v1.0/geojson.php).  Used d3.json function and Leaflet's geoJSON function to read in data and created earthquake layer on the map.  
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
### Tools used:
HTML, CSS, JavaScript, d3.js, leaflet, GitHub

### Major Steps:
+ Read earthquake data and tectonic plates data, and create layers for the overlay map.
+ Used mapbox to create 3 layers for the base map.
Code Snippets:
```javascript
var baseMaps = {
	    "Satellite": satelliteLayer,
	    "Grayscale": lightLayer,
	    "Outdoors": outdoorLayer
	  };
```
+ Added legend to the map to illustrate different magnitude by different color
```javascript
/*create legends for the map*/
	legend.onAdd = function (myMap) {

	    var div = L.DomUtil.create('div', 'legend');
	    var grades = [0, 1, 2, 3, 4, 5];
	    var labels = [];

	    /*create lists to represent different levels of magnitude scale*/
	    div.innerHTML = "<h4>Magnitude Scale</h4>";
	    for (var i = 0; i < grades.length; i++) {
	        labels.push('<li style="background:' + getColor(grades[i] + 1) + '"></i> ' +
	            grades[i] + ( (grades[i + 1] ? '&ndash;' + grades[i + 1] + '<br>' : '+') )
	            );
	    }
	    div.innerHTML += "<ul>" + labels.join("") + "</ul>";
	    return div;
	};

	legend.addTo(myMap);
	
function getColor(mag){
	if (mag <= 1) {
		return "#bfff00";
	} else if (mag <= 2) {
		return "#ffff00";
	} else if (mag <= 3) {
		return "#ffbf00";
	} else if (mag <= 4) {
		return "#ff8000";
	} else if (mag <= 5) {
		return "#ff4000";
	} else {
		return "#ff0000";
	}
}
```
+ Locations of earthquakes are displayed as circles with differnt size and color that correlat to the magnitude of earthquakes.
+ Show popups when user moves mouse over locations of earthquakes.
+ Legends of magnitude scales are included on the map.
+ Animation of earthquakes on the map following the sequence of time.




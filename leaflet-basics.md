# Leaflet JS basics

[Leaflet JS](https://leafletjs.com) is an open source JavaScript library that allows you to build web mapping applications. It supports both mobile and desktop applications, and has a small footprint. 

## Basics elements of a webpage

Any web application is build on HTML (HyperText Markup Language), which has a number of fixed elements. These elements will be in almost all web applications. 

```html
<!DOCTYPE html>
<html>
	<head>
		<title>Basic Leaflet Map</title>
		<meta charset="utf-8" />
	</head>
	<body>

	</body>
</html>
```

To add interactivity (such as events) into your web application, you can add Javascript (JS). This can be done using script tags. Then lastly, styling is done using CSS (Cascading Style Sheets). 

Here are some resources where you can learn more about HTML, JS and CSS:

- [W3C HTML tutorial](https://www.w3schools.com/html/)
- [W3C JavaScript tutorial](https://www.w3schools.com/js/default.asp)
- [W3C CSS tutorial](https://www.w3schools.com/css/default.asp)
- [FreeCodeCamp Curriculum](https://www.freecodecamp.org/learn) (if you want to take your coding to the next level, we highly recommend this resource)
- [FreeCodeCamp YouTube tutorials](https://www.youtube.com/c/Freecodecamp/featured) (there are a lot of full courses and crash courses available here)

## Creating a basic web map

When creating a basic web map, you need to start by adding the Leaflet JS library to your HTML code. You can find the latest JS and CSS links here, [https://leafletjs.com/examples/quick-start/](https://leafletjs.com/examples/quick-start/) (under Preparing your page). Next you will need to create a container for the map, we use a div for that with some styling. Below is an example of what you should now have (*last updated 01/09/2022*). 

```html
<!DOCTYPE html>
<html>
	<head>
		<title>Basic Leaflet Map</title>
		<meta charset="utf-8" />
		<link rel="stylesheet" href="https://unpkg.com/leaflet@1.8.0/dist/leaflet.css"
		integrity="sha512-hoalWLoI8r4UszCkZ5kL8vayOGVae1oxXe/2A4AO6J9+580uKHDO3JdHb7NzwwzK5xr/Fs0W40kiNHxM9vyTtQ=="
		crossorigin=""/>
		<!-- Make sure you put this AFTER Leaflet's CSS -->
		<script src="https://unpkg.com/leaflet@1.8.0/dist/leaflet.js"
		integrity="sha512-BB3hKbKWOc9Ez/TAwyWxNXeoV9c1v6FIeYiBieIWkpLjauysF18NzgR1MBNBXf8/KABdlkX68nAhlwcDFLGPCQ=="
		crossorigin=""></script>
		<style>
			html, body {
				height: 100%;
				margin: 0;
			}
			.leaflet-container {
				height: 400px;
				width: 600px;
				max-width: 100%;
				max-height: 100%;
			}
		</style>
	</head>
	<body>
		<div id="map"></div>
	</body>
</html>
```

This will give you an empty map, as we have not add a base map or any other layers. To do this, we need to declare a map variable, this needs to be done within JS. This variable will have a **.setView** method that allow us to point the map (or view) to a specific location, and also set the [zoom level](https://wiki.openstreetmap.org/wiki/Zoom_levels). 

Next, we need to add an actual base map. An example of a base map would be [OpenStreetMap](https://www.openstreetmap.org) (OSM). xyz tiles are raster tiles of a base map that is tiled, which allows you to load certain tiles for a specific area at a certain zoom level. Here is a nice list of different options, https://github.com/roblabs/xyz-raster-sources Below is an example of the script tag you need to display the OSM base map. 

```html
<script>
	var map = L.map('map').setView([-25.754544, 28.231482], 9);
	
	var tiles = L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
			maxZoom: 19,
			attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
	}).addTo(map);
</script>
```

## Loading a local GeoJSON onto your map

## Adding a layer controller with multiple base maps

## Displaying a WMS layer

The [web map service (WMS)](https://www.notion.so/Geoserver-7679281804704ac3a2ab27dc2a1e4cbf) format is similar to those of map tiles used for base maps, thus we can use the L.tileLayer function again, but more specifically the [WMS extended](https://leafletjs.com/reference.html#tilelayer-wms) one. This allows you to specify the URL of your server, such as the GeoServer, and the options/variables of the WMS request (e.g. layer, type of image, and styling). The Leaflet JS documentation will describe the options available and show you an example of how to use it, [https://leafletjs.com/reference.html#tilelayer-wms](https://leafletjs.com/reference.html#tilelayer-wms)

Generally the code for adding a WMS layer would look something like this:

```jsx
var mylayer = L.tileLayer.wms("[http://mygeoserver.co.za:8080/geoserver/workspace/wms](http://mesonet.agron.iastate.edu/cgi-bin/wms/nexrad/n0r.cgi)", {
	layers: 'workspace:mylayer',
	format: 'image/png',
	transparent: true,
	attribution: "Awesome data © 2022 University of Pretoria"
}).addTo(map);
```

To get these options, I would recommend using your GeoServer layer preview, and selecting the format you prefer and then opening that request in an application like [Postman](https://www.postman.com/) to inspect the variable. This will give you an idea of what is required, and what variables to use. 

The last line contains the method .**addTo(map)**, this adds the layer to your map. If you don’t add this, you will not be able to view the layer. 

## Displaying a WFS layer

This process is a bit more complicated that the WMS as Leaflet does not have an explicit process to use a WFS layer. Thus, the process is a bit longer. The first step would be to build the request you want to send to the GeoServer. This can be done by manually building the URL. Below is an example. Similar to the WMS, use something like [Postman](https://www.postman.com) to help you with this. 

```jsx
// Specify your root URL
var owsrootUrl = '[http://mygeoserver.co.za:8080/geoserver/workspace/ows](http://mesonet.agron.iastate.edu/cgi-bin/wms/nexrad/n0r.cgi)';

// Define your paramenters
var defaultParameters = {
				service : 'WFS',
        version : '1.0.0',
        request : 'GetFeature',
        typeName : 'workspace:mylayer',
        outputFormat : 'text/javascript',
        format_options : 'callback:getJson',
        SrsName : 'EPSG:4326'
};

/* Combine all of this into a correctly formatted URL
Refer to this page of the Docs, https://leafletjs.com/reference.html#util */
var parameters = L.Util.extend(defaultParameters);
var URL = owsrootUrl + L.Util.getParamString(parameters);

```

We now have a URL, but we need to send it to GeoServer and handle the response. We can do this by using [Ajax](https://en.wikipedia.org/wiki/Ajax_(programming)). Ajax allows us to do a HTTP request asynchronously, meaning your code will continue to run while you are waiting for the response, but once you have the data your callback function will run. We will use the jQuery library to do our Ajax request. 

The first step is to add the library to your web application. You can do it similar to how we added Leaflet JS. We will use the content distribution network (CDN) option as to avoid downloading the files. You can find the information [here](https://jquery.com/download/), and an example is below (which should be added in the head of your HTML. 

```jsx
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
```

The general structure of the code is explained below. 

```jsx
// create an empty layer
var WFSLayer = null;

// this is the ajax request, we are using the jsonp option
var ajax = $.ajax({
		url : URL,
		dataType : 'jsonp',
		/* this is the callback function that needs to run once the data is received,
		we are basically telling it to treat the data as a JSON */
		jsonpCallback : 'getJson',
		/* if everything works fine, we do the success function, you can also 
		add a error clause to execute if there was a problem */
		success : function (response) {
			//. this is the general Leaflet L.geoJson function, https://leafletjs.com/reference.html#geojson
			WFSLayer = L.geoJson(response, {
					style: function (feature) {
							return {
									//add you styling here
							};
					},
			}).addTo(map);
		}
});
```
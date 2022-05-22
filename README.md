# Mapping Earthquakes with JS and APIs

## Overview

### To create filterable maps based on stylized imagery (Street View, Satellite View, Dark Mode) and selected data layers (Earthquakes Occurring
Over The Past Week, Major Earthquakes Occurring over the Last Week, Tectonic Plate Outlines) in an HTML-based webpage. 

## Results

### Deliverable 1: Tectonic Plates

Having already created a map showcasing ``Earthquakes Over The Last 7 Days``, we first decided to add a new layer that would display the ``Tectonic Plates`` and how these earthquakes aligned with said faults lines. To create this new layer we had to do the following

    // Initialize New Layer
    let tectonic = new L.LayerGroup();
    
    // Reference New Layer To Overlayed Object (Join Our New Layer To The Words Appearing Onscreen)
    let overlays = {
        "Tectonic Plates": tectonic
    };
    
    // Make A Call To Our Data Source To Retrieve geoJSON Data
    d3.json("https://raw.githubusercontent.com/fraxen/tectonicplates/master/GeoJSON/PB2002_boundaries.json").then(function(data) {
      L.geoJSON( data,
      
    // Set Color And Width For Visual Appeal
        {color: "#ff0000",
         weight: 2.5,
    
    // Add Our New Layer To Our Webpage
      }).addTo(tectonic)

### Deliverable 2: Major Earthquakes

Once We completed our first new layer, we then took on the task of creating a third layer centered on only the ``Most Severe Earthquakes`` (scoring a 4.5 or greater on the Ritcher Scale over the past 7 days.) To create this chart, we followed the process established in deliverable one, as seen below. 

    // Initialize New Layer
    let major = new L.LayerGroup();
    
    // Reference New Layer To Overlayed Object (Join Our New Layer To The Words Appearing Onscreen)
    let overlays = {
      "Major Earthquakes": major
    };
    
    // Make A Call To Our Data Source To Retrieve geoJSON Data
    d3.json("https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/4.5_week.geojson").then(function(data) {
    
    // Use The Same Style As The Earthquake Map
    function styleInfo(feature) {
      return {
        opacity: 1,
        fillOpacity: 1,
        fillColor: getColor(feature.properties.mag),
        color: "#000000",
        radius: getRadius(feature.properties.mag),
        stroke: true,
        weight: 0.5
      };
    }
    
    // Set Three Colors For Map Markers
      function getColor(magnitude) {
      
    // One Color For Earthquakes With A Magnitude Greater Than 6
    if (magnitude > 6) {
      return "#ea2c2c";
    }
    
    // One Color For Earthquakes With A Magnitude Greater Than 5
    if (magnitude > 5) {
      return "#ea822c";
    }
    
    // One Color For Earthquakes With A Magnitude Less Than 5
    if (magnitude < 5) {
      return "#ee9c00";
    }}
    
    // Implement A Function To Make The Marker Radius Proportional To Magnitude
    function getRadius(magnitude) {
      if (magnitude === 0) {
        return 1;
      }
      return magnitude * 4;
    }

    // Create geoJSON Layer With Retrieved Data
    L.geoJson(data, {
    
    	// Create Circle Marker On Map
    	pointToLayer: function(feature, latlng) {
      		console.log(data);
      		return L.circleMarker(latlng);
        },
        
      // Set Style For Circle Marker With Style Function
    style: styleInfo,
    
     // Create A PopUp For Each Circle Marker Displaying Magnitude And Location of Earthquake 
     onEachFeature: function(feature, layer) {
      layer.bindPopup("Magnitude: " + feature.properties.mag + "<br>Location: " + feature.properties.place);
    }
    
    // Add Our New Layer To Our WebPage
    }).addTo(major);   
    
### Deliverable 3: Dark Mode

Finally, our last task saw us add a new map style to the webpage by creating a third tile layer (``Dark Mode``) and adding that layer to our base layer.

    // Create A Third Tile Layer 
    let dark = L.tileLayer('https://api.mapbox.com/styles/v1/mapbox/dark-v10/tiles/{z}/{x}/{y}?access_token={accessToken}', {
	  attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-  
    sa/2.0/">CC-BY-SA</a>, Imagery (c) <a href="https://www.mapbox.com/">Mapbox</a>',
	  maxZoom: 18,
	  accessToken: API_KEY
    });
    
    // let baseMaps = {
      "Dark": dark};
    
## Summary

Ultimately, using ``JavaScript`` and ``APIs`` can be an effective methodology for analyzing and visualizing data on an HTML-based webpage. While using these methods may initially appear overwhelming or challenging to new users; once its repetitive framework has been established, said user can easily refactor scripts, needing only to repurpose snippets of code.

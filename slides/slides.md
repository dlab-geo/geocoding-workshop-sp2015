% title: Introductory Geocoding
% subtitle: Photon and geopy
% author: Juan Shishido, Andrew Chong 
% author: School of Information
% author: GSR, D-Lab
% thankyou: Thanks!
% contact: <a href="http://people.ischool.berkeley.edu/~juanshishido/">Web</a>
% contact: <a href="https://github.com/juanshishido">GitHub</a>
% contact: <a href="http://www.linkedin.com/in/juanshishido/">LinkedIn</a>
% favicon: http://dlab.berkeley.edu/sites/all/themes/planetta/favicon.ico

---
title: Purpose
build_lists: false

Obtain coordinates for: street addresses, intersections, place names, or zip codes

Enables

- Mapping of addresses
- Linking with other geospatial data
- Spatial calculations, such as distances

---
title: Process
build_lists: false

In general, these are the steps in a geocoding project

- Identify needs
- Choose geocoder
- Preprocess data
- Geocode
- Verify output
- Postprocess data

---
title: Considerations
build_lists: false

Think about

- Cost
- Geographic scope
- Output quality
- Speed
- Scale

Two primary options: local and remote

---
title: Local Options
build_lists: false

In some cases, you may want or need to use a local geocoder

- Data is confidential or restricted use
- Too many addresses for remote

Software options: ArcGIS, Postgres/PostGIS

---
title: Remote Options
build_lists: false

Many options

- Google Maps Geocoding API
- SmartyStreets
- ArcGIS
- Nominatim
- DSTK
- Photon
- geopy

---
title: Remote Options
build_lists: false

Vary based on

- Usage limits
- Methods of use
- Output quality
- Coverage

---
title: Data Science Toolkit
class: segue dark nobackground

---
title: Google Maps Geocoding API 
build_lists: false

<a href="https://developers.google.com/maps/documentation/geocoding/intro" target="_blank">Google Maps Geocoding API </a> has high accuracy but has usage limits: 

- 2,500 free requests per day, 10 per second 
- $0.50/1000 requests, up to 100,000 daily 

---
title: Street Address to Coordinates

```/street2coordinates```

Pass in the address as a parameter

    https://maps.googleapis.com/maps/api/geocode/json?address=<_your address_>

---
title: Street Address to Coordinates

```/street2coordinates```

Pass in the address as a parameter

    https://maps.googleapis.com/maps/api/geocode/json?address=
    1600 Amphitheatre Pkwy, Mountain View, CA

---
title: Street Address to Coordinates

<pre class="prettyprint" data-lang="JSON">
{ "results" : ...
"geometry" : {
            "location" : {
               "lat" : 37.422245,
               "lng" : -122.0840084
            },
            "location_type" : "ROOFTOP",
            "viewport" : {
               "northeast" : {
                  "lat" : 37.42359398029149,
                  "lng" : -122.0826594197085
               },
               "southwest" : {
                  "lat" : 37.4208960197085,
                  "lng" : -122.0853573802915
               }
            }
         }...
}
</pre>

---
title: Evaluate

<img height=auto width=50% src="figures/google_hq_dstk_coords.png"><img height=auto width=50% src="figures/google_hq_address.png">

---
title: Try It
build_lists: false

    https://maps.googleapis.com/maps/api/geocode/json?address=<_your address_>

What happens when

- State is omitted
- Zip code is omitted
- Commas are removed
- Mix case

---
title: Photon
class: segue dark nobackground

---
title: Photon
build_lists: false

<a href="http://photon.komoot.de/" target="_blank">Photon</a> is free and open source

- Uses <a href="http://www.openstreetmap.org/" target="_blank">OpenStreetMap</a> data
- Worldwide coverage
- Multilingual search
- Typo tolerance
- Fast & scalable

However, "extensive usage will be throttled"

---
title: Photon API

Search

    photon.komoot.de/api/?q=berkeley

Limit number of results

    photon.komoot.de/api/?q=berkeley&limit=1

Preferred language

    photon.komoot.de/api/?q=berkeley&lang=fr

---
title: Photon API

Pass in the address as a parameter

    photon.komoot.de/api/?q=
    1600 Amphitheatre Pkwy, Mountain View, CA


---
title: Photon API

<pre class="prettyprint" data-lang="JSON">
{"features": [{
  "properties": {
    "osm_key":"office",
    "street":"Amphitheatre Parkway",
    "name":"Google Headquaters",
    "osm_id":2192620021,
    "osm_type":"N",
    "osm_value":"commercial",
  },
  "type":"Feature",
  "geometry": {
    "type":"Point",
    <b>"coordinates":[-122.0850862,37.4228139]</b>
  }
}],
"type":"FeatureCollection"}
</pre>

<a href="http://photon.komoot.de/api/?q=1600%20Amphitheatre%20Pkwy,%20Mountain%20View,%20CA" target="_blank">Full output</a>

---
title: Evaluate

<img height=auto width=50% src="figures/google_hq_photon_coords.png"><img height=auto width=50% src="figures/google_hq_address.png">

---
title: Bonus: geopy
class: segue dark nobackground

---
title: geopy
build_lists: false

Geocoding with Python

Access to many geocoding services

  - OpenStreetMap Nominatim
  - ESRI ArcGIS
  - Google Geocoding API
  - Baidu Maps
  - Bing Maps

<a href="https://github.com/geopy/geopy" target="_blank">geopy on GitHub</a>

---
title: geopy
build_lists: false

Example from the docs

    $ pip install geopy

<pre class="prettyprint" data-lang="python">
>>> import geopy
>>> from geopy.geocoders import Nominatim
>>> geolocator = Nominatim()
>>> location = geolocator.geocode("1600 Amphitheatre Pkwy, Mountain View, CA")
>>> print((location.latitude, location.longitude))
<b>(37.4228139, -122.0850862)</b>
</pre>

You can also reverse geocode, calculate distances, and more

Check out the geopy <a href="http://geopy.readthedocs.org/en/latest/" target="_blank">documentation</a>

---
title: JSON
class: segue dark nobackground

---
title: JSON Output
build_lists: false

JavaScript Object Notation

- Format typically used to send data between a server and web app

Convert JSON to CSV

- Write a script in Python, R, etc.
- Use other modules, e.g., ```pandas```

---
title: JSON to CSV

<pre class="prettyprint" data-lang="python">
import pandas as pd

# load JSON as DataFrame
json_data = pd.read_json("coordinates.json").T

# to CSV
json_data.to_csv("geocoded.csv")
</pre>

Note: ```.T``` transposes the DataFrame

---
title: Problems in Reference Data 
build_lists: false 

Varies over services (remote & local)

- Incorrect street ranges, inaccurate or low quality features 
- Inaccurate feature attributes

Year matching between geocoded and reference data

- Missing streets
- Address changes


---
title: Verify Output
build_lists: false

Ways to assess quality

- Compare input street name to ```street_name```
- Count missing values
- Test against sub-sample with known or high-quality coordinates 

Because results are based on an underlying database or interpolation method, there will be variation in coordinate quality. In cases where the results are not good enough, consider using another service for those addresses.

---
title: Postprocess

To GeoJSON

Link to Census Blocks

    http://data.fcc.gov/api/block/find?latitude=<_latitude_>&longitude=<_longitude_>

Block FIPS="<b>060855046011175</b>"

The first two characters (<b>06</b>) indicate the <b>state</b> (CA), the next three (<b>085</b>) indicate the <b>county</b> (Alameda), the next 6 indicate the <b>census tract</b> (<b>5046.01</b>) and the last four characters indicates the census <b>block</b> group and block number (<b>1175</b>).  The first digit of the block identifies the block group.

---
title: Mapping

Several options

- Leaflet
- geojson.io
- CartoDB
- ArcGIS/QGIS
- GeoCanvas
- Python

---
title: Best Practices
build_lists: false

Preprocess data

- Formatting
- Components

Sample and test

Use multiple sources

Map results to verify

---
title: Tutorial
build_lists: false

Clone the repo or download the zip file from:

<a href="http://bit.ly/dlab-geocoding" target="_blank">bit.ly/dlab-geocoding</a>

Navigate to the directory and start an IPython notebook instance

    $ ipython notebook

Let's get to work

We'll create a map of the 44 BART stations in the Bay Area
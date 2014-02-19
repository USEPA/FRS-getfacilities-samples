##  FRS GetFacilities REST API - Code Samples
![](/http://www.epa.gov/enviro/html/fii/img/data-stewards.jpg)

This repository contains sample HTML/JavaScript code to demonstrate the use of the EPA Facility Registry Service (FRS) [GetFacilities REST API](http://www.epa.gov/enviro/html/fii/FRS_REST_Services.html).  The GetFacilities REST API can be used for a number of different types of applications, whether performing a radial search for EPA regulated facilities, or querying for facility information based on name and location, spatial query by radial search, or other types of applications.  These have initially been developed to support the 2014 Exchange Network Conference, as simple code samples developed in a few hours and a few dozen lines of JavaScript.

These code samples are each self-contained HTML pages demonstrating JavaScript service calls using the [jQuery](http://jquery.com/) library to simplify calling the REST service and working with the response.  For demonstration purposes, code samples are provided using free/open source libraries, such as jQuery and [leaflet.js](http://leafletjs.com/) and no API keys are needed - however these can generally be easily adapted for Bing maps or other types of APIs, libraries and applications.

The REST API can return responses in XML, JSON and JSONP.  With jQuery, JSONP is an ideal choice as JavaScript can work natively with the returned objects.

## [FormLookup.html](https://github.com/USEPA/FRS-getfacilities-samples/blob/master/FormLookup.html)
This sample code demonstrates how a web form for entering facility data can be augmented and extended using the GetFacilities REST service to perform a search.  (Additionally, this sample also includes an example of how results can be displayed using [leaflet.js](http://leafletjs.com/) and [OpenStreetMap](http://www.openstreetmap.org/) tiles.)

**Live Demo**
[(link to live demo)](http://druidsmith.github.io/demo/FormLookup.html)

In the live demo, a sample query to try might be "3M, Cottage Grove MN" - which should retrieve information for the 3M plant located there.  

**Behind the scenes**

The entered parameters are taken and a query is built in the form of http://ofmpub.epa.gov/enviro/frs_rest_services.get_facilities?facility_name=3M&city_name=Cottage%20Grove&state_abbr=MN&output=JSONP
jQuery helps to retrieve any entered form fields for the search parameters, and then populates the form with the returned values for the selected facility.

## [MapDemo.html](https://github.com/USEPA/FRS-getfacilities-samples/blob/master/MapDemo.html)

This sample code demonstrates how a spatial query can be performed by providing a latitude/longitude, and user specified radius (in miles).  The user can also select which [program system](http://www.epa.gov/enviro/html/fii/data_sources.html) to query on (by acronym).  This code sample also uses leaflet.js and openstreetmap.

**Live Demo**
[(link to live demo)](http://druidsmith.github.io/demo/MapDemo.html)

In the live demo, you can drag the map to pan or use the navigation or mouse scroll to zoom in and out - then, pick a program acronym from the dropdown, enter a desired radius in miles, and then click on the map to query.  A query to try might be [CERCLIS](http://www.epa.gov/enviro/facts/cerclis/index.html) facilities within 4 miles of a given location selected on the map.

**Behind the scenes**

This demo uses the leaflet.js library to render the map, using OpenStreetMap tiles, and then uses functions from leaflet to get the latitude and longitude.  There are generally very similar corresponding functions in other map APIs such as Bing Maps, ESRI Javascript API or Google Maps.  The code sample also uses leaflet to draw the circle specifying the search area (this is converted from miles to km).  The query is then constructed from those parameters, for example http://ofmpub.epa.gov/enviro/frs_rest_services.get_facilities?latitude83=38.8&longitude83=-77.01&search_radius=4&pgm_sys_acrnm=CERCLIS&output=JSONP

Note:  The REST API was designed to use NAD83 lat/long in accordance with [EPA locational standards](http://iaspub.epa.gov/sor_internet/registry/datastds/findadatastandard/epaapproved/latitudelongitude/LatLongStandard_08112006.pdf), however in most cases this will be quite close to WGS84 lat/longs.

## [SysIDQuery.html](https://github.com/USEPA/FRS-getfacilities-samples/blob/master/SysIDQuery.html)

This sample code demonstrates how a system ID can be used to query FRS and retrieve information - as an example, a state identifier or other system ID such as Department of Energy EIA-860 ORIS power plant ID.

**Live Demo**
[(link to live demo)](http://druidsmith.github.io/demo/SysIDQuery.html)

In the live demo, you can select a system to query by using the dropdown, and then enter an ID to query.  For example, you can select Minnesota's MN-DELTA system and try the MN-DELTA ID 117212.

**Behind the scenes**

This sample code simply constructs a query using the specified parameters and retrieves the associated facility information.  
http://ofmpub.epa.gov/enviro/frs_rest_services.get_facilities?pgm_sys_acrnm=MN-DELTA&pgm_sys_id=117212&output=JSONP
Note in particular that things like the FRS RegistryID returned can be used to access a wide variety of other information, such as EPA Enforcement and Compliance History Reports or for Envirofacts data queries.
Additionally, additional detailed program output can be retrieved by adding &program_output=Yes to the query parameters.

## EPA Disclaimer
The United States Environmental Protection Agency (EPA) GitHub project code is provided on an "as is" basis and the user assumes responsibility for its use. EPA has relinquished control of the information and no longer has responsibility to protect the integrity , confidentiality, or availability of the information. Any reference to specific commercial products, processes, or services by service mark, trademark, manufacturer, or otherwise, does not constitute or imply their endorsement, recomendation or favoring by EPA. The EPA seal and logo shall not be used in any manner to imply endorsement of any commercial product or activity by EPA or the United States Government.


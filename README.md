##  FRS GetFacilities REST API - Code Samples

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
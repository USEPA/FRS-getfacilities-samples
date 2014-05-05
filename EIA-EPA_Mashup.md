## Collection of EPA / DOE Power Generation Facility Mashup Tips

![](http://upload.wikimedia.org/wikipedia/commons/e/e2/DTE_St_Clair.jpg)

This document presents some suggestions for mashing up Department of Energy data (EIA-860 Power Generation) with EPA data from EPA websites and APIs. Use cases could include using EPA's spatial search capability to augment and extend the EIA API in searching for power plants, or in connecting EPA data like emissions and greenhouse gas data to EIA power generation data.  An EPA API that can help this is the Facility Registry Service (FRS) API.

_New addition as of May 2014:  Ability to retrieve FRS facility detail reports using EIA-860 ORIS IDs or other identifiers - see below for details_

### Key Linkage:  How to connect between EPA facility data and Department of Energy ORIS IDs and EIA data

EPA integrates EIA-860 power plant data in its [Facility Registry Service (FRS)](http://epa.gov/frs) on an annual basis.  As such, the EIA-860 ORIS identifiers are available there.  FRS integrates facility data across 90 different datasets to provide a means of linking and crosswalking these datasets.  This can typically be done via the EPA Registry ID.

### FRS Facility Lookup REST Service

FRS has a [Facility Lookup REST Service](http://www.epa.gov/enviro/html/fii/FRS_REST_Services.html) which can be queried using program IDs.  

### Basic Example

Example:  The Keystone Power Plant in Shelocta, PA has an EIA-860 ORIS ID of 3136.
This can be queried via the FRS Facility Lookup Service as follows (simplest example, with defaults):

[http://ofmpub.epa.gov/enviro/frs_rest_services.get_facilities?pgm_sys_acrnm=EIA-860&pgm_sys_id=3136](http://ofmpub.epa.gov/enviro/frs_rest_services.get_facilities?pgm_sys_acrnm=EIA-860&pgm_sys_id=3136)

Deconstructing this call, the key elements and parameters are
- endpoint: http://ofmpub.epa.gov/enviro/frs_rest_services.get_facilities
- Program System Acronym is 'EIA-860': pgm_sys_acrnm=EIA-860
- Program System ID is '3136': pgm_sys_id=3136

The default response type is XML

### Useful Parameters

To specify JSON or JSONP, simply append this request as an output parameter using an ampersand, example:

http://ofmpub.epa.gov/enviro/frs_rest_services.get_facilities?pgm_sys_acrnm=EIA-860&pgm_sys_id=3136&output=JSON

To request additional EPA program outputs, add '&program_output=yes' as a parameter, example:

[http://ofmpub.epa.gov/enviro/frs_rest_services.get_facilities?pgm_sys_acrnm=EIA-860&pgm_sys_id=3136&program_output=yes&output=JSON](http://ofmpub.epa.gov/enviro/frs_rest_services.get_facilities?pgm_sys_acrnm=EIA-860&pgm_sys_id=3136&program_output=yes&output=JSON)

Program output may be useful for accessing some reports and data, for example accessing EPA E-GGRT greenhouse gas data.  This will be shown in more detail below.

### Useful Outputs

Note that the service returns many core facility elements that may be useful in applications, for example latitude and longitude:

> "Latitude83":"40.659509",
"Longitude83":"-79.337818"

These can be useful for displaying a simple map, for example using [Leaflet.js](http://leafletjs.com/).

However, the one attribute that can be very powerful in terms of mashups and unlocking a lot of additional data is the **FRS Registry ID**

> "RegistryId":"110000538525",

### Using the Registry ID to access an FRS Facility Detail Report

This Registry ID can be used to retrieve a number of EPA reports by simply plugging in the ID into the URL, for example an FRS Facility Detail Report:

http://iaspub.epa.gov/enviro/fii_query_detail.disp_program_facility?p_registry_id=110000538525

### Using a Program ID (such as an EIA ORIS ID) to access an FRS Facility Detail Report

This is a new feature, added to the FRS Facility Detail Reports in May of 2014.  Now, in addition to using an FRS Registry ID, other types of IDs (such as an EIA ORIS ID, RCRA Handler ID, NPDES Permit, et cetera) can be used to retrieve FRS Facility Detail Reports:

http://ofmpub.epa.gov/enviro/fii_query_detail.disp_program_facility?pgm_sys_acrnm_in=EIA-860&pgm_sys_id_in=599

It uses the parameters **pgm_sys_acrnm_in=[program acronym]** to specify the program (in this case, EIA-860) and **pgm_sy_id_in=NNNN** to specify the identifier, in this case 'NNNN' is the EIA ORIS ID.

### EPA ECHO Enforcement and Compliance Report

EPA ECHO Enforcement and Compliance Reports can also be retrieved using the FRS Registry ID, using the parameter **fid=[FRS Registry ID]** - example:

http://echo.epa.gov/detailed_facility_report?fid=110000538525

### EPA Envirofacts Detailed Report

Similarly, FRS Regstry ID can be used to retrieve Envirofacts detail reports, using the parameter **facility_uin=[FRS Registry ID]** - example:

http://oaspub.epa.gov/enviro/multisys2_v2.get_list?facility_uin=110000538525

### Greenhouse Gas Report

For some EPA reports, a program ID may be needed, for example, to retrieve information from EPA's Greenhouse Gas reporting program, the E-GGRT ID is needed.  This can be found when the program system output parameter is specified in the FRS REST Lookup, '&program_output=yes' which will return program ID, i.e.

> "ProgramSystemAcronym":"E-GGRT",
"ProgramSystemId":"1000883",
"ProgramFacilityName":"KEYSTONE"

This greenhouse gas ID can then be used to link to greenhouse gas reports in the ghgdata.epa.gov site, for example

http://ghgdata.epa.gov/ghgp/service/html/2012?id=1000883

This call will retrieve a snippet of HTML containing detailed information about greenhouse gas emissions for this plant

### Accessing Data via Envirofacts REST API

EPA's Envirofacts data warehouse additionally contains significant data resources, which can be accessed via a [REST API](http://www.epa.gov/enviro/facts/services.html)

This Envirofacts REST API utilizes canonic calls which can access data by specifying the table of interest and parameters for retrieval, based on the [Envirofacts Data Model](http://www.epa.gov/enviro/facts/efovw.html)

In a similar manner to the reports above, the facility identifiers can be used to unlock data from the Envirofacts REST API.

### Starting with Geography and getting to Energy Generators

The EPA Facility Lookup REST Service can also allow you to query via geography, for example, 'Shelocta, PA' or 'Indiana County, PA' or by coordinates

"Search for EIA-860 facilities in Shelocta, PA"

http://ofmpub.epa.gov/enviro/frs_rest_services.get_facilities?state_abbr=PA&city_name=Shelocta&pgm_sys_acrnm=EIA-860&program_output=yes&output=json

"Search for EIA-860 facilities in Indiana County, PA"

http://ofmpub.epa.gov/enviro/frs_rest_services.get_facilities?state_abbr=PA&county_name=Indiana&pgm_sys_acrnm=EIA-860&program_output=yes&output=json

"Search for EIA-860 facilities within a 10 mile radius of 46.65,-79.33"

http://ofmpub.epa.gov/enviro/frs_rest_services.get_facilities?latitude83=40.65&longitude83=-79.33&search_radius=10&pgm_sys_acrnm=EIA-860&output=json

### Conclusion

I would be interested in seeing what, if anything, people build using some of these tips.

I will try and post additional tips and info as available - and if you have any questions I can be reached various ways:

- Twitter - [@druidsmith](twitter.com/druidsmith)
- Email - [smith (dot) davidg @ epa (dot) gov](mailto:smith.davidg@epa.gov)

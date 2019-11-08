# Getting started with IUDX

IUDX consists of following components

- Catalogue 
  - Enables search of data-resources 
  - Provides meta-information about data-resources
- Resource Server 
  - Provides access of data from data-resources
- Authorisation server 
  - Manages authorisation to access the data-resources

This document provides examples to get one stared quickly on using IUDX. The examples below are for the Pune instance of IUDX, also referred to as PUDX.


## Information Resources
<Link to IUDX Documents>
<Link to pyIUDX>
<Link to Jupyter Notebook>
<Link to API docs>

## Catalogue APIs

- Search a set of resources using attribute search. For example, search using 'tags' attribute to search for all resources with 'pollution' tag.
curl -X GET  "https://pudx.catalogue.iudx.org.in/catalogue/v1/search?attribute-name=(tags)&attribute-value=(pollution)"  -H 'Accept: application/json'
  - Replace 'search' with 'count' in the URL-path above above to get count of all found resource items
curl -X GET  "https://pudx.catalogue.iudx.org.in/catalogue/v1/count?attribute-name=(tags)&attribute-value=(pollution)"  -H 'Accept: application/json'
  - Get only some fields from the catalogue item by specifying an attribute filter. For example, only get "NAME" and "id" attributes for the found items
curl -X GET  "https://pudx.catalogue.iudx.org.in/catalogue/v1/search?attribute-name=(tags)&attributelue=(pollution)&attribute-filter=(id,NAME)"  -H 'Accept: application/json'

- Search a set of resources using simple proximity search
curl -X GET  "https://pudx.catalogue.iudx.org.in/catalogue/v1/search?lat=18.528311&lon=73.874537&radius=2000"  -H 'Accept: application/json'
   - 'lat', 'lon' are the latitude and longitude values of the centre point. 'radius' is specified in meters.

- List all items in catalogue
curl -X GET   "https://pudx.catalogue.iudx.org.in/catalogue/v1/search"  -H 'Accept: application/json' -H 'Cache-Control: no-cache'

Now that we have got started on IUDX Catalogue, refer to the full [API specifications](https://apidocs.iudx.org.in/cat) for more detailed information

## Resource Server APIs
 - Get latest data from the resource. 
   - Uses the "id" attribute which can be read from corresponding catalogue item
   ```bash
   curl -X POST \
   https://pudx.resourceserver.iudx.org.in/resource-server/pscdcl/v1/search \
  -H 'Accept: */*' \
  -H 'Content-Type: application/json' \
  -d '{
        "id": "rbccps.org/aa9d66a000d94a78895de8d4c0b3a67f3450e531/pudx-resource-server/aqm-bosch-climo/Susgaon_46",
        "options": "latest"
   }'
   ```

 - Get data for a given resource during a given interval.
   ```bash
   curl -X POST \
   https://pudx.resourceserver.iudx.org.in/resource-server/pscdcl/v1/search \
   -H 'Content-Type: application/json' \
   -d '{
       "id": "rbccps.org/aa9d66a000d94a78895de8d4c0b3a67f3450e531/pudx-resource-server/aqm-bosch-climo/Susgaon_46",
       "time": "2019-10-30T09:30:00.000+05:30/2019-10-30T12:00:00.000+05:30",
       "TRelation": "during"
   }'
   ```
Detailed resource server API specifications can be found [here](https://apidocs.iudx.org.in/rs) 


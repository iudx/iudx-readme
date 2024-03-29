# Getting started with IUDX

IUDX consists of following components

- Catalogue 
  - Enables search of data-resources 
  - Provides meta-information about data-resources
- Resource Server 
  - Provides access of data from data-resources
- Authorisation server 
  - Manages authorisation to access the data-resources

This document provides examples to get you stared quickly on using IUDX. The examples below are for the Pune instance of IUDX, also referred to as PUDX.

## Catalogue APIs

- Search a set of resources using attribute search. For example, search using 'tags' attribute to search for all resources with the 'pollution' tag.
```bash
curl -X GET  "https://pudx.catalogue.iudx.org.in/catalogue/v1/search?attribute-name=(tags)&attribute-value=(pollution)"  -H 'Accept: application/json'
```
  - Replace 'search' with 'count' in the URL-path above above to get count of all found resource items

```bash
curl -X GET  "https://pudx.catalogue.iudx.org.in/catalogue/v1/count?attribute-name=(tags)&attribute-value=(pollution)"  -H 'Accept: application/json'
```
  - Get only some fields from the catalogue item by specifying an attribute filter. For example, only get "NAME" and "id" attributes for the found items
```bash
curl -X GET  "https://pudx.catalogue.iudx.org.in/catalogue/v1/search?attribute-name=(tags)&attributelue=(pollution)&attribute-filter=(id,NAME)"  -H 'Accept: application/json'
```

- Search a set of resources using simple proximity search
```bash
curl -X GET  "https://pudx.catalogue.iudx.org.in/catalogue/v1/search?lat=18.528311&lon=73.874537&radius=2000"  -H 'Accept: application/json'
```
   - 'lat', 'lon' are the latitude and longitude values of the centre point. 'radius' is specified in meters.

- List all items in catalogue
```bash
curl -X GET   "https://pudx.catalogue.iudx.org.in/catalogue/v1/search"  -H 'Accept: application/json' -H 'Cache-Control: no-cache'
```

Now that we have got started on IUDX Catalogue, refer to the full [API specifications](https://apidocs.iudx.org.in/cat) for more detailed information. For advance usage refer to [IUDX Python SDK](https://github.com/iudx/pyIUDX).

## Resource Server APIs
 - Get latest data from the resource. 
   - Uses the "id" attribute which can be read from corresponding catalogue item
   ```bash
   curl -X POST \
   https://pudx.resourceserver.iudx.org.in/resource-server/pscdcl/v1/search \
   -H 'Accept: \*/\*' \
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
Detailed resource server API specifications can be found [here](https://apidocs.iudx.org.in/rs). For advance usage on python refer to [IUDX Python SDK](https://github.com/iudx/pyIUDX).
### Note
- For certain temporal queries, e.g., "TRelation" equal to "during" or "before", to limit the response data size, there may be a restriction on the allowed time interval for which response data is provided. An error code of "416" will indicate the exceeded limit along with the maximum allowed time interval (in days).


## Authentication Authorization and Accounting AAA server 

 - To get a digital certificate, please visit the [IUDX CA](https://ca.iudx.org.in).
 - To know about the AAA setup and APIs please visit the [Auth Server](http://auth.iudx.org.in) 

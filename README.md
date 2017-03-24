# JSON-qb REST API

Implements the JSON-qb API specification. It aims to provide an easy to use API for developers that reuse statistical data stored in the form of RDF Data cubes. The API implementation can be installed on top of any RDF repository and offer basic and advanced operations on RDF Data Cubes.

## Explore cubes

### GET cubes

Parameter: none

Description: returns all the available cubes of an RDF repository (including the aggregated cubes created by the Data Cube Aggregator)

Example request:

GET [http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/cubes](http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/cubes)


Example result

```
{
  "cubes": [
    {
      "@id": "http://id.vlaanderen.be/statistieken/dq/kubus-gemiddelde-prijs#id",
      "label": "Cube average price real estate"
    },
    {
      "@id": "http://id.vlaanderen.be/statistieken/dq/kubus-gemiddelde-prijs-1993#id",
      "label": "Cube average price real estate (1993-2013)"
    },
    {
      "@id": "http://id.vlaanderen.be/statistieken/dq/kubus-bouwvergunningen#id",
      "label": "Cube building permits"
    },...
  ]
}
```

### GET aggregationSetcubes

Parameter: none

Description: returns only the initial cubes, not including the aggregated cubes created by the Data Cube Aggregator

Example request:

GET [http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/aggregationSetcubes](http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/aggregationSetcubes)

Example result

```
{
  "cubes": [
    {
      "@id": "http://id.vlaanderen.be/statistieken/dq/kubus-gemiddelde-prijs#id",
      "label": "Cube average price real estate"
    },
    {
      "@id": "http://id.vlaanderen.be/statistieken/dq/kubus-gemiddelde-prijs-1993#id",
      "label": "Cube average price real estate (1993-2013)"
    },
    {
      "@id": "http://id.vlaanderen.be/statistieken/dq/kubus-bouwvergunningen#id",
      "label": "Cube building permits"
    },...
  ]
}
```

## Aggregations

### GET create-aggregations

Parameters:
* dataset (required)
* A mapping of each cube measure with an aggregation function (supported functions: AVG, SUM, MIN, MAX, COUNT)

Description: computes aggregations of existing cubes using the specified aggregate functions. All possible dimension combinations are used to create aggregations. For example if a cube has three dimensions (e.g. time, geographical location, sex) the Data Cube Aggregator will compute aggregations and create new cubes with one and two dimensions based on the original cube’s observations. 


Example request: 

GET http://localhost:8080/JSON-QB-REST-API/create-aggregations?dataset=http://id.mkm.ee/statistics/def/cube/crashes&http://id.mkm.ee/statistics/def/measure/total_cost=SUM&http://id.mkm.ee/statistics/def/measure/average_cost=AVG&http://id.mkm.ee/statistics/def/measure/number_of_crashes=SUM

### GET cubeOfAggregationSet

Parameters:
* dataset (required)
* 1 ore more free dimension as array i.e. dimension[]=DIM1 & dimension[]=DIM2 ...

Description: returns the cube of an aggregation set that has the specified dimensions

Example request:

GET [http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/cubeOfAggregationSet?dataset=http://id.mkm.ee/statistics/def/cube/buildings&dimension%5B%5D=http://id.mkm.ee/statistics/def/dimension/main_usage&dimension%5B%5D=http://id.mkm.ee/statistics/def/dimension/municipality](http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/cubeOfAggregationSet?dataset=http://id.mkm.ee/statistics/def/cube/buildings&dimension%5B%5D=http://id.mkm.ee/statistics/def/dimension/main_usage&dimension%5B%5D=http://id.mkm.ee/statistics/def/dimension/municipality)

Example result

```
{
  "@id": "http://id.mkm.ee/statistics/def/cube/buildings_main_usage_municipality",
  "label": "Buildings Cube(main_usage,municipality)"
}
```
## Cube metadata

### GET dataset-metadata

Parameter: dataset (required)

Example request:

GET [http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/dataset-metadata?dataset=http://statistics.gov.scot/data/economic-activity-benefits-and-tax-credits/employment](http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/dataset-metadata?dataset=http://statistics.gov.scot/data/economic-activity-benefits-and-tax-credits/employment)

Example result

```
{
  "@id": "http://statistics.gov.scot/data/economic-activity-benefits-and-tax-credits/employment",
  "label": "Employment",
  "description": "People are classed as \u0027in employment\u0027 (employee or self-employed) if they have done at least one hour of paid work in the week prior to their LFS interview... ",
  "comment": "People are classed as \u0027in employment\u0027 (employee or self-employed) if they have done at least one hour of paid work in the week prior to their LFS interview",
  "issued": "2014-07-29T01:00:00Z",
  "modified": "2015-06-04T01:00:00Z",
  "license": "http://www.nationalarchives.gov.uk/doc/open-government-licence/version/2/"
}
```


### GET dimensions

Parameter: dataset (required)

Description: returns all the dimensions of a cube

Example request:

GET [http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/dimensions?dataset=http://id.mkm.ee/statistics/def/cube/buildings](http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/dimensions?dataset=http://id.mkm.ee/statistics/def/cube/buildings)

Example result

```
{
  "dimensions": [
    {
      "@id": "http://id.mkm.ee/statistics/def/dimension/main_usage",
      "label": "Main Usage"
    },
    {
      "@id": "http://id.mkm.ee/statistics/def/dimension/municipality",
      "label": "Municipality"
    },
    {
      "@id": "http://id.mkm.ee/statistics/def/dimension/property_type",
      "label": "Property Type"
    },
    {
      "@id": "http://id.mkm.ee/statistics/def/dimension/status",
      "label": "Status of Building"
    }
  ]
}
```

### GET attributes

Parameter: dataset (required)

Description: returns all the attributes of a cube

Example request:

GET [http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/attributes?dataset=http://id.mkm.ee/statistics/def/cube/crashes](http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/attributes?datasetet=http://id.mkm.ee/statistics/def/cube/crashes)

Example result

```
{
  "attributes": [
    {
      "@id": "http://purl.org/linked-data/sdmx/2009/attribute#unitMeasure",
      "label": "Unit of measure"
    }
  ]
}
```

### GET measures

Parameter: dataset (required)

Description: returns all the measures of a cube

Example request:

GET [http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/measures?dataset=http://id.mkm.ee/statistics/def/cube/crashes](http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/measures?dataset=http://id.mkm.ee/statistics/def/cube/crashes)

Example result

```
{
  "measures": [
    {
      "@id": "http://id.mkm.ee/statistics/def/measure/average_cost",
      "label": "Average Cost"
    },
    {
      "@id": "http://id.mkm.ee/statistics/def/measure/number_of_crashes",
      "label": "Number of crashes"
    },
    {
      "@id": "http://id.mkm.ee/statistics/def/measure/total_cost",
      "label": "Total Cost"
    }
  ]
}
```

### GET dimension-values

Parameters:
* dataset (required)
* dimension (required)

Description: returns all the values of a dimension that appear at a specific cube

Example request:

GET [http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/dimension-values?dataset=http://id.mkm.ee/statistics/def/cube/crashes&dimension=http://id.mkm.ee/statistics/def/dimension/day](http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/dimension-values?dataset=http://id.mkm.ee/statistics/def/cube/crashes&dimension=http://id.mkm.ee/statistics/def/dimension/day)

Example result

```
{
  "values": [
    {
      "@id": "http://id.mkm.ee/statistics/concept/day/Friday",
      "label": "Friday"
    },
    {
      "@id": "http://id.mkm.ee/statistics/concept/day/Monday",
      "label": "Monday"
    },
    {
      "@id": "http://id.mkm.ee/statistics/concept/day/Saturday",
      "label": "Saturday"
    },
    {
      "@id": "http://id.mkm.ee/statistics/concept/day/Sunday",
      "label": "Sunday"
    },
    {
      "@id": "http://id.mkm.ee/statistics/concept/day/Thursday",
      "label": "Thursday"
    },
    {
      "@id": "http://id.mkm.ee/statistics/concept/day/Tuesday",
      "label": "Tuesday"
    },
    {
      "@id": "http://id.mkm.ee/statistics/concept/day/Wednesday",
      "label": "Wednesday"
    }
  ],
  "dimension": {
    "@id": "http://id.mkm.ee/statistics/def/dimension/day",
    "label": "Day"
  }
}
```

### GET attribute-values

Parameters:
* dataset (required)
* attribute (required)

Description: returns all the values of an attribute that appear at a specific cube

Example request:

GET [http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/attribute-values?dataset=http://id.mkm.ee/statistics/def/cube/crashes&attribute=http://purl.org/linked-data/sdmx/2009/attribute#unitMeasure](http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/attribute-values?dataset=http://id.mkm.ee/statistics/def/cube/crashes&attribute=http://purl.org/linked-data/sdmx/2009/attribute#unitMeasure)

Example result

```
{
  "values": [
    {
      "@id": "http://qudt.org/1.1/vocab/unit#Number",
      "label": "Number"
    }    
  ],
  "dimension": {
    "@id": "http://purl.org/linked-data/sdmx/2009/attribute#unitMeasure",
    "label": "unitMeasure"
  }
}
```


### GET dimension-levels

Parameters:
* dataset (required)
* dimension (required)

Description: returns all the levels of dimension values (in case of hierarchical data) that appear at a specific cube

Example request:

GET [http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/dimension-levels?dataset=http://id.vlaanderen.be/statistieken/dq/kubus-bouwvergunningen%23id&dimension=http://id.vlaanderen.be/statistieken/def%23refArea](http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/dimension-levels?dataset=http://id.vlaanderen.be/statistieken/dq/kubus-bouwvergunningen%23id&dimension=http://id.vlaanderen.be/statistieken/def%23refArea)

Example result

```
{
  "values": [
    {
      "@id": "http://id.fedstats.be/classificationlevel/province#id",
      "label": "Province"
    },
    {
      "@id": "http://id.fedstats.be/classificationlevel/municipality#id",
      "label": "Municipality"
    },
    {
      "@id": "http://id.fedstats.be/classificationlevel/region#id",
      "label": "Region"
    },
    {
      "@id": "http://id.fedstats.be/classificationlevel/district#id",
      "label": "District"
    }
  ],
  "dimension": {
    "@id": "http://id.vlaanderen.be/statistieken/def#refArea",
    "label": "Reference area"
  }
}
```

## Cube data


### GET slice

Parameters: 
* dataset(required),
* 0 ore more measures as array i.e. measure[]=M1 & measure[]=M2 (optional) - if no measures defined ALL are returned
* 0 or more fixed dimension identifiers (optional), 
* mode= URI | label (optional) - if no mode is defined then labels are returned
* limit = NUMBER (e.g. limit=100) (optional)

Description: returns a set of observations of the specified cube, that match to the specified fixed dimensions
  
Example request 1:

GET [http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/slice?dataset=http://id.mkm.ee/statistics/def/cube/crashes&limit=100&mode=label](http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/slice?dataset=http://id.mkm.ee/statistics/def/cube/crashes&limit=100&mode=label)      

Example result 1:

  ```
 {
  "observations": [
    {
      "Average Cost": "1182",
      "Date": "1-1-2013",
      "Day": "Tuesday",
      "Number of crashes": "5",
      "Time": "No available time",
      "Total Cost": "5908",
      "@id": "http://id.mkm.ee/statistics/cube_crashes/observation/1"
    },
    {
      "Average Cost": "400",
      "Date": "1-1-2013",
      "Day": "Tuesday",
      "Number of crashes": "1",
      "Time": "24:00",
      "Total Cost": "400",
      "@id": "http://id.mkm.ee/statistics/cube_crashes/observation/2"
    },
    {
      "Average Cost": "627",
      "Date": "1-1-2013",
      "Day": "Tuesday",
      "Number of crashes": "1",
      "Time": "21:00",
      "Total Cost": "627",
      "@id": "http://id.mkm.ee/statistics/cube_crashes/observation/3"
    },
    {
      "Average Cost": "733",
      "Date": "1-1-2013",
      "Day": "Tuesday",
      "Number of crashes": "2",
      "Time": "20:00",
      "Total Cost": "1465",
      "@id": "http://id.mkm.ee/statistics/cube_crashes/observation/4"
    }, ...
  ]
 }
  ```
     
Example request 2:

GET [http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/slice?dataset=http://id.mkm.ee/statistics/def/cube/crashes&measure[]=http://id.mkm.ee/statistics/def/measure/average_cost&http://id.mkm.ee/statistics/def/dimension/day=http://id.mkm.ee/statistics/concept/day/Friday&limit=100&mode=URI](http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/slice?dataset=http://id.mkm.ee/statistics/def/cube/crashes&measure[]=http://id.mkm.ee/statistics/def/measure/average_cost&http://id.mkm.ee/statistics/def/dimension/day=http://id.mkm.ee/statistics/concept/day/Friday&limit=100&mode=URI)

Example result 2:
```
{
  "observations": [
    {
      "http://id.mkm.ee/statistics/def/dimension/date": "4-1-2013",
      "http://id.mkm.ee/statistics/def/dimension/time": "http://id.mkm.ee/statistics/concept/time/notime",
      "http://id.mkm.ee/statistics/def/measure/average_cost": "868",
      "@id": "http://id.mkm.ee/statistics/cube_crashes/observation/48"
    },
    {
      "http://id.mkm.ee/statistics/def/dimension/date": "4-1-2013",
      "http://id.mkm.ee/statistics/def/dimension/time": "http://id.mkm.ee/statistics/concept/time/24",
      "http://id.mkm.ee/statistics/def/measure/average_cost": "515",
      "@id": "http://id.mkm.ee/statistics/cube_crashes/observation/49"
    },
    {
      "http://id.mkm.ee/statistics/def/dimension/date": "4-1-2013",
      "http://id.mkm.ee/statistics/def/dimension/time": "http://id.mkm.ee/statistics/concept/time/23",
      "http://id.mkm.ee/statistics/def/measure/average_cost": "1500",
      "@id": "http://id.mkm.ee/statistics/cube_crashes/observation/50"
    },
    {
      "http://id.mkm.ee/statistics/def/dimension/date": "4-1-2013",
      "http://id.mkm.ee/statistics/def/dimension/time": "http://id.mkm.ee/statistics/concept/time/21",
      "http://id.mkm.ee/statistics/def/measure/average_cost": "282",
      "@id": "http://id.mkm.ee/statistics/cube_crashes/observation/51"
    },...
  ]
}
```

### GET table

Parameter: 
* dataset(required), 
* 1 or more row as array (required) i.e. row[]=DIM1 & row[]=DIM2 (required)           
* 0 ore more col as array i.e. col[]=DIM3 & col[]=DIM4 (optional)           
* 0 ore more measures as array i.e. measure[]=M1 & measure[]=M2 (optional) - if no measures defined ALL are returned         
* 0 or more fixed dimension identifiers (optional)

Description: returns a 2D table representation of the cube’s observations that match to the specified fixed dimensions

NOTE: currenlty the API supports maximum 1 row and 1 col. Need to update to support more!!!

Example 2D table request:

GET [http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/table?dataset=http://statistics.gov.scot/data/economic-activity-benefits-and-tax-credits/employment&measure[]=http://id.vlaanderen.be/statistieken/def%23werkzaamheidsgraad&col[]=http://id.vlaanderen.be/statistieken/def%23refArea&row[]=http://id.vlaanderen.be/statistieken/def%23timePeriod&http://purl.org/linked-data/sdmx/2009/dimension%23sex=http://purl.org/linked-data/sdmx/2009/code%23sex-F&http://id.vlaanderen.be/statistieken/def%23leeftijdsgroep=http://id.vlaanderen.be/statistieken/concept/leeftijdsgroep_35-49%23id](http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/table?dataset=http://statistics.gov.scot/data/economic-activity-benefits-and-tax-credits/employment&measure[]=http://id.vlaanderen.be/statistieken/def%23werkzaamheidsgraad&col[]=http://id.vlaanderen.be/statistieken/def%23refArea&row[]=http://id.vlaanderen.be/statistieken/def%23timePeriod&http://purl.org/linked-data/sdmx/2009/dimension%23sex=http://purl.org/linked-data/sdmx/2009/code%23sex-F&http://id.vlaanderen.be/statistieken/def%23leeftijdsgroep=http://id.vlaanderen.be/statistieken/concept/leeftijdsgroep_35-49%23id)

Example result
```
{
  "structure": {
    "free_dimensions": {
      "timePeriod": {
        "@id": "http://id.vlaanderen.be/statistieken/def#timePeriod",
        "label": "Period of time"
      },
      "refArea": {
        "@id": "http://id.vlaanderen.be/statistieken/def#refArea",
        "label": "Reference Area"
      }
    },
    "locked_dimensions": {
      "sex": {
        "@id": "http://purl.org/linked-data/sdmx/2009/dimension#sex",
        "label": "Geslacht",
        "locked_value": {
          "@id": "http://purl.org/linked-data/sdmx/2009/code#sex-F",
          "label": "sex-F"
        }
      },
      "leeftijdsgroep": {
        "@id": "http://id.vlaanderen.be/statistieken/def#leeftijdsgroep",
        "label": "Age group",
        "locked_value": {
          "@id": "http://id.vlaanderen.be/statistieken/concept/leeftijdsgroep_35-49#id",
          "label": "35-49"
        }
      }
    },
    "dimension_values": {
      "refArea": {
        "S12000033": {
          "@id": "http://statistics.gov.scot/id/statistical-geography/S12000033",
          "label": "Aberdeen City"
        },
        "S12000034": {
          "@id": "http://statistics.gov.scot/id/statistical-geography/S12000034",
          "label": "Aberdeenshire"
        },...       
      },
      "timePeriod": {
        "jaar_2004": {
          "@id": "http://id.vlaanderen.be/statistieken/concept/jaar_2004#id",
          "label": "2004"
        },
        "jaar_2005": {
          "@id": "http://id.vlaanderen.be/statistieken/concept/jaar_2005#id",
          "label": "2005"
        },...     
      }
    }
  },
  "headers": {
    "columns": {
      "refArea": ["S12000033", "S12000034", "S12000041","S12000035","S12000036","S12000005","S12000013",... ]
    },
    "rows": {"timePeriod": ["jaar_2004", "jaar_2005","jaar_2006","jaar_2007", "jaar_2008","jaar_2009", "jaar_2010","jaar_2011"]
    }
  },
  "data": [[73.4,79.6, 77.6, 78.3, 74.4, 79.1, 82.8, ...], [76.6,78.8, 74.5, 84.3, 80.0, 76.4, 89.3,...],...]
 }

```

Example 1D table request:

GET [http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/table?dataset=http://statistics.gov.scot/data/economic-activity-benefits-and-tax-credits/employment&measure[]=http://id.vlaanderen.be/statistieken/def%23werkzaamheidsgraad&row[]=http://id.vlaanderen.be/statistieken/def%23timePeriod&http://purl.org/linked-data/sdmx/2009/dimension%23sex=http://purl.org/linked-data/sdmx/2009/code%23sex-F&http://id.vlaanderen.be/statistieken/def%23leeftijdsgroep=http://id.vlaanderen.be/statistieken/concept/leeftijdsgroep_35-49%23id&http://id.vlaanderen.be/statistieken/def%23refArea=http://statistics.gov.scot/id/statistical-geography/S12000033](http://wapps.islab.uom.gr:8084/JSON-QB-REST-API/table?dataset=http://statistics.gov.scot/data/economic-activity-benefits-and-tax-credits/employment&measure[]=http://id.vlaanderen.be/statistieken/def%23werkzaamheidsgraad&row[]=http://id.vlaanderen.be/statistieken/def%23timePeriod&http://purl.org/linked-data/sdmx/2009/dimension%23sex=http://purl.org/linked-data/sdmx/2009/code%23sex-F&http://id.vlaanderen.be/statistieken/def%23leeftijdsgroep=http://id.vlaanderen.be/statistieken/concept/leeftijdsgroep_35-49%23id&http://id.vlaanderen.be/statistieken/def%23refArea=http://statistics.gov.scot/id/statistical-geography/S12000033)

Example result:
```
{
  "structure": {
    "free_dimensions": {
      "timePeriod": {
        "@id": "http://id.vlaanderen.be/statistieken/def#timePeriod",
        "label": "Period of time"
      }
    },
    "locked_dimensions": {
      "sex": {
        "@id": "http://purl.org/linked-data/sdmx/2009/dimension#sex",
        "label": "Geslacht",
        "locked_value": {
          "@id": "http://purl.org/linked-data/sdmx/2009/code#sex-F",
          "label": "sex-F"
        }
      },
      "leeftijdsgroep": {
        "@id": "http://id.vlaanderen.be/statistieken/def#leeftijdsgroep",
        "label": "Age group",
        "locked_value": {
          "@id": "http://id.vlaanderen.be/statistieken/concept/leeftijdsgroep_35-49#id",
          "label": "35-49"
        }
      },
      "refArea": {
        "@id": "http://id.vlaanderen.be/statistieken/def#refArea",
        "label": "Reference Area",
        "locked_value": {
          "@id": "http://statistics.gov.scot/id/statistical-geography/S12000033",
          "label": "Aberdeen City"
        }
      }
    },
    "dimension_values": {
      "timePeriod": {
        "jaar_2004": {
          "@id": "http://id.vlaanderen.be/statistieken/concept/jaar_2004#id",
          "label": "2004"
        },
        "jaar_2005": {
          "@id": "http://id.vlaanderen.be/statistieken/concept/jaar_2005#id",
          "label": "2005"
        },
        "jaar_2006": {
          "@id": "http://id.vlaanderen.be/statistieken/concept/jaar_2006#id",
          "label": "2006"
        },
        "jaar_2007": {
          "@id": "http://id.vlaanderen.be/statistieken/concept/jaar_2007#id",
          "label": "2007"
        },
        "jaar_2008": {
          "@id": "http://id.vlaanderen.be/statistieken/concept/jaar_2008#id",
          "label": "2008"
        },
        "jaar_2009": {
          "@id": "http://id.vlaanderen.be/statistieken/concept/jaar_2009#id",
          "label": "2009"
        },
        "jaar_2010": {
          "@id": "http://id.vlaanderen.be/statistieken/concept/jaar_2010#id",
          "label": "2010"
        },
        "jaar_2011": {
          "@id": "http://id.vlaanderen.be/statistieken/concept/jaar_2011#id",
          "label": "2011"
        }
      }
    }
  },
  "headers": {
    "rows": {"timePeriod": ["jaar_2004","jaar_2005","jaar_2006","jaar_2007","jaar_2008","jaar_2009", "jaar_2010", "jaar_2011"   ]
    }
  },
  "data": [73.4, 76.6, 77.8, 79.6, 82.0, 75.5, 76.3, 80.2]
}

```




# Weekly Permit Upload
## How to use this repository

### Requirements
The data ingestion task runs weekly from 00:00:00-02:00:00 UTC−06:00 (US Central) on Tuesday and will ingest `.csv` files for each market dated for the day prior. For example, the task scheduled for July 27 2021 will ingest these three files located in the repository: `20210726_Austin.csv` `20210726_Dallas Fort Worth.csv` `20210726_Houston.csv`

Files uploaded to this folder must be `.csv` and the filename must match this format with the date format being `YYYYMMDD` and the market being case-sensitive.

Files must be uploaded weekly before Tuesday and dated for the preceding Monday to be automatically be ingested and available by 3AM Tuesday.

### How to upload for automatic ingestion
Files may be uploaded using a web browser by visiting https://github.com/retellhq/public-data/tree/main/permit-data and clicking the `Add File` button near the top

![upload](https://user-images.githubusercontent.com/14897747/134660342-9115c414-8078-4a18-8b8a-d305e3ea4221.png)

Once the files to be uploaded are selected, a summary for the change must be filled out (description optional).

<img width="1258" alt="CleanShot 2021-09-24 at 05 53 55@2x" src="https://user-images.githubusercontent.com/14897747/134663476-e31f0f04-60dc-4e27-960b-8721491b9483.png">

Commit changes and your new files should appear in the /permit-data/ folder in this repository.

## Performing a manual ingestion task on files in this repository
If weekly files were not uploaded by the following Monday but were added after the task has run then a Druid index task can be manually submitted using the Druid dashboard under the ingestion tab.

![CleanShot 2021-09-24 at 05 37 23@2x](https://user-images.githubusercontent.com/14897747/134661453-9af028f9-71d9-4bec-b262-73b5dd437b93.png)

The following json template is to be used for each file that needs to be ingested with these lines replaced:
 - URL on `line 9` replaced with the link to the raw file hosted on GitHub 
 - Interval on `line 29` replaced with an interval spanning a week. For a file dated 20210726 the interval will be `[2021-07-26/2021-07-19]`
 - Expression on `line 85` replaced with `nvl(\"dummyCol\", 'Austin')` and withthe appropriate market to be filled in for those columns

```
{
  "type": "index_parallel",
  "spec": {
    "ioConfig": {
      "type": "index_parallel",
      "inputSource": {
          "type": "http",
          "uris": [
              "https://example.com"
          ]
      },
      "inputFormat": {
        "type": "csv",
        "findColumnsFromHeader": true
      },
      "appendToExisting": true
    },
    "tuningConfig": {
      "type": "index_parallel",
      "partitionsSpec": {
        "type": "dynamic"
      }
    },
    "dataSchema": {
      "dataSource": "DATA_SOURCE",
      "granularitySpec": {
        "type": "uniform",
        "queryGranularity": "NONE",
        "rollup": false,
        "segmentGranularity": "WEEK",
        "intervals": ["INTERVALS"]
      },
      "timestampSpec": {
        "column": "pdate",
        "format": "M/d/yyyy"
      },
      "dimensionsSpec": {
        "dimensions": [
            "state",
            "county",
            "permno",
            "ptype",
            "pdate",
            "pdescription",
            {"name": "jvalue", "type": "double"},
            {"name": "jsqfeet", "type": "double"},
            "jaddr",
            "jcity",
            "jstate",
            "jzip",
            "jsubdiv",
            "jlotno",
            "aname",
            "aaddr",
            "acity",
            "astate",
            "azip",
            "aphone",
            "atype",
            "aurl",
            "afax",
            "aprimarycontact",
            "aemail",
            "bldcode",
            "units",
            "bldgs",
            "ctype",
            "legal",
            "contractor",
            "caddress",
            "ccity",
            "cstate",
            "czip",
            "cphone",
            "curl",
            "cfax",
            "cprimarycontact",
            "cemail",
            "Market"
        ]
      },
      "transformSpec": {
        "transforms": [
          {
          "type": "expression",
          "name": "Market",
          "expression": ""
          }
        ]
      } 
    }
  }
}
```
It may take up to 10 minutes before the new segments are available.

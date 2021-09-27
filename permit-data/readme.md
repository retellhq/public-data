# Weekly Permit Upload
## How to use this repository

### Requirements
The data ingestion task runs weekly from 00:00:00-02:00:00 UTC−06:00 (US Central) on Tuesday and will ingest `.csv` files for each market dated for the day prior. For example, the task scheduled for July 27 2021 will ingest these three files located in their respective folders in this repository: `20210726_Austin.csv` `20210726_Dallas Fort Worth.csv` `20210726_Houston.csv`

Files uploaded to these folders must be `.csv` and the filename must match this format with the date format being `YYYYMMDD` and the market being case-sensitive.

Files must be uploaded weekly before Tuesday and dated for the preceding Monday to be automatically be ingested and available by 3AM Tuesday.

### How to upload for automatic ingestion
Files may be uploaded using a web browser by clicking the `Add File` button near the top.


![upload](https://user-images.githubusercontent.com/14897747/134660342-9115c414-8078-4a18-8b8a-d305e3ea4221.png)

Once the file(s) to be uploaded are selected, a summary for the change must be filled out (description optional).

<img width="1258" alt="CleanShot 2021-09-24 at 05 53 55@2x" src="https://user-images.githubusercontent.com/14897747/134663476-e31f0f04-60dc-4e27-960b-8721491b9483.png">

Commit changes and your new files should appear in the /permit-data/ folder in this repository.


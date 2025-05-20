## Spotify Data Pipeline – AWS Serverless ETL Project

This project is about building a end-to-end serverless data pipeline on AWS that collects and analyzes trending songs from the Spotify API. It follows the ETL (Extract, Transform, Load) pattern and enables weekly insights using AWS Lambda, S3, Glue, and Athena.

## Context

A new artist coming into the music industry wants to track weekly top global songs to understand what kind of music trends worldwide.

This pipeline helps the client:

- Collect weekly Spotify chart data
- Clean and organize it
- Store it in a structured, queryable format
- Run analytics using SQL

## Workflow Architecture

[![image.png](https://i.postimg.cc/wTmwJ21Y/image.png)](https://postimg.cc/hfK8R982)

1. CloudWatch triggers Lambda weekly.

2. Lambda (extract) fetches top tracks from Spotify API → saves to S3 (raw_data/).

3. S3 Trigger activates Lambda (transform) when a new file is uploaded.

4. Lambda (transform) processes and stores structured data in transformed_data/.

5. Glue Crawler scans transformed_data/ → updates Glue Catalog table.

6. Athena is used to run SQL queries on the dataset.

## Sample Athena Query

```
SELECT 
  song_name,
  song_popularity,
  song_url
FROM songs_data
ORDER BY song_popularity DESC
LIMIT 10;
```
Result:

[![image.png](https://i.postimg.cc/C19q78Hb/image.png)](https://postimg.cc/YGf0qhPS)

## How to Deploy (High-level)

1. Create an S3 bucket (e.g. spotify-etl-project-senal)

2. Register an app on Spotify Developer Portal to get your Client ID/Secret

3. Set up AWS Lambda function for:
    ```
    data_extraction.py (triggered weekly by CloudWatch)
    data_transformation.py (triggered by S3 PUT events)
    ```

4. Use Glue Crawler to scan transformed_data/

5. Run queries using Athena

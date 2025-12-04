# Datasets

This page summarises the user-provided data required by the toolkit. The provided data must conform to the standards below.

## Points of interest (POI)

| Column name | Column type | Levels | Example | Mandatory | Definition |
|------------|-------------|---------|----------|-----------|------------|
| lat | float | | 36.7538 | Yes | Latitude coordinate in geographic Coordinate Reference System WGS84 |
| lon | float | | 3.0588 | Yes | Longitude coordinate in geographic Coordinate Reference System WGS84 |
| connectivity_type | string | unknown, mobile, mobile_broadband, metro, fiber, wireless, satellite, wired | fibre | No | Type of internet connectivity. If this column is missing, the connectivity status will be inferred using [speed test](https://www.ookla.com/ookla-for-good/open-data) measurements. |
| country_code | string | | DZA | No | ISO 3166-1 alpha-3 country code |
| dataset_id | UUID | | 987fcdeb-51a2-12d3-a456-426614174000 | No | Unique identifier for the dataset, automatically generated |
| has_electricity | boolean | | True | No | Whether the POI has electricity. If missing, no electricity will be assumed. |
| is_connected | boolean | | True | No | Whether the POI has connectivity. If this column is missing, the connectivity status will be inferred using [speed test](https://www.ookla.com/ookla-for-good/open-data) measurements. |
| number_of_users | boolean | | True | No | Peak number of internet user at the POI. If missing, this will be estimated by the demand model using population data. |
| poi_id | UUID | | 123e4567-e89b-12d3-a456-426614174000 | No | Unique identifier for the POI, automatically generated |
| poi_type | string | | school | No | Type of point of interest |

## Cell sites

| Column name | Column type | Levels | Example | Mandatory | Definition |
|------------|-------------|---------|----------|-----------|------------|
| lat | float | | 38.988755 | Yes | Latitude coordinate in geographic Coordinate Reference System WGS84 |
| lon | float | | 1.401938 | Yes | Longitude coordinate in geographic Coordinate Reference System WGS84 |
| radio_type | string | 2G, 3G, 4G, 5G | 4G | Yes | Type of radio transmission technology |
| antenna_height | float | | 25 | No | Antenna height in meters from the ground, if missing a height of 25 meters will be assumed. |
| country_code | string | | DZA | No | ISO 3166-1 alpha-3 country code |
| dataset_id | UUID | | 987fcdeb-51a2-12d3-a456-426614174000 | No | Unique identifier for the dataset |
| ict_id | UUID | | 123e4567-e89b-12d3-a456-426614174000 | No | Cell tower identifier |

## Transmission nodes

| Column name | Column type | Levels | Example | Mandatory | Definition |
|------------|-------------|---------|----------|-----------|------------|
| lat | float | | 38.988755 | Yes | Latitude coordinate in geographic Coordinate Reference System WGS84 |
| lon | float | | 1.401938 | Yes | Longitude coordinate in geographic Coordinate Reference System WGS84 |
| country_code | string | | DZA | No | ISO 3166-1 alpha-3 country code |
| dataset_id | UUID | | 987fcdeb-51a2-12d3-a456-426614174000 | No | Unique identifier for the dataset |
| ict_id | UUID | | 123e4567-e89b-12d3-a456-426614174000 | No | Node identifier |
| transmission_medium | string | fibre, microwave, other | fibre | No | Transmission medium. If missing, 'fibre' will be assumed. |

## Mobile coverage

| Column name | Column type | Levels | Example | Mandatory | Definition |
|------------|-------------|---------|----------|-----------|------------|
| geometry | geometry | | POLYGON((-74.0060 40.7128, -73.9857 40.7484, -73.9772 40.7516, -74.0060 40.7128)) | Yes | Mobile coverage polygons in geographic Coordinate Reference System WGS84 |
| radio_type | str | 2G, 3G, 4G, 5G | 4G | Yes | Radio technology type |
| country_code | string | | DZA | No | ISO 3166-1 alpha-3 country code |
| dataset_id | UUID | | 987fcdeb-51a2-12d3-a456-426614174000 | No | Unique identifier for the dataset |
| fid | str | | 123e4567-e89b-12d3-a456-426614174000 | No | Unique identifier for polygons |
# Datasets

This page summarizes the data user-provided required by the toolkit. The provided data must conform to the standards below.

## Points of interest (POI)

| Column name | Column type | Levels | Example | Mandatory | Definition |
|------------|-------------|---------|----------|-----------|------------|
| poi_id | UUID | | 123e4567-e89b-12d3-a456-426614174000 | No | Unique identifier for the POI, automatically generated |
| dataset_id | UUID | | 987fcdeb-51a2-12d3-a456-426614174000 | No | Unique identifier for the dataset, automatically generated |
| lat | float | | 36.7538 | Yes | Latitude coordinate |
| lon | float | | 3.0588 | Yes | Longitude coordinate |
| country_code | string | | DZA | No | ISO 3166-1 alpha-3 country code |
| is_connected | boolean | | True | No | Whether the POI has connectivity. If this column is missing, the connectivits status will be inferred using [speed test](https://www.ookla.com/ookla-for-good/open-data) measurements. |
| connectivity_type | string | unknown, mobile, mobile_broadband, metro, fiber, wireless, satellite, wired | fiber | No | Type of internet connectivity. If this column is missing, the connectivits status will be inferred using [speed test](https://www.ookla.com/ookla-for-good/open-data) measurements. |
| has_electricity | boolean | | True | No | Whether the POI has electricity. If missing, no electricity will be assumed. |
| number_of_users | boolean | | True | No | Peak number of internet user at the POI. If missing, this will be estimated by the demand model using population data. |
| poi_type | string | | school | No | Type of point of interest |

## Cell sites

| Column name | Column type | Levels | Example | Mandatory | Definition |
|------------|-------------|---------|----------|-----------|------------|
| ict_id | UUID | | 123e4567-e89b-12d3-a456-426614174000 | No | Cell tower identifier |
| dataset_id | UUID | | 987fcdeb-51a2-12d3-a456-426614174000 | No | Unique identifier for the dataset |
| lat | float | | 38.988755 | Yes | Cell tower geographical latitude |
| lon | float | | 1.401938 | Yes | Cell tower geographical longitude |
| radio_type | string | 2G, 3G, 4G, 5G | 4G | Yes | Type of radio transmission technology |
| antenna_height | float | | 25 | No | Antenna height in meters from the ground, if missing a height of 25 meters will be assumed. |
| operator_name | string | | TelOperator | No | Mobile network operator name |
| backhaul_type | string | fiber, microwave, satellite | fiber | No | Type of backhaul connectivity of the cell tower |
| backhaul_throughput_mbps | float | | 1000 | No | Equipped throughput of the backhaul |

## Transmission nodes

| Column name | Column type | Levels | Example | Mandatory | Definition |
|------------|-------------|---------|----------|-----------|------------|
| ict_id | UUID | | 123e4567-e89b-12d3-a456-426614174000 | No | Node identifier |
| dataset_id | UUID | | 987fcdeb-51a2-12d3-a456-426614174000 | No | Unique identifier for the dataset |
| lat | float | | 38.988755 | Yes | Geographical latitude |
| lon | float | | 1.401938 | Yes | Geographical longitude |
| transmission_medium | string | fiber, microwave, other | fiber | No | Transmission medium. If missing, 'fiber' will be assumed. |
| node_status | string | operational, planned, under construction | operational | No | Status of the node. If missing, 'operational' will be assumed. |
| operator_name | string | | TelOperator | No | Name of the mobile operator |
| equipped_capacity_mbps | float | | 1000 | No | Equipped bandwidth ready for use to connect subscribers |
| potential_capacity_mbps | float | | 2000 | No | Total theoretical bandwidth available for subscriber connections |

## Mobile coverage

| Column name | Column type | Levels | Example | Mandatory | Definition |
|------------|-------------|---------|----------|-----------|------------|
| geometry | geometry | | POLYGON((-74.0060 40.7128, -73.9857 40.7484, -73.9772 40.7516, -74.0060 40.7128)) | Yes | Mobile coverage polygons |
| radio_type | str | 2G, 3G, 4G, 5G | 4G | Yes | Radio technology type |
| fid | str | | 123e4567-e89b-12d3-a456-426614174000 | No | Unique identifier for polygons |
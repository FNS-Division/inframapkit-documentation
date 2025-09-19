# Datasets

This page summarizes the data user-provided required by the toolkit. The provided data must conform to the standards below.

## Points of interest (POI)

| Column name | Column type | Levels | Example | Mandatory | Definition |
|------------|-------------|---------|----------|-----------|------------|
| poi_id | UUID | | 123e4567-e89b-12d3-a456-426614174000 | Yes | Unique identifier for the POI |
| dataset_id | UUID | | 987fcdeb-51a2-12d3-a456-426614174000 | Yes | Unique identifier for the dataset |
| lat | float | | 36.7538 | Yes | Latitude coordinate |
| lon | float | | 3.0588 | Yes | Longitude coordinate |
| country_code | string | | DZA | Yes | ISO 3166-1 alpha-3 country code |
| is_connected | boolean | | True | Yes | Whether the POI has connectivity |
| connectivity_type | string | unknown, mobile, mobile_broadband, metro, fiber, wireless, satellite, wired | fiber | Yes | Type of internet connectivity |
| has_electricity | boolean | | True | Yes | Whether the POI has electricity |
| number_of_users | boolean | | True | No | Peak number of internet user at the POI |
| poi_type | string | | school | No | Type of point of interest |
| is_public | boolean | | True | No | Whether the POI is public or private |
| poi_subtype | string | | primary school | No | Specific subtype of the POI |

## Cell sites

| Column name | Column type | Levels | Example | Mandatory | Definition |
|------------|-------------|---------|----------|-----------|------------|
| ict_id | UUID | | 123e4567-e89b-12d3-a456-426614174000 | Yes | Cell tower identifier |
| dataset_id | UUID | | 987fcdeb-51a2-12d3-a456-426614174000 | Yes | Unique identifier for the dataset |
| latitude | float | | 38.988755 | Yes | Cell tower geographical latitude |
| longitude | float | | 1.401938 | Yes | Cell tower geographical longitude |
| radio_type | string | 2G, 3G, 4G, 5G | 4G | Yes | Type of radio transmission technology |
| antenna_height | float | | 25 | Yes | Antenna height in meters from the ground |
| operator_name | string | | TelOperator | No | Mobile network operator name |
| backhaul_type | string | fiber, microwave, satellite | fiber | No | Type of backhaul connectivity of the cell tower |
| backhaul_throughput_mbps | float | | 1000 | No | Equipped throughput of the backhaul |

## Transmission nodes

| Column name | Column type | Levels | Example | Mandatory | Definition |
|------------|-------------|---------|----------|-----------|------------|
| ict_id | UUID | | 123e4567-e89b-12d3-a456-426614174000 | Yes | Node identifier |
| dataset_id | UUID | | 987fcdeb-51a2-12d3-a456-426614174000 | Yes | Unique identifier for the dataset |
| latitude | float | | 38.988755 | Yes | Geographical latitude |
| longitude | float | | 1.401938 | Yes | Geographical longitude |
| transmission_medium | string | fiber, microwave, other | fiber | Yes | Transmission medium |
| node_status | string | operational, planned, under construction | operational | Yes | Status of the node |
| operator_name | string | | TelOperator | No | Name of the mobile operator |
| equipped_capacity_mbps | float | | 1000 | No | Equipped bandwidth ready for use to connect subscribers |
| potential_capacity_mbps | float | | 2000 | No | Total theoretical bandwidth available for subscriber connections |

## Mobile coverage

| Column name | Column type | Levels | Example | Mandatory | Definition |
|------------|-------------|---------|----------|-----------|------------|
| geometry | geometry | | POLYGON((-74.0060 40.7128, -73.9857 40.7484, -73.9772 40.7516, -74.0060 40.7128)) | Yes | Mobile coverage polygons |
| radio_type | str | 2G, 3G, 4G, 5G | 4G | Yes | Radio technology type |
| fid | str | | 123e4567-e89b-12d3-a456-426614174000 | No | Unique identifier for polygons |
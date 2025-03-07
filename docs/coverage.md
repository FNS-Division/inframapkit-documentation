# Coverage

## Overview

The Coverage class analyzes telecommunication coverage for points of interest (POIs). It determines whether POIs have access to specific types of telecommunication coverage (2G, 3G, 4G, 5G, or satellite).

**Key features:**

- Supports multiple analysis methods including coverage maps, proximity to cell sites, and buffer zones
- Handles different coverage types (2G, 3G, 4G, 5G, satellite)
- Provides summary statistics and formatted results

## Class Parameters

### Required

- **points_of_interest** (`PointOfInterestCollection`): Collection of points of interest for analysis.
- **mobile_coverage_type** (`str`): Type of coverage to analyze ('2G', '3G', '4G', '5G', 'satellite').

## Optional

- **mobile_coverage_data** (`geopandas.GeoDataFrame`, optional): Coverage polygons with a 'coverage' column. Required when method='map'.
- **cell_sites** (`CellSiteCollection`, optional): Collection of cell sites. Required when method='cellsites' or 'buffer'.
- **method** (`str`, default='map'): Analysis method to use:
    - 'map': Uses geographic coverage map polygons
    - 'cellsites': Uses proximity to cell sites
    - 'buffer': Uses buffered areas around cell sites
    - 'all_visible': Assumes all POIs have coverage
    - 'no_visible': Assumes no POIs have coverage
- **search_radius** (`int`, default=35): Distance in meters for cellsites method.
- **buffer_distance** (`int`, optional): Buffer size in meters for buffer method.
- **logger** (`logging.Logger`, optional): Logger instance. If None, a default logger is created.

## Class attributes

- **points_of_interest**: Collection of points of interest
- **cell_sites**: Collection of cell sites (if provided)
- **method**: Selected analysis method
- **mobile_coverage_data**: GeoDataFrame of coverage polygons (if provided)
- **mobile_coverage_type**: Type of coverage being analyzed
- **coverage_column**: Column name for results (e.g., '4G_coverage')
- **results_table**: GeoDataFrame with coverage analysis results
- **analysis_stats**: Dictionary with coverage statistics
- **analysis_param**: Dictionary for analysis parameters

## Methods

- **perform_analysis()**: Executes coverage analysis using the selected method and prints a summary.
- **get_results_table()**: Returns a DataFrame with coverage results in a nested dictionary format.
- **format_analysis_summary()**: Returns a human-readable summary of the analysis statistics.

## Example

```python
import pandas as pd
import geopandas as gpd
from giga_inframapkit.entities.pointofinterest import PointOfInterestCollection
from giga_inframapkit.entities.cellsite import CellSiteCollection
from giga_inframapkit.mapping.coverage import Coverage

# 1. Set up your data collections

poi_df = pd.read_csv("input/points_of_interest.csv")
poi_collection = PointOfInterestCollection()
poi_collection.load_from_records(poi_df.to_dict('records'))

# PointOfInterestCollection: 100 entities

coverage_data = gpd.read_file("input/mobile_coverage_4g.gpkg")

# 2. Create a Coverage analysis instance

coverage = Coverage(
    points_of_interest = poi_collection,
    mobile_coverage_data = coverage_data,
    mobile_coverage_type = "4G"
    )

# 3. Run the analysis

coverage.perform_analysis()

# 4G coverage Analysis Summary:
# Number of points of interest: 100
# Number of points of interest covered by signal: 8
# Number of points of interest not covered by signal: 92
# Time taken for analysis: 0.01 seconds

coverage.get_results_table()

# 	poi_id	coverage
# 0	23dd6a45-3656-435b-b3b1-c16efab9daeb	{'4G': True}
# 1	e13a657c-edf0-4013-92db-6c70136e3ac9	{'4G': False}
# 2	de75c87b-2676-47be-8454-4c44c4e6f644	{'4G': False}
# 3	4267fc81-0e9f-40ca-84c8-84529958dc22	{'4G': True}
# 4	ae0ccc6e-5f91-4a58-a60b-68e8ebcdc797	{'4G': False}
# ...	...	...
# 95	9b09549f-3f66-4a26-beda-db793cd49363	{'4G': True}
# 96	a7a55e1e-6078-4457-89fd-ab69814da42e	{'4G': False}
# 97	85495887-5439-4f49-82a9-8e0a031b9dc4	{'4G': False}
# 98	798ec30c-be58-4b93-9ebb-e6979cf73336	{'4G': False}
# 99	96f84def-7438-4e29-b45b-3368a83544ca	{'4G': True}
```
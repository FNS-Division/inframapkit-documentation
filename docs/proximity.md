# Proximity

## Overview

The Proximity class calculates distances between points of interest (POIs) and telecommunications infrastructure elements. It performs spatial analysis to find the nearest cell sites and transmission nodes for each POI.

**Key features:**

- Calculates distances to the nearest infrastructure elements by type (2G/3G/4G/5G cell sites, fiber nodes)
- Uses Haversine formula to account for Earth's curvature in distance calculations
- Efficiently processes large datasets using k-d trees for nearest-neighbor searches
- Provides both raw distance data and summary statistics

## Class parameters

### Required

- **points_of_interest** (`PointOfInterestCollection`): Collection of points of interest for which proximity will be calculated.
- **cell_sites** (`CellSiteCollection`): Collection of cell site locations with attributes including radio type (2G, 3G, 4G, 5G).
- **transmission_nodes** (`TransmissionNodeCollection`): Collection of transmission infrastructure nodes with attributes including transmission medium type.

### Optional

- **logger** (`logging.Logger`, optional):
Logger instance for capturing messages. If None, a default logger will be created.

## Class attributes

- **points_of_interest**: Original collection of POIs
- **cell_sites**: Original collection of all cell sites
- **transmission_nodes**: Original collection of all transmission nodes
- **cell_sites_2g/3g/4g/5g**: Filtered collections by radio type
- **transmission_nodes_fiber**: Filtered collection of fiber-based transmission nodes
- **analysis_param**: Dictionary for storing configuration parameters
- **analysis_stats**: Dictionary storing statistics (counts of elements, mean distances)
- **analysis_results**: Dictionary storing raw results (distances from each POI to infrastructure)

## Methods

- **perform_analysis()**: Executes the proximity analysis for all POIs and infrastructure elements.
- **get_results_table()**: Returns a formatted DataFrame with the analysis results in a structured format.
- **get_storage_table()**: Returns a DataFrame with the raw analysis results.
- **format_analysis_summary()**: Formats the analysis statistics as a human-readable string summary.

## Example

```python
import pandas as pd
from giga_inframapkit.entities.pointofinterest import PointOfInterestCollection
from giga_inframapkit.entities.cellsite import CellSiteCollection
from giga_inframapkit.entities.transmissionnode import TransmissionNodeCollection
from giga_inframapkit.proximity import Proximity

# 1. Set up your data collections

dataset_ids = {
    'pointofinterest': 'points_of_interest.csv',
    'cellsite': 'cell_sites.csv',
    'transmissionnode': 'transmission_nodes.csv'
}

data_collections = {
    'pointofinterest': PointOfInterestCollection(),
    'cellsite': CellSiteCollection(),
    'transmissionnode': TransmissionNodeCollection()
}

for data_category, filepath in dataset_ids.items():
    df = pd.read_csv(f"input/{filepath}").to_dict('records')
    data_collections[data_category].load_from_records(records)

# PointOfInterestCollection: 100 entities
# CellSiteCollection: 100 entities
# TransmissionNodeCollection: 30 entities

# 2. Create a Proximity analysis instance

proximity = Proximity(
    points_of_interest=poi_collection,
    cell_sites=cell_sites,
    transmission_nodes=transmission_nodes
)


# 3. Run the analysis

proximity.perform_analysis()

# Proximity Analysis Summary:
# Number of points of interest: 100
# Number of cell sites: 100
# Number of transmission nodes: 30
# Number of 2G cell sites: 6
# Number of 3G cell sites: 30
# Number of 4G cell sites: 64
# Number of 5G cell sites: 0
# Number of fiber nodes: 30
# Mean cell site distance: 1267.17
# Mean transmission node distance: 2451.69
# Mean 2G cell site distance: 5647.04
# Mean 3G cell site distance: 2419.78
# Mean 4G cell site distance: 1606.58
# Mean fiber node distance: 2451.69
# Time taken for analysis: 0.0 seconds

# 4. Inspect output

p_results = proximity.get_results_table()
p_results.head()

# 	poi_id	cell_site_dist	transmission_node_dist
# 0	23dd6a45-3656-435b-b3b1-c16efab9daeb	{'any': 1158, '2G': 2365, '3G': 2475, '4G': 11...	{'any': 2559, 'fiber': 2559}
# 1	e13a657c-edf0-4013-92db-6c70136e3ac9	{'any': 2817, '2G': 10258, '3G': 3967, '4G': 2...	{'any': 1808, 'fiber': 1808}
# 2	de75c87b-2676-47be-8454-4c44c4e6f644	{'any': 646, '2G': 4049, '3G': 1086, '4G': 646...	{'any': 3183, 'fiber': 3183}
# 3	4267fc81-0e9f-40ca-84c8-84529958dc22	{'any': 1066, '2G': 1066, '3G': 2863, '4G': 13...	{'any': 1678, 'fiber': 1678}
# 4	ae0ccc6e-5f91-4a58-a60b-68e8ebcdc797	{'any': 462, '2G': 7196, '3G': 2288, '4G': 462...	{'any': 1615, 'fiber': 1615}

```
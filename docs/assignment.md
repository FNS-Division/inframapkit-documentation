# Technology Assignment

## Overview

This model identifies the optimal technology for unconnected points of interest (POI), based on total deployment costs or other user-specified criteria. Technologies evaluated include: fiber, cellular (point-to-area), point-to-point microwave (p2p), and satellite. The optimal solutions are determined by solving an optimization problem that maximizes total net revenues, subject to technological constraints. The model incorporates feasibility assessments for each technology at each POI, drawing on information provided by other models in the toolkit.

**Key features:**

- Determines the optimal mix of technologies for all unconnected POIs to maximize total operator profits.
- Connects all POIs when no maximum budget exists, or selectively connects POIs to maximize operator profits within budget constraints.

## Dependencies

The `Technology Assignment` model has multiple dependencies, which are explained in the [Model Integration Framework](integrations.md) page of this documentation and also summarized below.

- **Fiber Path**: The `Fiber Path` model determines which POIs can be connected to the fiber network given a constraint on the length of a fiber line. This determines which POIs it is feasible to connect with fiber. In short, POIs that are too far away from a transmission node cannot be connected with fiber.
- **Visibility**: The `Visibility` model determines which POIs are visible from at least one cell site. Radio links using point-to-point microwave technology require visibility. Therefore, this model determines which POIs it is feasible to connect with this technology.
- **Coverage**: The `Coverage` model assesses which POIs are located in an area with 3G, 4G or 5G coverage. If at least one of those cellular technologies are available, then it is feasible to connect those POIs using point-to-area technology.

Moreover, the `Technology Assignment` model also requires the user to provide a `Cost` model. This is in order to: i) compare the deployment cost of different technologies and ii) report the cost associated with the final technology assignment solution. The `Cost` model itself also has its own model dependencies. First, it requires the output from the `Fiber Path` model to estimate the amount of fiber line and therefore fiber CAPEX cost for each POI. Second, it requires the `Demand` model which estimates the `total_mbps` metric, the total required throughput at each POI. This quantity is used to compute the revenues an operator can expect to recover at each POI.

## Technology Feasibility Criteria

First, the `Technology Assignment` model assesses which technologies are feasible for each POI based on the criteria below.

_Table. Technology feasibility criteria._

| Technology | Feasibility criteria |
|-------------------|---------------|
| fiber | The POI must be within a configured distance to a transmission node, or another fiber-connected POI |
| p2area | The POI must be within a 3G, 4G or 5G coverage area |
| p2p | The POI must be visible from a cell site or another POI |
| satellite | Always feasible |

## Optimization Problem

### Defining the Problem

The diagram below summarizes the optimization problem solved by the `Technology Assignment` model. This is a linear programming problem solved using the [CBC](https://coin-or.github.io/Cbc/intro.html) solver and the [PuLP](https://coin-or.github.io/pulp/) Python library.

If there is no maximum budget, the solver maximizes the operator's total net revenue while connecting all POIs. If there is a maximum budget, the solver maximizes the operator's revenue while keeping the costs under the maximum budget. The operator revenues are derived using each POI's required throughpout (in Mbps). POIs with more people living around them generate higher revenues for the operator (see [Cost Model](costmodel.md)).

_Figure. Optimization problem._

![optimization](diagrams/assignment.drawio.svg)

### Special Case: User-defined Technology Ranking

It is possible to assign technologies based on other criteria than costs and revenues. For example, users can specify a preferred ranking of technologies such as the one below:

```python
ranking = {"fiber": 1, "p2area": 2, "p2p": 3, "satellite": 4}
```

In the case above, fiber is always the preferred option, followed by p2area, p2p and satellite. As before, a technology can only be assigned to a POI if it is feasible. These preferences may be related to the quality, policies or regulations. If such a ranking is provided, the costs in the objective function default to zero for all technologies and the revenues are the values from this ranking dictionary. Therefore, the decision on which technology to select for a POI is no longer based on revenue but on those pre-specified preferences.

## Additional Constraints Related to Technologies

### Fiber

The optimization problem has additional constraints related to fiber connectivity. These constraints are derived from the output of the `Fiber Path` model. In the example below, new fiber connections cannot be longer than 5km. Therefore, $$POI_{B}$$ cannot directly connect to nearest the transmission node because it is 6km away. However, $$POI_{B}$$ can become connected with fiber by connecting to nearby $$POI_{A}$$ instead - provided that $$POI_{A}$$ is also connected with fiber.

_Figure. Fiber constraints._

![fiber-constraints](diagrams/fiber-constraints.drawio.svg)

In this case, the decision variable $$X_{(B, fiber)}$$ can only be equal to 1 if $$X_{(A, fiber)}$$ is also equal to 1. This is expressed by adding a constraint to the model in the form of:

$$X_{(B, fiber)} - X_{(B, fiber)} \cdot X_{(A, fiber)} \leq 0$$

### Point-to-point microwave

The optimization problem also has additional constraints related to point-to-point microwave connectivity (p2p).

_Figure. P2P constraints._

![p2p-constraints](diagrams/p2p-constraints.drawio.svg)

If a POI is not visible a cell site, it can still become connected with p2p if it is visible from another POI. However, if both a cell site and a POI are visible - the solver will favour establishing a radio link to the cell site rather than to another POI. In the example below, $$POI_{B}$$ can connect using p2p only if $$POI_{A}$$ is also connected (with any technology).

## Class Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| used_technologies | `dict` | Required | Specifies available technologies {'fiber', 'p2area', 'p2p', 'satellite': bool} |
| poi_data | `pd.DataFrame` | Required | Original POI data (incl. identifier, connection status, electricity availability) |
| mapping_results | `pd.DataFrame` | Required | Results from Proximity, Coverage and Demand models |
| visibility_results | `pd.DataFrame` | Required | Results from Visibilty model |
| fiberpath_results | `pd.DataFrame` | Required | Results from Fiber Path model |
| primary_tech_params | `dict` | None | Technology parameters for cost calculations (required if `cost_model` not provided) |
| ranking_dictionary | `dict` | None | User-defined  preferences for technology rankings {'fiber', 'p2area', 'p2p', 'satellite': int} |
| max_fiber_dist_km_per_poi | `int` | None | Maximum fiber connection distance per POI in km |
| cost_model | `CostModel` | None | Cost model for cost computations. If none is provided, `primary_tech_params` must be provided to create one. |
| logger | `logging.Logger` | None | Logger for output messages |

## Class Attributes

| Attribute | Type | Description |
|-----------|------|-------------|
| used_technologies | `dict` | Dictionary of technologies to be used in the analysis |
| ranking_dictionary | `dict` | User-defined technology preference rankings |
| poi_data | `pd.DataFrame` | Original POI data (incl. POI identifier, connection status, electricity availability) |
| mapping_results | `pd.DataFrame` | Results from Proximity, Coverage and Demand models |
| visibility_results | `pd.DataFrame` | Results from Visibilty model |
| fiberpath_results | `pd.DataFrame` | Results from Fiber Path model |
| fiberpath_results_filtered | `pd.DataFrame` | Filtered fiberpath_results table based on max distance |
| visibility_results_filtered | `pd.DataFrame` | Filtered visibility_results table based on POI and cell site distance (order column) |
| poi_inputs | `pd.DataFrame` | Merged dataset of all inputs for analysis |
| cost_model | `CostModel` | Model for cost computations |
| time_periods | `int` | Number of time periods for analysis |
| max_fiber_dist_km_per_poi | `int` | Maximum fiber connection distance per POI in km |
| assignment_solution_table | `pd.DataFrame` | Results of technology assignment analysis |
| logger | `logging.Logger` | Logger for output messages |

## Methods

| Method | Return Type | Description |
|--------|-------------|-------------|
| check_columns() | `None` | Verifies that all required columns are present in `poi_inputs` |
| check_feasibility() | `pd.DataFrame` | Evaluates technology feasibility for each POI |
| get_fiber_dependencies() | `dict` | Retrieves the fiber path dependencies for each POI |
| get_p2p_dependencies() | `dict` | Retrieves the P2P visibility dependencies for each POI |
| perform_analysis(scenario="lowest_cost", schedule=True, max_budget=None) | `pd.DataFrame` | Assigns optimal technology to POIs based on cost or ranking |
| clear_assignment_solution_table() | `None` | Clears the current assignment solution table |
| compute_solution_cost(costmodel=None, only_unconnected_pois=False) | `pd.DataFrame` | Computes costs for assigned technologies. It is possible to provide a different Cost model than the one used for the technology assignment with `costmodel`, and `only_unconnected_pois` controls whether we report costs for all or only unconnected POIs |
| _update_fiberpath_results_filtered() | `None` | Updates filtered fiber path results when max distance changes |
| _filter_visibility_output() | `None` | Filters visibility results to include only first-order connections |
| _update_poi_inputs() | `None` | Updates the merged POI input dataset |

## Outputs

## Example

```python
import pandas as pd
from giga_inframapkit.costmodel.costs import CostModel
from giga_inframapkit.costmodel.techassignment import TechnologyAssigner

# 1. Read required inputs

    # POI data
poi_df = pd.read_csv("input/points_of_interest.csv")
poi_df.head()

# poi_id	is_connected	lat	lon	has_electricity	electricity_type
# 0	23dd6a45-3656-435b-b3b1-c16efab9daeb	False	39.007771	1.561872	False	NaN
# 1	e13a657c-edf0-4013-92db-6c70136e3ac9	False	38.906858	1.275420	False	NaN
# 2	de75c87b-2676-47be-8454-4c44c4e6f644	False	38.993134	1.353725	False	NaN
# 3	4267fc81-0e9f-40ca-84c8-84529958dc22	False	39.004266	1.525952	False	NaN
# 4	ae0ccc6e-5f91-4a58-a60b-68e8ebcdc797	False	38.956769	1.323312	False	NaN

    # Results from Proximity, Coverage, Demand models
pcd_results_df = pd.read_json("input/pcd.json")
pcd_results_df.head()

# 	poi_id	lat	lon	cell_site_dist	transmission_node_dist	population	poi_count	number_of_users	total_mbps	coverage
# 0	23dd6a45-3656-435b-b3b1-c16efab9daeb	39.007771	1.561872	{'any': 1158, '2G': 2365, '3G': 2475, '4G': 11...	{'any': 2559, 'fiber': 2559}	{'population_1km': 443, 'population_2km': 2885...	{'poi_count_1km': 1, 'poi_count_2km': 4, 'poi_...	443	2215	{'4G': True}

    # Results from Visibility model
visibility_results_df = pd.read_csv("input/visibility.csv")
visibility_results_df.head()

# poi_id	ict_id	radio_type	ground_distance	antenna_los_distance	azimuth_angle	geometry	is_visible	num_visible	order	visible_pois	is_visible_pois
# 0	23dd6a45-3656-435b-b3b1-c16efab9daeb	f7412e93-8fb4-4abf-a298-8fb0876a97f5	4G	1502.0	1503.0	189.86	LINESTRING (1.561872175 39.00777101, 1.5588949...	True	1	1	['4267fc81-0e9f-40ca-84c8-84529958dc22', '386b...	True
# 1	e13a657c-edf0-4013-92db-6c70136e3ac9	8a4cfe6a-ce1a-4096-84dc-be09b32b8567	4G	4814.0	4825.0	143.54	LINESTRING (1.2754202 38.90685783, 1.308471633...	True	1	1	['40a28229-3dbf-4f08-a516-3f46680a2e55', '8c09...	True

    # Results from Fiber Path model
fiberpath_results_df = pd.read_csv("input/fiberpath.csv")
fiberpath_results_df.head()

# poi_id	closest_node_id	closest_node_distance	connected_node_id	connected_node_distance	fiber_path	upstream_node_id	upstream_node_distance	cluster_id	max_dist_km	in_mst_solution	n_conns
# 0	2b2ca058-e2b5-4ce2-a075-c3075f9027ef	c43ce78b-33f7-45b5-8763-ae7bca7fb995	3002.713	c43ce78b-33f7-45b5-8763-ae7bca7fb995	3002.713	['c43ce78b-33f7-45b5-8763-ae7bca7fb995']	c43ce78b-33f7-45b5-8763-ae7bca7fb995	3002.713	1	50.0	True	1
# 1	5a88dfd9-2448-4643-8446-ec81f79f236e	3555590c-de40-4e86-93df-8524dbc6c4c7	4240.552	541c5949-ab47-4a42-83df-2f5653718d5a	8397.644	['541c5949-ab47-4a42-83df-2f5653718d5a', 'ba2f...	955bca73-4483-45af-ae38-adddd0751bf7	3318.664	1	50.0	True	3

    # Microeconomic inputs (e.g. cost per km of fiber)
cost_inputs_df = pd.read_csv("input/cost_inputs.csv")
cost_inputs_df.head()

# 	Variable name	Value
# 0	hw_setup_cost_fiber	500.0
# 1	focl_constr_cost_fiber	8000.0
# 2	reinv_period_fiber	5.0
# 3	an_hw_maint_and_repl_fiber	0.1
# 4	an_traffic_fees_one_mbps_fiber	12.0

# 2. Specify which technologies can be considered

used_technologies = {
    'fiber': True,
    'p2area': True,
    'p2p': True,
    'satellite': True
}

# 2. Create a TechnologyAssigner instance

assigner = TechnologyAssigner(
    used_technologies=used_technologies,
    poi_data = poi_df,
    mapping_results=pcd_results_df,
    visibility_results=visibility_results_df,
    fiberpath_results=fiberpath_results_df,
    primary_tech_params=cost_inputs_df
    )

# 2025-04-02 10:35:26,717 - cost_ESP - INFO - A new cost model has been set up.
# 2025-04-02 10:35:26,724 - cost_ESP - INFO - Time periods set to 10.

# 3. Perform analysis without a maximum budget

assigner.perform_analysis(
    scenario="lowest_cost",
    max_budget=None
    )

# 2025-04-02 10:36:30,892 - cost_ESP - INFO - Checking the feasibility of technologies...
# 2025-04-02 10:36:30,974 - cost_ESP - INFO - Using the ranking scenario to assign technologies...
# 2025-04-02 10:36:30,982 - cost_ESP - INFO - Setting up the optimization problem...
# 2025-04-02 10:36:30,986 - cost_ESP - INFO - There are 100 POIs with at least one feasible technology.
# 2025-04-02 10:36:30,996 - cost_ESP - INFO - Optimizer parameter 'schedule' is set to False, meaning that connections lasts either 10 or 0 periods.
# 2025-04-02 10:36:31,005 - cost_ESP - INFO - Getting optimizer constraints related to fiber...
# 2025-04-02 10:36:31,009 - cost_ESP - INFO - Getting optimizer constraints related to P2P...
# 2025-04-02 10:36:31,011 - cost_ESP - INFO - Optimizing the NPV and connecting all POIs without a maximum budget...
# 2025-04-02 10:36:31,012 - cost_ESP - INFO - Number of optimizer variables: 433
# 2025-04-02 10:36:31,013 - cost_ESP - INFO - Number of optimizer constraints: 601
# 2025-04-02 10:36:31,013 - cost_ESP - INFO - Starting optimization...
# 2025-04-02 10:36:32,430 - cost_ESP - INFO - Solution Status: 1, Optimal
# 2025-04-02 10:36:32,443 - cost_ESP - INFO - No POIs connected with p2p in time period 0, skipping path constraint check.

# Welcome to the CBC MILP Solver 
# Version: 2.10.3 
# Build Date: Dec 15 2019 

# Result - Optimal solution found

# Objective value:                4000.00000000
# Enumerated nodes:               0
# Total iterations:               0
# Time (CPU seconds):             0.01
# Time (Wallclock seconds):       0.02

# Option for printingOptions changed from normal to all
# Total time (CPU seconds):       0.01   (Wallclock seconds):       0.03

# 4. Perform analysis without a maximum budget, only report costs for unconnected POIs

cost_solution = assigner.compute_solution_cost(only_unconnected_pois=True)
cost_solution.head()

# 									annual_cost	annual_cost_per_poi	annual_revenue	annual_revenue_per_poi	init_capex	pp_coo	pp_coo_per_poi	pp_profit	pp_profit_per_poi	pp_revenue	pp_revenue_per_poi	first_connected_period	number_of_periods
# poi_id	pp	max_dist_km	technology	is_connected	lat	lon	upstream_node_distance	bound													
# 005e4a38-ee50-41b2-a7ba-fd8fd86c5179	10	50	fiber	False	38.930953	1.286008	2762.078	central	5111.3	5111.3	13200.0	13200.0	24806.3	51112.6	51112.6	80887.4	80887.4	132000.0	132000.0	0	10
# 019d2f45-228e-4943-b302-1af7cb2ae820	10	50	fiber	False	38.942632	1.238764	3284.765	central	6031.2	6031.2	57240.0	57240.0	29405.9	60311.9	60311.9	512088.1	512088.1	572400.0	572400.0	0	10
```

# Documentation for cost model

## 1. Introduction

This document sets out the methodology used to cost the deployment of four connectivity technologies: fiber, cellular, point-to-point microwave and satellite.

This document also describes the implementation of these cost models using two Python classes: `CostModel` and `TechnologyAssigner`. These classes are part of a larger system designed to model costs and assign technologies for network deployment.

## 2. Cost model methodology

The cost model distinguishes between Capital Expenditure (Capex) and Operational Expenditure (Opex):

Capital Expenditure (Capex):

- Initial hardware investment.
- Periodic hardware refresh costs, scheduled at predetermined intervals.

Operational Expenditure (Opex):

- Annual maintenance costs, calculated as a percentage of the initial hardware investment.
- Traffic costs, derived by multiplying the annual transit bandwidth cost per 1 Mbps with the channel throughput for each connected point of interest.

The cost of operating (COO) for the planning period is the sum of both Capex and Opex.

For each cost model and technology, we describe the model inputs and the model logic below.

### 2.1 Fiber cost model

In the fiber cost model, the parameter `expansion_factor`, accounts for the surplus of fiber optical cable line that is usually purchased (usually 10% over the required amount).

**Table 1. Fiber cost model inputs.**

| Technology | Parameter | Variable name | Measurement unit |
| ---| ---| ---| --- |
| Fiber (fiber) | Annual access bandwidth cost for 1 Mbps of dedicated internet access channel over a fiber optic cable line | an\_isp\_fees\_one\_mbps\_fiber | USD per Mbps per year |
| Fiber (fiber) | Annual hardware maintenance and replacement costs​ | an\_hw\_maint\_and\_repl\_fiber | USD per year​ (as a fraction of hardware CapEx​) |
| Fiber (fiber) | Annual transit bandwidth cost  for 1 Mbps of dedicated internet access channel over a fiber optic cable line | an\_traffic\_fees\_one\_mbps\_fiber | USD per Mbps per year |
| Fiber (fiber) | Fiber optical cable line​ construction cost (materials, equipment, labor) | focl\_constr\_cost\_fiber | USD per km​ |
| Fiber (fiber) | Hardware refresh after | reinv\_period\_fiber | Years |
| Fiber (fiber) | On-premises hardware setup cost​ (materials, equipment, labor) | hw\_setup\_cost\_fiber | USD per school​ |
| Fiber (fiber) | Project planning period | pp\_fiber | Years |

**Figure 1. Fiber cost model.**

![fiber-cost-model](diagrams/fiber-cost-model.drawio.svg)

### 2.2 Cellular cost model
  
**Table 2. Cellular cost model inputs.**

| Technology | Parameter | Variable name | Measurement unit |
| ---| ---| ---| --- |
| Cellular (p2area) | Annual hardware maintenance and replacement costs​ | an\_hw\_maint\_and\_repl\_p2area | USD per year​ (as a fraction of hardware CapEx​) |
| Cellular (p2area) | Annual ISP fee for 1 Mbps of dedicated internet access channel over cellular network | an\_isp\_fees\_one\_mbps\_p2area | USD per Mbps per year |
| Cellular (p2area) | Annual Traffic fee for 1 Mbps of dedicated internet access channel over cellular network | an\_traffic\_fees\_one\_mbps\_p2area | USD per Mbps per year |
| Cellular (p2area) | On-premises hardware setup cost​ (materials, equipment, labor) | hw\_setup\_cost\_p2area | USD per school​ |
| Cellular (p2area) | Project planning period | pp\_p2area | Years |
| Cellular (p2area) | Reinvest into hardware after | reinv\_period\_p2area | Years |

**Figure 2. Cellular cost model.**

![cellular-cost-model](diagrams/cellular-cost-model.drawio.svg)

### 2.3 Point-to-point microwave cost model

In the point-to-point model, the cost model incorporates three new types of infrastructure: access links, backhaul links and retransmission towers. Capital expenditure is required to set up these materials, which also generate one-time and annual license fees.

**Table 3. Point-to-point cost model inputs.**

| Technology | Parameter | Variable name | Measurement unit |
| ---| ---| ---| --- |
| Point to point microwave (p2p) | Annual hardware maintenance and replacement costs​ | an\_hw\_maint\_and\_repl\_p2p | USD per year​ (as a fraction of hardware CapEx​) |
| Point to point microwave (p2p) | Annual ISP fee for 1 Mbps of dedicated internet access channel over a P2P microwave link | an\_isp\_fees\_one\_mbps\_p2p | USD per Mbps per year |
| Point to point microwave (p2p) | Annual recurring license fee for 1MHz | an\_license\_fee\_1mhz\_p2p | USD per MHz per year |
| Point to point microwave (p2p) | Annual Traffic fee for 1 Mbps of dedicated internet access channel over a P2P microwave link | an\_traffic\_fees\_one\_mbps\_p2p | USD per Mbps per year |
| Point to point microwave (p2p) | Bandwidth per access link | access\_link\_bandwidth\_p2p | MHz |
| Point to point microwave (p2p) | Bandwidth per backhaul link | backhaul\_link\_bandwidth\_p2p | MHz |
| Point to point microwave (p2p) | Microwave point-to-point access link installation and comissioning cost (materials, equipment, labor) | access\_link\_setup\_p2p | USD per hop​ |
| Point to point microwave (p2p) | Microwave point-to-point backhaul link installation and comissioning cost (materials, equipment, labor) | backhaul\_link\_setup\_p2p | USD per hop​ |
| Point to point microwave (p2p) | Number of microwave point-to-point backhaul links | backhaul\_link\_num\_p2p | Links |
| Point to point microwave (p2p) | Number of retransmission telecommunication towers | retr\_tower\_num\_p2p | Towers |
| Point to point microwave (p2p) | One time license fee for 1MHz | one\_time\_license\_fee\_1mhz\_p2p | USD per MHz |
| Point to point microwave (p2p) | On-premises hardware setup cost​ (materials, equipment, labor) | hw\_setup\_cost\_p2p | USD per school​ |
| Point to point microwave (p2p) | Project planning period | pp\_p2p | Years |
| Point to point microwave (p2p) | Reinvest into hardware after | reinv\_period\_p2p | Years |
| Point to point microwave (p2p) | Retransmission telecommunication tower installation cost | retr\_tower\_inst\_p2p | USD per tower |

**Figure 3. Point-to-point microwave cost model.**

![p2p-cost-model](diagrams/p2p-cost-model.drawio.svg)

### 2.4 Satellite cost model

**Table 4. Satellite cost model inputs.**

| Technology | Parameter | Variable name | Measurement unit |
| ---| ---| ---| --- |
| Satellite (satellite) | Annual hardware maintenance and replacement costs​ | an\_hw\_maint\_and\_repl\_sat | USD per year​ (as a fraction of hardware CapEx​) |
| Satellite (satellite) | Annual ISP fee for 1 Mbps of dedicated internet access channel over satellite channel | an\_isp\_fees\_one\_mbps\_sat | USD per Mbps per year |
| Satellite (satellite) | Annual Traffic fee for 1 Mbps of dedicated internet access channel over satellite channel | an\_traffic\_fees\_one\_mbps\_sat | USD per Mbps per year |
| Satellite (satellite) | On-premises hardware setup cost​ (materials, equipment, labor) | hw\_setup\_cost\_sat | USD per school​ |
| Satellite (satellite) | Project planning period | pp\_sat | Years |

**Figure 4. Satellite cost model.**

![satellite-cost-model](diagrams/satellite-cost-model.drawio.svg)

## 3. Implementing the cost model

### 3.1 CostModel Class

#### 3.1.1 Overview

The `CostModel` class is designed to model the costs associated with deploying and maintaining various types of network connections, including fiber, cellular (P2Area), point-to-point (P2P), and satellite.

#### 3.1.2 Attributes

*   **`primary_tech_params`** (pd.DataFrame): Cost parameters. This includes, for example, the cost of laying down one kilometer of fiber optical cable line. The columns `Variable name` and `Value` are required. Refer to the tables 1 to 4 in the documentation for information on each cost parameter. Run `CostModel.get_required_keys()` to see which values of column `Variable name` are required.
*   **`poi_num`** (dict): Number of points of interest (POIs) for different technologies. The keys to this dictionary are are **fiber**, **p2area**, **p2p** and **satellite**, and the default values are 1 POI for each technology.
*   **`ch_throughput`** (dict): Channel throughput for different technologies. The keys to this dictionary are **fiber**, **p2area**, **p2p** and **satellite**, and the default values are 1 mbps (Megabits per second) for each technology.
*   **`focl_length_fiber`** (float): Length of fiber optic cable for fiber technology, in kilometers.
*   `expansion_factor` (float): Factor to expand fiber optic cable length. This is set by default to 1.1, meaning that operators will purchase an additional 10% of fiber optic cable than is strictly required.
*   **`logger`** (logging.Logger): Logger instance. If none is provided, one is created.

#### 3.1.3 Methods

Each technology has its own associated cost method, which returns cost results inside a dictionary with keys **number\_poi**, **ch\_throughput**, **pp\_coo**, **pp\_coo\_per\_per\_poi**, **pp\_capex**, **init\_capex**, **an\_opex**, **init\_capex\_per\_poi** and **an\_opex\_per\_poi**. In addition, the results from `compute_fiber_costs()` also include the **fiber\_length**. Cost results are also stored in the class attribute `_results_table`.

*   **`compute_fiber_costs`**`()`: Calculate costs for fiber network deployment.
*   **`compute_cellular_costs`**`()`: Calculate costs for cellular network deployment.
*   **`compute_p2p_costs`**`()`: Calculate costs for point-to-point network deployment.
*   **`compute_satellite_costs`**`()`: Calculate costs for satellite network deployment.
*   **`compute_costs`**`()`: Compute costs for all types of connections.
*   **`get_results_table`**`()`: Return results as a DataFrame.
*   **`reset_parameters()`**: Reset parameters `poi_num` , `ch_throughput` and `focl_length_fiber` to default values.

#### 3.1.4 Usage Example

```python
# Print the first five rows of cost model inputs
print(cost_inputs.head())

#                 Variable name   Value
# 0         hw_setup_cost_fiber   500.0
# 1      focl_constr_cost_fiber  8000.0
# 2          reinv_period_fiber     3.0
# 3  an_hw_maint_and_repl_fiber     0.1
# 4                    pp_fiber    10.0

# Initialize an instance of CostModel class for a scenario where
  # We are connecting 15 POIs with each technology
  # The total channel_throughput associated with each technology is 10 mbps 
  # 10 kilometers of fiber optic cable line are required to connect those 15 POIs with fiber

costmodel = CostModel(primary_tech_params=cost_inputs,
                      poi_num = {'fiber': 15, 'p2area': 15, 'p2p': 15, 'satellite': 15},
                      ch_throughput = {'fiber': 10, 'p2area': 10, 'p2p': 10, 'satellite': 10},
                      focl_length_fiber = 5)

# Compute costs for each technology
costmodel.compute_costs()

# Get cost results in DataFrame format and display first five rows
costodel.get_results_table()

# 	cost_type	    technology	  value
# 0	number_poi	    fiber	      1.00000
# 1	fiber_length	fiber	      5.00000
# 2	ch_throughput	fiber	      10.00000
# 3	pp_coo	        fiber	      91700.00000
# 4	pp_coo_per_poi	fiber	      91700.00000
```

### 3.2 TechnologyAssigner Class

#### 3.2.1 Overview

The `TechnologyAssigner` class is responsible for assigning technologies to Points of Interest (POIs).

First, the **feasibility** of connecting each POI with the four technologies is assessed. The feasibility criteria are inferred from the results of the proximity, coverage, demand, fiber path and visibility analyses. For example, it might not be feasible to connect a POI using point-to-point microwave technology because there are no visible cell towers from the POI location.

Second, among the feasible technologies, a technology is assigned to each POI based on either minimum costs or user preferences.

- **Costs**: In this scenario, POIs are assigned the cheapest technology, based on the provided cost model and POI characteristics.
- **User preferences**: In this scenario, POIs are assigned the preferred technology based on the provided ranking of technologies.

#### 3.2.2 Attributes

*   **`used_technologies`** (dict): Technologies to be used in the analysis. If a technology is set to `False` then POIs will never be assigned that technology. See example below.

```python
{'fiber': True, 'p2mp': True, 'p2p': True, 'satellite': True}
```

*   **`ranking_dictionary`** (dict): User-defined ranking of technologies, used by the method `assign_technology` if assigning technologies based on user preferences (`scenario` =`ranking` ). See example below.

```python
{'fiber': 1, 'p2mp': 2, 'p2p': 3, 'satellite': 4}
```

*   **`mapping_results`** (pd.DataFrame): Results of proximity, coverage and demand analysis. This DataFrame has one row per POI. This table must contain the following columns:
    *   **poi\_id**: POI identifier.
    *   **lat**: POI latitude.
    *   **lon**: POI longitude.
    *   **cell\_site\_dist**: distance to the closest cell tower.
    *   **4G\_coverage**: whether the POI has 4G coverage.
    *   **is\_connected**: whether the POI is already connected.
    *   **total\_mbps**: connectivity demand associated with POI.
*   **`visibility_results`** (pd.DataFrame): Results of visibility analysis. This table must contain the following columns:
    *   **is\_visible**: whether the POI is visible from a cell tower.
    *   **num\_visible**: number of cell towers that are visible from the POI.
    *   **antenna\_los\_distance**: line-of-sight distance to the nearest antenna.
*   **`fiberpath_results`** (pd.DataFrame): Results of fiber path analysis. This table must contain the following columns:
    *   **max\_dist\_km**: The maximum length of the fiber optic cable used to connect the POI (between the POI and the closest connected node, which is not necessarily a transmission node).
    *   **fiber\_path**: Sequence of nodes between the POI the transmission node.
    *   **upstream\_node\_distance**: Distance in meters to the next node in the POI's fiber path.
*   **`max_fiber_dist_km_per_poi`** (int): Maximum fiber optic cable distance allowed per POI (between the POI and the next node in the POI's fiber path). This value must also exist in the column `max_dist_km` of the provided dataset `fiberpath_results`. If the length proposed by the fiber path solution to connect a POI is larger than `max_fiber_dist_km_per_poi` , then connecting this POI with fiber is not feasible.
*   **`max_cell_dist_km_per_poi`** (float): Maximum distance allowed between a POI and a cell tower. If the distance between a POI and the nearest cell tower is larger than `max_cell_dist_km_per_poi`, then connecting this POI with cellular technology is not feasible.
*   **`cost_model`** (CostModel): Instance of the CostModel class.

#### 3.2.3 Methods

*   **`check_feasibility`**`()`: Check feasibility of each technology for connecting POIs. Feasibility is assessed using the following inputs, available as class attributes:
    *   `used_technologies`
    *   `mapping_results` and `max_cell_dist_km_per_poi`
    *   `visibility_results`
    *   `fiberpath_results` and `max_fiber_dist_km_per_poi`

Returns a table with one row per POI and four columns: fiber\_feasible, p2area\_feasible, p2p\_feasible and satellite\_feasible

*   **`compute_cost_per_poi`**`()`: Compute cost for each POI for each technology, based on the provided cost model (`cost_model`). The costs are personalized to each POI, based on that POI's connectivity demand (in mbps) and, for fiber, based on the length of fiber optical cable that would be required to connect that POI with fiber.

Returns a DataFrame with one row per POI and four columns: fiber\_cost, p2area\_cost, p2p\_cost and satellite\_cost

*   **`assign_technology(`**`scenario`**`)`**: Assign feasible technologies based on chosen scenario. The parameter `scenario` can take one of the following values: `lowest_cost` or `ranking`.
    *   If assigning technologies based on `lowest_cost`, then each POI gets assigned the technology with the lowest cost according to the method `compute_cost_per_poi().`
    *   If assigning technologies based on `ranking` , then each POI gets assigned the technology with the highest rank according to the input `ranking_dictionary`.

This method enforces a technology assignment solution with a feasible fiber path, meaning that a POI can connect to a transmission node through other POIs that are also connected with fiber.

Returns a DataFrame with one row per POI and one column: technology.

*   `get_results_table()`: Generate aggregated metrics on technology assignments solution. Returns a DataFrame with the following values for each technology: poi\_num, total\_mbps and fiber\_length.

#### 3.2.4 Usage Examples

```python
# Initialize an instance of CostModel class
costmodel = CostModel(primary_tech_params=cost_inputs)

# Visualize first five rows of cost_inputs
print(cost_inputs.head())
#                 Variable name   Value
# 0         hw_setup_cost_fiber   500.0
# 1      focl_constr_cost_fiber  8000.0
# 2          reinv_period_fiber     3.0
# 3  an_hw_maint_and_repl_fiber     0.1
# 4                    pp_fiber    10.0

# Initialize an instance of TechnologyAssigner, and provide the cost model defined above as an input
assigner = TechnologyAssigner(
    used_technologies=used_technologies,
    ranking_dictionary=ranking_dictionary, 
    mapping_results=mapping_results, #output of proximity, coverage and demand models analyses
    visibility_results=visibility_results, #output of visibility
    fiberpath_results=fiberpath_results, #output of fiber path analysis
    max_fiber_dist_km_per_poi=1, # 1km
    max_cell_dist_km_per_poi=1, # 1km
    cost_model=costmodel) # Cost model defined above

# View assigner inputs
  # used technologies
print(used_technologies)
# {'fiber': True, 'p2mp': True, 'p2p': True, 'satellite': True}

  #ranking_dictionary
print(ranking_dictionary)
# {'fiber': 1, 'p2mp': 2, 'p2p': 3, 'satellite': 4}

  #mapping_results
print(mapping_results.head())
#                                  poi_id        lat       lon  cell_site_dist  \
# 0  23dd6a45-3656-435b-b3b1-c16efab9daeb  39.007771  1.561872            1158   
# 1  e13a657c-edf0-4013-92db-6c70136e3ac9  38.906858  1.275420            2817   
# 2  de75c87b-2676-47be-8454-4c44c4e6f644  38.993134  1.353725             646   
# 3  4267fc81-0e9f-40ca-84c8-84529958dc22  39.004266  1.525952            1066   
# 4  ae0ccc6e-5f91-4a58-a60b-68e8ebcdc797  38.956769  1.323312             462   

#    4G_coverage  is_connected  total_mbps  
# 0          1.0             0       32800  
# 1          0.0             0        2400  
# 2          0.0             0       10300  
# 3          1.0             0       14900  
# 4          0.0             0       21300

  #visibility_results
print(visibility_results.head())
#                                  poi_id                                ict_id  \
# 0  23dd6a45-3656-435b-b3b1-c16efab9daeb  f7412e93-8fb4-4abf-a298-8fb0876a97f5   
# 1  23dd6a45-3656-435b-b3b1-c16efab9daeb  ad9f5591-f2fd-4ec6-a35f-31c4fb8d7013   
# 2  23dd6a45-3656-435b-b3b1-c16efab9daeb  4ea23bde-9f03-4e96-871d-59276323e1c2   
# 3  e13a657c-edf0-4013-92db-6c70136e3ac9  8a4cfe6a-ce1a-4096-84dc-be09b32b8567   
# 4  de75c87b-2676-47be-8454-4c44c4e6f644  9429f910-1d6b-4e52-8bb4-6f331d21e7ee   

#    ground_distance  antenna_los_distance  is_visible  num_visible  
# 0           1502.0                1503.0        True            3  
# 1           1784.0                1784.0        True            3  
# 2           2598.0                2598.0        True            3  
# 3           4814.0                4825.0        True            1  
# 4            646.0                 647.0        True            3

  #fiberpath_results
print(fiberpath_results.head())
#                                  poi_id  max_dist_km  \
# 0  5b9fbca7-20b7-4b7a-8555-1be8137a8e12          1.0   
# 1  798ec30c-be58-4b93-9ebb-e6979cf73336          1.0   
# 2  07514ee2-1dd3-4e93-a5fb-8a1a70b64edd          1.0   
# 3  304e1e26-439a-45fb-8837-9a38fdc6ec41          1.0   
# 4  03116452-1288-434e-83d5-fed21199c4a8          1.0   

#                                           fiber_path  \
# 0           ['278a16fe-081f-4d8d-bd4b-8fd8400ae23f']   
# 1  ['99fbad83-671b-47b4-9241-af3aa1e160ce', '8212...   
# 2           ['8ad3e27f-e257-40bb-bdde-005c547536e8']   
# 3           ['926790f6-1345-4cab-92eb-0d28185e2d54']   
# 4           ['090dcd28-f45e-42b6-be51-73fa90b3e7f7']   

#                        upstream_node_id  upstream_node_distance  
# 0  278a16fe-081f-4d8d-bd4b-8fd8400ae23f                 660.705  
# 1  8212c26b-ccf0-4c70-8c8b-0bd759f2cbff                 727.908  
# 2  8ad3e27f-e257-40bb-bdde-005c547536e8                 277.562  
# 3  926790f6-1345-4cab-92eb-0d28185e2d54                 862.034  
# 4  090dcd28-f45e-42b6-be51-73fa90b3e7f7                 478.980

# Check feasibility of each technology
assigner.check_feasibility()
#	poi_id	fiber_feasible	p2area_feasible	p2p_feasible	satellite_feasible
# 0	005e4a38-ee50-41b2-a7ba-fd8fd86c5179	False	False	True	True
# 1	019d2f45-228e-4943-b302-1af7cb2ae820	False	False	False	True
# 2	03116452-1288-434e-83d5-fed21199c4a8	True	False	True	True
# 3	033ae344-5dde-4ee7-8abd-9c83729197bf	True	False	True	True
# 4	04da0ba1-3b87-4a8e-bb66-72ac9a19f1cd	False	False	True	True

# Compute cost 
assigner.compute_cost_per_poi()
# poi_id	fiber_cost	p2area_cost	p2p_cost	satellite_cost
# 0	005e4a38-ee50-41b2-a7ba-fd8fd86c5179	NaN	3192400.0	3.211048e+06	3202200.0
# 1	019d2f45-228e-4943-b302-1af7cb2ae820	NaN	7488400.0	7.507048e+06	7498200.0
# 2	03116452-1288-434e-83d5-fed21199c4a8	9464548.0	1032400.0	1.051048e+06	1042200.0
# 3	033ae344-5dde-4ee7-8abd-9c83729197bf	18099194.4	1104400.0	1.123048e+06	1114200.0
# 4	04da0ba1-3b87-4a8e-bb66-72ac9a19f1cd	NaN	21876400.0	2.189505e+07	21886200.0

# Assign technologies based on the lowest cost
assigner.assign_technology(scenario="lowest_cost")
# poi_id  technology	
# f4757abf-5db7-4f0e-a7cc-0c437012a72e	satellite
# b32e5cdd-cc23-4748-9c61-c62201237bf6	satellite
# 8955c08a-d3a8-4dd0-872a-09489285a97a	satellite
# 9b09549f-3f66-4a26-beda-db793cd49363	p2area
# 798ec30c-be58-4b93-9ebb-e6979cf73336	satellite
# ...	...
# f5d03633-ba23-4de9-80ea-e5ae1b82acfa	satellite
# f8e815fd-fcbe-4607-9ae0-f5fa0d56c8d8	satellite
# f9394e7c-c097-4bd1-98c1-c36ab13dde8e	satellite
# fa2f7383-f6dd-4b0e-9db6-269f8f5d4a17	satellite
# feaecc5f-7357-48b4-9cc8-a845d67ecb4c	satellite

# Assign technologies based on ranking
assigner.assign_technology(scenario="ranking")
# poi_id      technology	
# f4757abf-5db7-4f0e-a7cc-0c437012a72e	fiber
# b32e5cdd-cc23-4748-9c61-c62201237bf6	fiber
# 8955c08a-d3a8-4dd0-872a-09489285a97a	fiber
# 9b09549f-3f66-4a26-beda-db793cd49363	fiber
# 798ec30c-be58-4b93-9ebb-e6979cf73336	fiber
# ...	...
# f5d03633-ba23-4de9-80ea-e5ae1b82acfa	p2p
# f8e815fd-fcbe-4607-9ae0-f5fa0d56c8d8	p2p
# f9394e7c-c097-4bd1-98c1-c36ab13dde8e	p2p
# fa2f7383-f6dd-4b0e-9db6-269f8f5d4a17	p2p
# feaecc5f-7357-48b4-9cc8-a845d67ecb4c	p2p

# View summary of results
assigner.get_results_table()
# 	technology	metric	value	type
# 0	fiber	fiber_length	0.0	unconnected
# 1	fiber	poi_num	0.0	unconnected
# 2	p2area	poi_num	8.0	unconnected
# 3	p2p	poi_num	0.0	unconnected
# 4	satellite	poi_num	92.0	unconnected
```

### 3.3 Integration and Workflow

The diagram below shows how a typical workflow for cost modelling and technology assignment works, allowing for two different ways of assigning technologies: minimum costs or a ranking.

![cost-model-integration](diagrams/cost-model-integration.drawio.svg)
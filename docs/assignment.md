# Technology Assignment

## Overview

This model identifies the optimal technology for unconnected points of interest (POI), based on total deployment costs or other user-specified criteria. Technologies evaluated include: fiber, cellular (point-to-area), point-to-point microwave (p2p), and satellite. The optimal solutions are determined by solving an optimization problem that maximizes total net revenues, subject to technological constraints. The model incorporates feasibility assessments for each technology at each POI, drawing on information provided by other models in the toolkit.

**Key features:**

- Determines the optimal mix of technologies for all unconnected POIs to maximize total operator profits.
- Connects all POIs when no maximum budget exists, or selectively connects POIs to maximize operator profits within budget constraints.
- Allows substitution of profit-related criteria with alternative considerations such as regulatory compliance or service quality.

## Dependencies

The `Technology Assignment` model has multiple dependencies, which are explained in the [Model Integration Framework](integrations.md) page of this documentation and summarized below.

- **Fiber Path**: The `Fiber Path` model determines which POIs can be connected to the fiber network given a constraint on the length of a fiber line. This determines which POIs it is feasible to connect with fiber. In short, POIs that are too far away from a transmission node cannot be connected with fiber.
- **Visibility**: The `Visibility` model determines which POIs are visible from at least one cell site. Radio links using point-to-point microwave technology require visibility. Therefore, this model determines which POIs it is feasible to connect with this technology.
- **Coverage**: The `Coverage` model assesses which POIs are located in an area with 3G, 4G or 5G coverage. If at least one of those cellular technologies are available, then it is feasible to connect those POIs using point-to-area technology.

Moreover, the `Technology Assignment` model also requires the user to provide a `Cost` model. This is in order to: i) compare the deployment cost of different technologies and ii) report the cost associated with the final technology assignment solution. The `Cost` model itself also has its own model dependencies. First, it requires the output from the `Fiber Path` model to estimate the amount of fiber line and therefore fiber CAPEX cost for each POI. Second, it requires the `Demand` model which estimates the `total_mbps` metric, which is the total required throughput at each POI. This quantity is used to compute the revenues an operator can expect to recover at each POI.

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

If there is no maximum budget, the solver maximizes the operator's total net revenue while connecting all POIs. If there is a maximum budget, the solver maximizes the operator's revenue while keeping the costs under the maximum budget. The operator revenues are derived using each POI's required throughpout (in mbps). POIs with a larger population living around them generate higher revenues for the operator (see [Cost Model](costmodel.md)).

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

The optimization problem has additional constraints related to fiber connectivity. These constraints are derived from the output of the `Fiber Path` model. In the example below, new fiber connections cannot be longer than 5km. Therefore, $POI_{B}$ cannot directly connect to nearest the transmission node because it is 6km away. However, $POI_{B}$ can become connected with fiber by connecting to nearby $POI_{A}$ - provided that $POI_{A}$ is also connected with fiber.

_Figure. Fiber constraints._

![fiber-constraints](diagrams/fiber-constraints.drawio.svg)

In this case, the decision variable $X_{(B, fiber)}$ can only be equal to 1 if $X_{(A, fiber)}$ is also equal to 1. This is expressed by adding a constraint to the model in the form of:

$X_{(B, fiber)} - X_{(B, fiber)} \cdot X_{(A, fiber)} \leq 0$

### Point-to-point microwave

The optimization problem also has additional constraints related to point-to-point microwave connectivity (p2p).

_Figure. P2P constraints._

![p2p-constraints](diagrams/p2p-constraints.drawio.svg)

If a POI is not visible a cell site, it can still become connected with p2p if it is visible from another POI. However, if both a cell site and a POI are visible - the solver will favor the radio link to the cell site rather than to another POI. In the example below, $POI_{B}$ can connect using p2p only if $POI_{A}$ is also connected (with any technology).

## Class Parameters

## Class Attributes

## Methods

## Outputs

## Example

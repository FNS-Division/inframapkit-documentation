# Technology Assignment

## Overview

This model identifies the optimal technology for unconnected points of interest (POI), based on total deployment costs or other user-specified criteria. Technologies evaluated include: fiber, cellular (point-to-area), point-to-point microwave, and satellite. The optimal solutions are determined by solving an optimization problem that maximizes total net revenues, subject to technological constraints. The model incorporates feasibility assessments for each technology at each POI, drawing on information provided by other models in the toolkit.

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

## Optimization problem

Introduce the decision variables...

X poi, time, tech where...

feas poi, tech

Paste example of matrix from slides

Maximize an objective function... two options... diagram with two paths

Talk about special case of the ranking approach

Additional constraints (in text)

- Fiber
- P2P

Explain which SOLVER is used and HOW

To summarize...

## Class Parameters

## Class Attributes

## Methods

## Outputs

## Example

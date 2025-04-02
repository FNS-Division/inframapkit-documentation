# Connectivity Planning Platform Developer Documentation

## Background

The Connectivity Planning Platform is a suite of models designed for the analysis of telecommunication infrastructure data and to plan the connectivity of Points of Interest (POIs). The models hosted on this platform include:

- [Proximity](proximity.md): to assess the proximity of POIs to ICT infrastructure
- [Coverage](coverage.md): to assess the coverage status for POIs (including 3G, 4G, 5G)
- [Demand](demand.md): to estimate the number of internet users at each POI and the required throughput
- [Visibility](visibility.md): to assess the feasibility of point-to-point microwave links that require line-of-sight visibility
- [Fiber Path](fiberpath.md): to simulate the optimal fiber path connecting POIs to transmission nodes
- [Cost Model](costmodel.md): to evaluate the cost (capital and operational expenditures) of deploying connectivity solutions
- [Technology Assignment](assignment.md): to select the mix of technologies (among fiber, point-to-area, point-to-point and satellite) that maximize operator revenues, under budgetary and technological constraints

This documentation serves as a guide to developers of the Connectivity Planning Platform, providing insights into its functionality and application.

## How to read this documentation?

This documentation is aimed at a technical audience, including those who will fine-tune models to suit their analysis needs and those who wish to contribute to the development of the platform.

Many of the tools in this platform are inter-related. The outputs of some models are required to run dependent models. For an overview of the end-to-end analysis workflow and the model dependencies, start with the [Model Integration Framework](integrations.md) then read about the individual models.
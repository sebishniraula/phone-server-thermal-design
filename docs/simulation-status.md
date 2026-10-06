# Simulation Status and Next Runs

## Baseline

Current baseline:

- 40 logic boards
- 19 in × 24 in × 2U chassis
- four front fans
- 19 °C ambient
- 12 W modeled heat load per board
- forced-air conjugate heat-transfer model
- 1 mm, 8 W/(m·K) board-to-heatsink interface layer

The first baseline run indicates a rear-of-rack hot region with board temperatures around 95 °C.

## Why perturb the geometry?

The original model contains assumptions that are difficult to know exactly before destructive hardware characterization or physical rack validation. Treating those values as perfectly known would make the model look more certain than it is.

The current simulation campaign therefore perturbs the **shape and position of components**, along with rack-level airflow features, to see which assumptions materially change the thermal result.

The purpose is to answer questions such as:

- Does uncertainty in the modeled SoC location materially affect rack-level conclusions?
- How sensitive is the result to board spacing?
- How much does fan spacing change temperature uniformity?
- Is the rear hot region primarily an inlet problem, a pressure-drop problem, or a bypass-flow problem?
- Does the ranking of candidate designs change between an 8 W and 12 W heat load?

## Planned cases

1. Baseline four-front-fan arrangement
2. Fans distributed more evenly across the front face
3. Additional rear or side inlet openings
4. Inlet shroud / baffle
5. Different board spacing
6. Different fan pressure-flow operating points
7. 8 W per-board heat load
8. 12 W per-board heat load
9. Component-position perturbations
10. Simplified geometry / shape perturbations

## Comparison metrics

Each candidate should be compared using the same primary metrics:

- maximum board temperature;
- average board temperature;
- hottest-to-coolest temperature spread;
- rear-versus-front temperature difference;
- airflow uniformity;
- pressure drop;
- fan power;
- geometry / manufacturing complexity.

The result of the study should be an engineering tradeoff, not only a single “best temperature” number.

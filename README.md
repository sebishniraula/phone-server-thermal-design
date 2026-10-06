# Phone Server Thermal Design

> **Work in progress.** This is not the paper, but an accumulation of the necessary information, figures, assumptions, and engineering decisions that are discussed in more depth in the paper, which is still in progress.

I am currently running simulations with perturbations in the shapes and positions of components to include more engineering tradeoffs in the methods paper. The current plan is to publish the completed work as an **arXiv preprint rather than a journal paper**, since this is primarily a documentation/methods effort. That could change if the work takes a novel direction or produces new knowledge while the design and simulation work develops.

## Why this project exists

The larger project is an attempt to repurpose retired or cosmetically defective smartphones as a low-cost compute cluster. Instead of treating a phone as a complete consumer device, the system removes the casing, display, battery, and unnecessary peripherals and uses the **logic board as the compute node**.

That immediately turns thermal management into a system-level problem. A phone is normally designed around a tightly constrained consumer enclosure; a server cluster expects sustained operation, predictable cooling, and many processors working together. The specific problem here is therefore:

**How do we adequately and uniformly cool a dense rack of 40 smartphone logic boards under sustained load?**

Uneven cooling matters because every node contributes to the cluster. If one region runs much hotter than the rest, thermal throttling or failure there can reduce the usefulness of the whole server.

## Current prototype

The prototype contains **40 phone logic boards** arranged inside a rack-scale chassis.

| Parameter | Current value / assumption |
|---|---|
| Compute nodes | 40 smartphone logic boards |
| Chassis | 19 in × 24 in × 2U |
| Cooling approach | Forced-air convection |
| Fans | 4 front-mounted fans |
| Fan operating point | 100% duty cycle |
| Fan power | ~9 W each |
| Measured phone power | ~8 W sustained in current tests |
| Modeled phone heat load | 12 W per board |
| Modeled processor temperature condition | ~100 °C |
| Ambient temperature | 19 °C |
| Thermal interface layer | 8 W/(m·K), 1 mm |
| Simulation type | Conjugate heat transfer |
| Simulation platform | SimScale |

The rack uses the conventional 19-inch server form factor because standard server hardware already exists around it: power supplies, fans, mounting components, and rack infrastructure.

## Mechanical layout

The boards are arranged in parallel, with their long axis aligned with the main airflow direction. Each board is attached to a rectangular plate/heatsink structure that can be clamped into the rack.

![Rack CAD model](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfH5EymIs86qAukBSWVZtzEGvO_8coPMXdSQ38JJ6X4Hji0qc-nes7pmcYbX1codCzhFuOI3RQxONZhHuljB27CagFQM6r56SQIk-mDJziQ9qXbsBzoT9WGtGsLZZmqe_LgcT6LTxixdcSGikyWY3Zlbadj-Y_z9va_qcbsOreHYuPnaXo=s2048?key=5Qii6-4OpIi7sWm_Y2ks1A)

The layout was partly inspired by dense phone-farm hardware. The use case is different, but it is one of the few examples of mechanically packaging many phones or phone boards in one enclosure.

A logic-board model was built approximately 1:1 in SolidWorks. Very small components were omitted because modeling every package would add substantial geometric complexity without proportional value for the current rack-level study. The major copper and metallic shielding structures were retained.

Because the processor package was not physically delayered, the exact SoC position under the shielding was not directly known. The internal heat-producing region is therefore an engineering approximation.

![Logic-board model](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfv3EZtlSjQTVUXB9Ht_TcjMcheQrgKjU1WSktJtFmRWVciKHDUftEmWulOoZEQ9TkVDlEfPR3jyLIN2m8C0Mvpaoa_PoLGBOrAhcIk2uTCimB6ZfNYcBtgrhp4mB76M4DHIoI4ruQBlRtMNfGeysB-afW96thFhbyeJcBoJ1GuvI2T=s2048?key=5Qii6-4OpIi7sWm_Y2ks1A)

## Power and thermal characterization

Before the CFD model, the phone was stressed experimentally while power draw and internal temperature were logged.

The PSU exposes power data over USB, allowing power draw to be recorded programmatically. Internal temperature was read from device sensors while processor stress workloads were running.

Under one sustained workload, the board repeatedly drew around **8 W**.

![Power draw and temperature under load](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdrl8RQlyDqDMzWGPpQUHyGnOylZENwDVJ85qWZ6hrC_8XhZVFNCfe8YOdClMNzPgBBKlCjzUIh9OJG2Nor8SczP9cFIiYOST01NUXeWPELIZM_EC6WXAM0ttvH4E9XoMow9dV6yM7NEEAXLi05quRx6zd5IRVCcumEeV5PKmsubgpzRpw=s2048?key=5Qii6-4OpIi7sWm_Y2ks1A)

For the present CFD model, **12 W per board** is used as a conservative heat-load assumption. This is intentionally different from the measured ~8 W value and is one of the parameters being examined in sensitivity studies.

A second run shows power behavior alongside the large-cluster temperature signal:

![Power versus cluster temperature](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdgMeOs3K4K05Z1-IFLXRpuGEYd_sdWCsk0XTHQz9eqtXc4dzluI6CV-QYs0R6X1VKGIwm7mYy_aGzTHv1mT_mdw017sEHCBJCnPJJVBZNZ4n37tue_kxYA2cYI8WG68fjSFCNPmA21qH89rFyf8AOg6JK61DEmxCKFvpSC6_T4lP5crY8=s2048?key=5Qii6-4OpIi7sWm_Y2ks1A)

## Cooling approach

The primary cooling mechanism is **forced convection**.

Liquid cooling was not chosen for the current stage because of its cost and implementation complexity relative to the thermal range observed in the phone hardware. The prototype instead uses active air cooling and studies how fan placement, board spacing, inlet geometry, and chassis layout influence temperature uniformity.

Once the boards are removed from their original product enclosure, cooling has to be designed as part of the server rather than inherited from the phone.

## Simulation setup

The current model was run in **SimScale** as a conjugate heat-transfer problem.

The simulation includes solid conduction through the rack, board, shield, and heatsink structures; forced airflow from the fans; heat transfer between the solid and fluid domains; and a thermal interface layer between the board contact face and the heatsink.

The main material mapping is:

- aluminum for chassis/heatsink structures;
- copper for copper heat-spreading regions;
- steel for metallic shielding;
- FR4 for the logic-board substrate;
- silicon for the simplified processor regions.

![Material-region detail](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdXRn7vD8RNIZ_u8amsWKZNX0peFc1hWKA6Vkp-f5zHr1sGlJRS619Oule4ZzR4zipZvbTNunYff-o2wxC9JmMCTiMmQIZnhghj5TWixNWt3D2STSS1E9lyKUnngCvuCpzKcvsWbyGyVf4Dz1K1dSDiyiLye2UZgp60Lt932qrRH8gRTfk=s2048?key=5Qii6-4OpIi7sWm_Y2ks1A)

The board-to-heatsink interface is currently modeled as **1 mm thick with a conductivity of 8 W/(m·K)**.

## Preliminary result

The initial conjugate heat-transfer simulation took approximately **2032 minutes** to complete.

The clearest result from the first run is that the boards toward the **back of the chassis run hottest**, reaching approximately **95 °C** under the modeled conditions.

![Preliminary temperature field](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfL42ildhck0kJqZuCiOrAVF3LwnjJksZX-ElJ9JboV3hD3zNgwmoAG2PxU1BVwUUoqdcyR4ojqF_uTLPr3JaoItzoJoRdB59sDuuxnKbfNd31Sak47ZmKYtx9bI1Hr0TyxcO51mn35l9SFtC98SuW2Hsa_45gmdTeFN9clvTCw8qIE=s2048?key=5Qii6-4OpIi7sWm_Y2ks1A)

A second simulation view of the rack:

![Thermal model view](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc1j0O9zxpV2RKxnsdhHkYYPhgeedwc8E4uhR15WAiZny-an4nxoa90Y_ANem2VmxhiKGgDqF57keGAPerAQbfMXQLjXdkQB-5t54iuzG3wirJuc9pWdvKgr0tFrxFd1kNEhcTDC14T6g1h8k3aZNzx6O6Z-7-G-lwbrqzbIfIplbmkmZ0=s2048?key=5Qii6-4OpIi7sWm_Y2ks1A)

This result points to a lack of uniform inlet access near the rear boards and suggests that the four front fans should be spaced so that air reaches the board cluster more evenly.

The next question is not only *which boards are hottest?* but *why did the air travel that way?* Particle-tracer and velocity-field views are therefore particularly useful for the next iteration.

## Physical hardware reference

The rack/board arrangement is also being compared against the physical hardware assembly:

![Physical board rack](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcjarj4QGr_GKhRw3D4lbRoloHsTcI4sljX_tjXR1AE5XBHPfCSGl4vnVZTNgqeczOn3vT9ysq1ZM-jbjTsiR33tT3PN323_uH3XEU9qV14hK02RhuYBYOzB9P3gejW3LwDvMpRw1X55Qu7_uqzziYG3WVEAgCpGqZscDNJ4ySJDJNo=s2048?key=5Qii6-4OpIi7sWm_Y2ks1A)

## Current engineering questions

The next simulation stage is intentionally about exposing engineering tradeoffs rather than producing one prettier temperature contour.

The main variables being perturbed are:

- fan positions and spacing;
- inlet and outlet geometry;
- board spacing and position;
- heatsink geometry;
- component-shape simplifications;
- heat load per board;
- airflow restrictions and bypass paths;
- possible shrouds or baffles.

The comparison metrics will include, where available, maximum board temperature, average board temperature, hottest-to-coolest temperature spread, airflow uniformity, pressure drop, fan power, and sensitivity to the 8 W versus 12 W phone load.

A good design is not necessarily the one with the lowest single temperature. It should also cool the boards **uniformly**, remain mechanically simple, fit the 2U envelope, and avoid disproportionate fan power or cooling complexity.

## What is measured vs. assumed

### Measured / observed

- approximate sustained phone power draw under stress;
- internal device temperature during stress testing;
- physical dimensions used for CAD;
- behavior of the physical phone/logic-board hardware;
- the first CFD temperature field.

### Modeled / assumed

- 12 W heat generation per phone in the conservative simulation;
- approximately 100 °C processor-region condition used in the model;
- exact SoC location beneath the shielding;
- simplified small-component geometry;
- 19 °C ambient;
- thermal-interface properties;
- fan boundary conditions based on the selected fan curve;
- omission of fan self-heating from the current CFD model.

These assumptions are documented explicitly because part of the goal of the methods paper is to make the engineering process reproducible.

## Status

The current result should be treated as **preliminary design guidance**, not a validated thermal predictor.

Next steps:

1. Run controlled geometry and component-position perturbations.
2. Compare alternative fan and inlet layouts with identical boundary conditions.
3. Inspect velocity fields / particle traces rather than temperature alone.
4. Perform sensitivity analysis on uncertain model parameters.
5. Validate the simulation against the physical 40-board rack.
6. Document the engineering tradeoffs in the methods paper.

The paper itself is being maintained separately. This repository is intended to show the design process, assumptions, figures, and engineering work behind it while that paper is still being developed.

---

**Research status:** In progress — thermal simulations and design-space exploration are ongoing.

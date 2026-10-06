# Technical Notes

This file collects implementation-level details useful for reproducing or auditing the current thermal model without turning this repository into a copy of the paper.

## System definition

The server prototype uses 40 stripped smartphone logic boards as compute nodes. References to a “phone” in the project generally mean the logic board after removing the display, casing, battery, and other nonessential peripherals.

The hardware work also included finding a practical way to power the boards without individually maintaining the original phone batteries. The custom battery-side PCB used for the original battery/PMIC interaction can be separated and powered from a conventional supply, which is more suitable for rack operation and power logging.

## Rack geometry

- Standard rack width: 19 in
- Chassis depth: 24 in
- Height: 2U
- Compute nodes: 40
- Primary layout: two dense banks of parallel logic boards
- Cooling: front-mounted forced-air fans
- Current fan count: 4

The 19-inch form factor was chosen mainly because it allows reuse of conventional server infrastructure.

## Logic-board CAD simplification

The board is modeled approximately 1:1.

Very small components were omitted where their geometric detail was not expected to materially change the rack-level result. Major conductive structures, including the copper and metallic shield regions, were retained.

The exact processor location is not directly known because the shielding/package was not destructively analyzed. The processor region is therefore represented by simplified internal blocks.

## Experimental characterization

Stress workloads were run while PSU power draw and internal device temperature were logged.

The current data shows sustained power around 8 W in the tested workload. The CFD model uses 12 W per phone as a deliberately conservative modeling assumption. Future comparisons should include both values.

## Thermal model

Current assumptions:

- ambient: 19 °C;
- processor-region condition: approximately 100 °C;
- thermal interface: 8 W/(m·K);
- interface thickness: 1 mm;
- four fans;
- selected fan curve at 100% duty cycle;
- approximately 9 W electrical fan rating;
- fan self-heating omitted from the current simulation.

### Material mapping

| Model region | Material |
|---|---|
| Rack / major heatsink structures | Aluminum |
| Heat-spreading region | Copper |
| Metallic shield | Steel |
| Main PCB | FR4 |
| Simplified processor blocks | Silicon |

## Preliminary CFD observation

The first SimScale conjugate heat-transfer run took approximately 2032 minutes.

The rear part of the board array reached the highest temperatures, approximately 95 °C under the modeled conditions. The current interpretation is that rear inlet access and overall airflow distribution are inadequate.

This is an interpretation of the temperature field and geometry. It still needs airflow visualization and physical validation.

## Validation target

The physical-rack validation should compare simulated and measured temperature at representative front, middle, and rear board positions under a known workload.

Useful measurements include board/device temperature, inlet/outlet temperature, local air velocity, fan flow/pressure information, and thermal imaging where practical.

Until that comparison exists, the model should be presented as a preliminary engineering model rather than a validated predictor.

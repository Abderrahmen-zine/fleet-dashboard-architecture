# Fleet Operations Dashboard: Architecture (Microsoft Fabric and Power BI)

Architecture document for a real-time fleet dashboard covering 800 delivery vehicles across three regions. It combines near-real-time telemetry (five-minute refresh) with historical delivery performance analysed as YTD, Prior Year and Year-over-Year, with a target page load under four seconds.

## Contents

| Path | Description |
|---|---|
| [`docs/Fleet_Dashboard_Architecture.docx`](docs/Fleet_Dashboard_Architecture.docx) | Full architecture document (5 sections) |
| [`kql/telemetry_5min_window.kql`](kql/telemetry_5min_window.kql) | Five-minute windowed telemetry query |
| [`kql/retention_policy.kql`](kql/retention_policy.kql) | Raw telemetry retention policy |
| [`dax/measures.dax`](dax/measures.dax) | Optimized base measures |
| [`dax/calculation_group.dax`](dax/calculation_group.dax) | Time Intelligence calculation group items |
| [`semantic-model/field_parameter.dax`](semantic-model/field_parameter.dax) | Field parameter for switching measures |

## Architecture at a glance

1. **Real-Time Intelligence:** Azure IoT Hub, Eventstream (filter and field mapping), Eventhouse table `VehicleTelemetry`, 60-day raw retention.
2. **Semantic model:** star schema (`FactDeliveries`, `DimDate`, `DimVehicle`, `DimRoute`), many-to-one single-direction relationships, Date and Geography hierarchies.
3. **DAX:** VAR-based measures over column references, one calculation group (Current, YTD, PY, YoY, YoY %).
4. **Performance tuning:** incremental refresh (3-year archive, 7-day incremental window), Import-mode aggregation table, auto date/time disabled.
5. **Dashboard:** three pages (Fleet Live View, Delivery Performance, Vehicle Detail), DirectQuery to Eventhouse for live data.

## Note on figures

This is a design document for a hypothetical scenario. The Performance Analyzer timings it reports (for example the 2.8 s final page load) are illustrative benchmarks, not measurements from a deployed system.

## Skills demonstrated

KQL, Eventstream and Eventhouse, dimensional modelling, DAX optimization, calculation groups, incremental refresh, aggregations, Power BI performance tuning.

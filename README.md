# dynamic-parking-pricing
Urban parking lots often suffer from inefficiencies due to static pricing. This project builds a dynamic pricing system for 14 urban parking spaces, using real-time data (occupancy, traffic, events, vehicle types) to adjust prices intelligently. The goal is to improve utilization and maximize revenue.
---

## Tech Stack

- Python 3.x
- Pandas
- Numpy
- Pathway (real-time streaming)
- Bokeh (interactive visualization)
- Google Colab
- GitHub

---
## Architecture Diagram

![Architecture Diagram](./architecture.svg)

## Detailed Explanation of Project Architecture and Workflow

The project implements a real-time dynamic pricing engine for urban parking lots using multiple data sources and machine learning logic. Here’s how it works:

1. **Data Ingestion (Pathway Stream Processor):**
    - The dataset simulates real-time streaming of parking data.
    - Data arrives at 30-minute intervals across 14 urban parking lots for 73 days.
    - Pathway reads the CSV file as a streaming data source, delivering each row as a simulated real-time event.

2. **Feature Engineering:**
    - Compute key metrics for each parking lot at every time step:
        - Occupancy Rate = Occupancy / Capacity
        - Queue Length
        - Nearby Traffic Condition (mapped to numeric levels)
        - Special Day Indicator (e.g. events or holidays)
        - Vehicle Type Weight (e.g. Car = 1, Bike = 0.5, Truck = 2)

3. **Dynamic Pricing Logic:**
    - **Model 1 (Baseline):**
        - Price increases linearly with Occupancy Rate.
    - **Model 2 (Demand-Based):**
        - Calculates a demand score using a mathematical function:
            ```
            Demand = α*(Occupancy/Capacity) + β*(QueueLength) - γ*(Traffic) + δ*(SpecialDay) + ε*(VehicleTypeWeight)
            ```
        - Demand is normalized and price is adjusted accordingly:
            ```
            Price = BasePrice * (1 + λ * NormalizedDemand)
            ```
        - Price is constrained between $5 and $20 for stability.
    - **Model 3 (Competitive Pricing):**
        - Calculates distance between parking lots using latitude and longitude (haversine formula).
        - Adjusts price based on competitor prices:
            - Lowers price if nearby lots are cheaper.
            - Raises price if competitors are more expensive and occupancy allows.
        - May suggest rerouting vehicles if lots are full.

4. **Real-Time Processing:**
    - Pathway processes incoming rows in real-time.
    - Pricing is updated dynamically for each lot and each time slot.
    - Outputs updated prices continuously for further analytics or dashboards.

5. **Visualization (Bokeh):**
    - Real-time plots of:
        - Dynamic prices over time.
        - Occupancy trends vs price.
        - Optional competitor price comparisons.
    - Visuals help validate the dynamic pricing logic and interpret model behavior.

6. **Output:**
    - Updated pricing for each parking lot is saved to an output CSV or displayed on interactive dashboards.

---
- **References:**
    - [Pathway Docs - From Jupyter to Deploy](https://pathway.com/developers/user-guide/deployment/from-jupyter-to-deploy/)
    - [Pathway Docs - First Real-Time App](https://pathway.com/developers/user-guide/introduction/first_realtime_app_with_pathway/)
    - [Summer Analytics 2025](https://www.caciitg.com/sa/course25/)

# capstone_project_summer_analytics

this project is made as a part of final project for summer analytics - IIT guwahati - 2025

# features 
- dynamic parking prices
- parking prices plot 

# Data description 
- given the data collected from 14 urban parking spaces over 73 days, sampled at18 time points per day with 30 minutes of time difference (from 8:00 AM to 4:30
PM the same day).

it included the following feature 

- Latitude and Longitude of each parking space (to calculate proximity to competitors).
- Capacity (maximum number of vehicles that can be parked)
- Occupancy (current number of parked vehicles)
- Queue length (vehicles waiting for entry)
- Type of incoming vehicle: car, bike, or truck
- Nearby traffic congestion level

- Special day indicator (e.g., holidays, events)

## Objective of this project
goal is to build a dynamic pricing model for each parking space such that:
• The price is realistic
lly updated in real-time based on:
-  Historical occupancy patterns
-  Queue length
-  Nearby traffic
-  Special events
-  Vehicle type
-  Competitor parking prices
-  It starts from a base price of $10
-  The price variation is smooth and explainable, not erratic

## Tech stack used 
- Python 
- pandas 
- numpy 
- Pathway
- Bokeh plots
# Project Architecture
The architecture of this project starts with a CSV file that contains historical parking data. This file includes columns such as timestamp, occupancy, capacity, latitude, longitude, type of vehicle, traffic condition nearby, queue length, and a special day flag.

The data is first preprocessed using pandas. This involves applying ordinal encoding to convert categorical columns like VehicleType and TrafficConditionNearby into numeric values so they can be used in mathematical computations. The cleaned data is then saved back to a CSV file which becomes the input for the next stage.

Next, the project uses Pathway to simulate a real-time data stream. Instead of reading the CSV all at once, Pathway’s replay function feeds the data line by line at a specified input rate, mimicking how data would come from live sensors in a smart parking environment.

Once the data is streaming, Pathway groups the data into daily windows. It does this by applying a tumbling window over the timestamp, effectively partitioning the data by each day. For each day, it reduces the data to key summary statistics such as maximum occupancy, maximum capacity, average queue length, and the maximum values seen for traffic condition, vehicle type, and whether it was a special day.

These aggregated daily statistics are then fed into a pricing function. In Model 1, the price depends simply on occupancy. In Model 2, it is calculated based on multiple factors: occupancy rate, average queue, traffic condition, type of vehicle, and the special day flag, each with a different weight. The final formula is designed to keep the price roughly between 10 and 20 rupees, directly using mathematical operations without intermediate variables.

Finally, the dynamically calculated prices are plotted over time using Panel and Bokeh. This creates an interactive visualization that shows how the price changes day by day based on demand and other influencing factors, allowing easy monitoring of the dynamic pricing strategy in action.
![Untitled diagram _ Mermaid Chart-2025-07-07-035756](https://github.com/user-attachments/assets/e89a2629-975b-4c89-a3e0-8b480560be83)

## Model 1 - Baseline Linear Model
 A simple model where the next price is a function of the previous price and current
occupancy:
- Linear price increase as occupancy increases
- Acts as a reference point
# Plot of parking price with respect to time 
  ![Screenshot 2025-07-06 100008](https://github.com/user-attachments/assets/d6c1c323-4d90-40d9-9425-a954df116e38)

- explaination
- # Compute the price using a simple dynamic pricing formula:
        # Pricing Formula:
        #     price = base_price + demand_fluctuation
        #     where:
        #         base_price = 10 (fixed minimum price)
        #         demand_fluctuation = (occ_max - occ_min) / cap
        #
        # Intuition:
        # - The greater the difference between peak and low occupancy in a day,
        #   the more volatile the demand is, indicating potential scarcity.
        # - Dividing by capacity normalizes the fluctuation (to stay in [0,1] range).
        # - This fluctuation is added to the base price of 10 to set the final price.
        # - Example: If occ_max = 90, occ_min = 30, cap = 100
        #            => price = 10 + (90 - 30)/100 = 10 + 0.6 = 10.6

## Model-2 Demand-Based Price Function

mathematical demand function using key features:
- Occupancy rate
- Queue length
- Traffic level
- Special day
- Vehicle type

- explanation
  
- This pricing model is designed such that the dynamic parking price will always stay
  in the range of 10 to 20 rupees, depending on several key factors:
 
  - Availability of spaces relative to total capacity (occupancy rate)
  - Average queue length at the time interval
  - Traffic condition nearby (encoded as low=0, average=1, high=2)
  - Type of vehicle (car, bike, cycle, truck encoded as 0-3)
  - Whether the day is a special day or a regular working day (special flag)
 
  The occupancy rate is considered the strongest driver of price,followed by the average queue length.
 
  The traffic value has a negative impact on demand: worse traffic (higher number) slightly reduces the demand, so it subtracts from the demand function.
  The vehicle type and special day contribute positively,slightly raising the demand and thus the price.
 
  The overall demand function is defined as:
  demand = (
     0.6 * (pw.this.occ_max / pw.this.cap) +
     0.3 * (pw.this.queue_avg / 10) -
     0.1 * pw.this.traffic_value +
     0.1 * pw.this.vehicle_value +
     0.05 * pw.this.special_flag
  )
 
  Since this demand is roughly normalized between 0 and 1 (via inline normalization in the formula), we compute the final price as: price = 10 + 10 * demand
  
  # Plot of parking price with respect to time
   ![Screenshot 2025-07-06 100027](https://github.com/user-attachments/assets/64eeb606-1b55-4e05-ace4-99a951a58591)




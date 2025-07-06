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
• The price is realistically updated in real-time based on:
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

## Model 1 - Baseline Linear Model
 A simple model where the next price is a function of the previous price and current
occupancy:
- Linear price increase as occupancy increases
- Acts as a reference point

## Model-2 Demand-Based Price Function

mathematical demand function using key features:
- Occupancy rate
- Queue length
- Traffic level
- Special day
- Vehicle type





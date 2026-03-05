# API Weather Report Dashboard

## Project Overview

This project focuses on building an automated weather analytics dashboard by integrating data from a weather API and transforming it into structured insights using Power BI. The goal of the project was to demonstrate how real-time API data can be collected, cleaned, modeled, and visualized to support monitoring and decision-making.

Weather data is often consumed in raw JSON format from APIs, which is not immediately usable for analysis. This project addresses that challenge by transforming API responses into a structured dataset and presenting key weather metrics through an interactive dashboard.

---

## Project Objective

The primary objective of this project was to:

* Extract weather data from an external API source
* Transform and structure the data for analysis
* Build an interactive dashboard to monitor weather patterns
* Demonstrate how API-based datasets can be integrated into business intelligence workflows

The project simulates a real-world scenario where organizations rely on external APIs to power dashboards used for operational monitoring and reporting.

---

## Tools and Technologies

* **Power BI Desktop**
* **REST API Integration**
* **Power Query**
* **Data Modeling**
* **DAX**
* **JSON Data Transformation**

---

## Data Collection

Weather data was collected using a public weather API. The API provides real-time and forecast weather information in JSON format.

Using Power BI's web connector and Power Query, the API response was imported and parsed into structured tables.

Key steps included:

* Connecting to the API endpoint
* Expanding JSON objects into tabular format
* Selecting relevant weather attributes
* Standardizing field names and formats

---

## Data Preparation

Since API responses contain nested JSON structures, the data required transformation before analysis.

Data preparation included:

* Expanding nested JSON fields
* Converting timestamps into readable date formats
* Removing irrelevant fields
* Structuring the dataset for dashboard use

These steps ensured the dataset was clean, consistent, and suitable for building visualizations.

---

## Dashboard Design

The dashboard was designed to provide a quick overview of weather conditions and trends.

The visualizations were selected to help users quickly interpret weather conditions and monitor patterns.

### Key Metrics Displayed

* Temperature
* Humidity
* Wind Speed
* Weather Conditions
* Forecast Trends

---

## DAX Queries
*AQI_status_text = 
VAR AQI = ROUND (SELECTEDVALUE ('Current'[current.air_quality.pm10]), 0)
RETURN
    SWITCH(
        TRUE(),
        AQI <= 54, "Good",    --Good (Green)
        AQI <= 154, "Moderate",   --Moderate (Yellow)
        AQI <= 254, "Poor",   --Poor (Orange) 
        AQI <= 354, "Unhealthy",   --Unhealthy (Red)                -- Used to get the alert status for the PM10 (AQI)
        AQI <= 424, "Severe",   -- Severe (Purple)
        "Hazardous"                --Hazardous (Dark Maroon)
    )
    
*CO Color = 
VAR AQI = ROUND (SELECTEDVALUE ('Current'[current.air_quality.co]), 0)
RETURN
    SWITCH(
        TRUE(),
        AQI <= 50, "#43d946",    --Good (Green)
        AQI <= 100, "#fff570",   --Moderate (Yellow)
        AQI <= 150, "#ff9800",   --Poor (Orange) 
        AQI <= 200, "#d99343",   --Unhealthy (Red)                -- Used to get the alert for CO (AQI)
        AQI <= 300, "#ff5b0f",   -- Severe (Purple)
        "#d95243"                --Hazardous (Dark Maroon)
    )

*Current_temp_C = SUM('Current'[current.temp_c]) & " °C "         -- Used to get the current temparature in Degree celsius KPI

*Fore_Current_temp_C = AVERAGE(Forecast_Day[forecast.forecastday.day.avgtemp_c]) & " °C "    -- Used to get the forecast temperature in degree celsius

*Humidity = SUM('Current'[current.humidity])& " %"                -- Used to get the current humidity in percentage KPI

*Last_updated_date_current = "last updated, "&FORMAT(FIRSTNONBLANK('Current'[current.last_updated].[Date]," "),"dd/mm/yy")     -- Used to get the last updated date

*Left_value_PM10 = [MAxValue] - SUM('Current'[current.air_quality.pm10])        -- Used to gte the alert for the max AQI level for PM10

*Left_value_rain = 100 - SUM('Forecast_Day'[forecast.forecastday.day.daily_chance_of_rain]) -- Used to get the value for chances of rain for 100% stacked bar chart

*MAxValue = 424           -- The max value for the alert of PM10

*NO2 Color = 
VAR AQI = ROUND (SELECTEDVALUE ('Current'[current.air_quality.no2]), 0)
RETURN
    SWITCH(
        TRUE(),
        AQI <= 53, "#43d946",    --Good (Green)
        AQI <= 100, "#fff570",   --Moderate (Yellow)                   -- Used to get the alert for NO2 (AQI)
        AQI <= 360, "#ff9800",   --Poor (Orange) 
        AQI <= 649, "#d99343",   --Unhealthy (Red)
        AQI <= 1249, "#ff5b0f",   -- Severe (Purple)
        "#d95243"                --Hazardous (Dark Maroon)
    )

*O3 Color = 
VAR AQI = ROUND (SELECTEDVALUE ('Current'[current.air_quality.o3]), 0)
RETURN
    SWITCH(
        TRUE(),
        AQI <= 50, "#43d946",    --Good (Green)
        AQI <= 100, "#fff570",   --Moderate (Yellow)                 -- Used to get the alert for O3 (AQI)
        AQI <= 150, "#ff9800",   --Poor (Orange) 
        AQI <= 200, "#d99343",   --Unhealthy (Red)
        AQI <= 300, "#ff5b0f",   -- Severe (Purple)
        "#d95243"                --Hazardous (Dark Maroon)
    )

*Perciptation/rainfall = SUM('Current'[current.precip_mm])&" mm"    -- Used to get the current rainfall/perciptation KPI

*PM10 Color = 
VAR AQI = ROUND (SELECTEDVALUE ('Current'[current.air_quality.pm10]), 0)
RETURN
    SWITCH(
        TRUE(),
        AQI <= 54, "#43d946",    --Good (Green)
        AQI <= 154, "#fff570",   --Moderate (Yellow)
        AQI <= 254, "#ff9800",   --Poor (Orange)                      -- Used to get the alert for PM10 (AQI)
        AQI <= 354, "#d99343",   --Unhealthy (Red)
        AQI <= 424, "#ff5b0f",   -- Severe (Purple)
        "#d95243"                --Hazardous (Dark Maroon)
    )

*PM2.5 Color = 
VAR AQI = ROUND (SELECTEDVALUE ('Current'[current.air_quality.pm2_5]), 0)
RETURN
    SWITCH(
        TRUE(),
        AQI <= 12, "#43d946",    --Good (Green)
        AQI <= 35.4, "#fff570",   --Moderate (Yellow)                 -- Used to get the alert for PM2.5 (AQI)
        AQI <= 55.4, "#ff9800",   --Poor (Orange) 
        AQI <= 150.4, "#d99343",   --Unhealthy (Red)
        AQI <= 250.4, "#ff5b0f",   -- Severe (Purple)
        "#d95243"                --Hazardous (Dark Maroon)
    )

*Pressure = SUM('Current'[current.pressure_in])&" Pa"                 -- Used ot get the current atmospheric pressure KPI

*SO2 Color = 
VAR AQI = ROUND (SELECTEDVALUE ('Current'[current.air_quality.so2]), 0)
RETURN
    SWITCH(
        TRUE(),
        AQI <= 35, "#43d946",    --Good (Green)
        AQI <= 75, "#fff570",   --Moderate (Yellow)
        AQI <= 185, "#ff9800",   --Poor (Orange)                     -- Used to get the alert for SO2 (AQI)
        AQI <= 304, "#d99343",   --Unhealthy (Red)
        AQI <= 604, "#ff5b0f",   -- Severe (Purple)
        "#d95243"                --Hazardous (Dark Maroon)
    )

*Visibility = SUM('Current'[current.vis_km])&" km"                 -- Used to get the current visibility KPI

*Wind_Speed = SUM('Current'[current.wind_kph])&" Kph"               -- Used to get the current wind speed KPI

*Day_Name = FORMAT(Forecast_Day[forecast.forecastday.date].[Day],"ddd")   -- Used to extract the day from the forecast_day date column

*Sunrise_date = FORMAT(Forecast_Day[forecast.forecastday.astro.sunrise], "hh:mm AM/PM")    -- Used to get the Sunrise time from the forecast_day

*Sunset_date = FORMAT(Forecast_Day[forecast.forecastday.astro.sunset], "hh:mm AM/PM")      -- Used to get the Sunset time from the forecast_day 

--

## Visualization Rationale


![Dashboard_upload]()


### Temperature Trend Visualization

A line chart was used to display temperature changes over time. This visualization helps users understand temperature fluctuations and identify patterns such as warming or cooling trends.

### Weather Condition Overview

A categorical visualization was used to summarize weather conditions such as clear, cloudy, or rainy. This allows quick identification of dominant weather patterns.

### Wind and Humidity Indicators

Gauge-style or KPI visuals were used to highlight current wind speed and humidity levels. These indicators provide quick insights into environmental conditions without requiring detailed analysis.

---

# Dashboard Insights and Visualization Rationale

The Weather API Dashboard provides a consolidated view of current weather conditions, short-term forecasts, and environmental indicators for quick monitoring. Each visualization was chosen to communicate specific insights efficiently for **Leh region**.

---

# 1. Current Weather Overview (KPI Card)

### Insight

The dashboard shows the **current temperature in Leh as -1.8°C with sunny conditions**, indicating extremely cold weather conditions typical of high-altitude regions.

Additional cities such as **Bengaluru (30.4°C) and Hyderabad (36°C)** highlight a strong regional temperature contrast across India.

### Why this visual is used

A **KPI card** is ideal for displaying a single key metric such as current temperature.

### Importance

* Provides **instant weather status**
* Helps users quickly assess conditions without analyzing charts
* Useful for real-time monitoring dashboards

---

# 2. Forecast Weather (Line Chart)

### Insight

The temperature forecast shows a **gradual cooling trend**, dropping from approximately **-5.2°C to -6.9°C** over the upcoming days.

This indicates:

* A **stable but slightly declining temperature trend**
* Continued cold conditions for the region

### Why a Line Chart is Used

Line charts are ideal for **time-series data**, such as weather forecasts.

### Importance

* Shows **temperature trends over time**
* Helps identify **warming or cooling patterns**
* Useful for planning travel, logistics, or outdoor activities

---

# 3. Sunrise and Sunset (Information Cards)

### Insight

* **Sunrise:** 06:36 AM
* **Sunset:** 06:19 PM

This indicates approximately **11 hours and 43 minutes of daylight**.

### Why this visual is used

Simple information cards are used because the data represents **single daily values rather than trends**.

### Importance

* Helps understand **daylight duration**
* Important for **travel planning and outdoor activities**
* Useful for agriculture and solar energy monitoring

---

# 4. Weather Metrics Panel (Multi-KPI Cards)

Metrics shown include:

| Metric        | Value    | Insight                     |
| ------------- | -------- | --------------------------- |
| Humidity      | 53%      | Moderate humidity level     |
| Wind Speed    | 5.8 Kph  | Low wind intensity          |
| Visibility    | 10 km    | Clear visibility            |
| Pressure      | 30.23 Pa | Stable atmospheric pressure |
| UV Index      | 10.10    | High UV exposure            |
| Precipitation | 0 mm     | No rainfall                 |

### Why KPI Cards are Used

Cards provide **quick summaries of key environmental indicators** without requiring complex interpretation.

### Importance

* Enables **fast situational awareness**
* Essential for **weather monitoring systems**
* Supports **operational decision-making**

---

# 5. Air Quality Index (Gauge Chart)

### Insight

The **PM10 level is 19**, categorized as **Good air quality**.

Other pollutants measured include:

* CO
* NO₂
* SO₂
* O₃
* PM2.5

All values appear relatively low, indicating **clean air conditions**.

### Why a Gauge Chart is Used

Gauge charts are commonly used for **performance indicators with defined thresholds**.

### Importance

* Quickly communicates **air quality status**
* Helps classify pollution levels visually
* Useful for **public health monitoring**

---

# 6. Pollutant Breakdown (Indicator Metrics)

### Insight

The pollutant metrics show:

* CO: 297
* NO₂: 8
* SO₂: 1
* O₃: 107
* PM2.5: 8

These values suggest **low industrial pollution levels**, likely due to Leh's **low population density and limited industrial activity**.

### Why this visualization is used

Numeric indicators allow users to **monitor pollutant concentrations individually**.

### Importance

* Helps identify **specific pollution sources**
* Useful for **environmental monitoring**
* Supports air quality management

---

# 7. Chances of Rain (Horizontal Bar Chart)

### Insight

Rain probability increases on:

* **Saturday – 62%**
* **Sunday – 67%**

This indicates a higher likelihood of precipitation during the weekend.

### Why a Horizontal Bar Chart is Used

Horizontal bar charts are effective for **comparing categorical probabilities across multiple days**.

### Importance

* Makes rainfall probability easy to compare
* Helps users quickly identify **high-risk rain days**
* Supports **travel and logistics planning**

---

# Overall Dashboard Insights

From the combined visualizations, we can conclude:

* The region is experiencing **cold but stable weather conditions**
* **Air quality is good**, indicating minimal pollution
* **Weekend rain probability increases**
* Weather indicators show **low wind and moderate humidity**
* Temperature forecasts suggest **continued cold conditions**

---

# Business / Real-World Use Cases

This type of dashboard can support:

* **Travel planning platforms**
* **Agriculture monitoring systems**
* **Environmental monitoring**
* **Logistics and transportation planning**
* **Smart city weather intelligence systems**

---

## Key Outcomes

* Successfully integrated API-based data into Power BI.
* Demonstrated the ability to transform nested JSON data into structured datasets.
* Built a dashboard capable of visualizing dynamic external data sources.
* Showcased practical experience with real-time data pipelines.

---

## Skills Demonstrated

* API Data Integration
* Data Cleaning and Transformation
* Power BI Dashboard Development
* Data Modeling
* DAX Query
* Power Query
* Business Intelligence Reporting

---

## Project Significance

Many modern analytics systems rely on external APIs to power dashboards and monitoring systems. This project demonstrates how API-driven data pipelines can be built and visualized effectively using Power BI.

It highlights the process of converting raw external data into structured insights that support monitoring, reporting, and decision-making.

---

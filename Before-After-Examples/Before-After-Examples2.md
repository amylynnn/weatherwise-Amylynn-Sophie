---
title: "Before/After Example: interactive_temperature_plot and interactive_precipitation_plot functions"
author: "Amylynn Sophie"
---


## Initial Prompt

```
Write a function that visualizes the temperature and precipitation forecast using interactive widgets so that users can explore weather data over time.
```

## Initial AI-Generated Code (Before)

```python
def plot_weather(data):
    times = [entry['dt_txt'] for entry in data['list']]
    temps = [entry['main']['temp'] for entry in data['list']]
    plt.plot(times, temps)
    plt.show()

```

## My Analysis of the Initial Code

The function technically works, but it has some limitations:
- Not interactive; shows all data at once
- Labels unreadable without rotation or formatting
- Doesn’t support precipitation or other variables
- No info for future developers
- Can’t adjust how much data to view

## My Follow-up Prompt

```
Can you make this more interactive using ipywidgets?
I’d like the user to control how many hours/days of forecast data they see using a slider.
Please write separate functions for temperature and precipitation. Add proper layout and docstrings.
```

## Improved AI-Generated Code (After)

```python

def interactive_temperature_plot(data, city):
    """
    Show an interactive temperature line chart with a slider.
    
    Args:
        data (dict): Weather data as returned from API
        city (str): City name for labeling the chart

    Provides:
        A matplotlib plot with a slider to control how many forecast entries are displayed.
    """
    # Extract timestamps and temperature values
    times = [datetime.strptime(entry['dt_txt'], "%Y-%m-%d %H:%M:%S") for entry in data['list']]
    temps = [entry['main']['temp'] for entry in data['list']]

    # Inner function to dynamically update the plot
    def plot_upto(index=8):
        plt.figure(figsize=(12, 6))
        plt.plot(times[:index], temps[:index], marker='o', linestyle='-', color='orange')
        plt.title(f"Temperature Forecast for {city.title()}")
        plt.xlabel("Date & Time")
        plt.ylabel("Temperature (°C)")
        plt.xticks(rotation=45)
        plt.grid(True)
        plt.tight_layout()
        plt.show()

    # Create the interactive slider
    widgets.interact(plot_upto, index=widgets.IntSlider(min=1, max=len(times), step=1, value=8))


def interactive_precipitation_plot(data, city):
    """
    Show an interactive precipitation bar chart with a slider.

    Args:
        data (dict): Weather data as returned from API
        city (str): City name for labeling the chart

    Provides:
        A matplotlib bar chart with a slider to view rainfall data over time.
    """
    times = [datetime.strptime(entry['dt_txt'], "%Y-%m-%d %H:%M:%S") for entry in data['list']]
    rain = [entry.get('rain', {}).get('3h', 0) for entry in data['list']]

    def plot_upto(index=8):
        plt.figure(figsize=(12, 6))
        plt.bar(times[:index], rain[:index], width=0.1, color='blue', edgecolor='black')
        plt.title(f"Precipitation Forecast for {city.title()} (Next 5 Days)")
        plt.xlabel("Date & Time")
        plt.ylabel("Rain (mm in 3h)")
        plt.xticks(rotation=45)
        plt.grid(axis='y', linestyle='--', alpha=0.5)
        plt.tight_layout()
        plt.show()

    widgets.interact(plot_upto, index=widgets.IntSlider(min=1, max=len(times), step=1, value=8))


```

## Why My Prompting Strategy Was Effective

My follow-up prompt was effective because it:
- Empowers user to explore the forecast progressively
- Line & bar charts	Clear separation between temperature and rain
-	Makes future customization easy
- Explains inputs and behavior for collaborators
- Polished; visuals	Grid, ticks, labels, and sizing all considered

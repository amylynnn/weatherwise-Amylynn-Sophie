---
title: "Before/After Example: Weather Data Function"
author: "Amylynn Sophie"
---


## Initial Prompt

```
Can you write a function to fetch weather data for a given city using the OpenWeatherMap API?
 I'd like the function to support using mock data for testing, and ideally avoid unnecessary API calls by caching previous results for a short time.
```

## Initial AI-Generated Code (Before)

```python
import requests

def get_weather_data(city):
    url = f"http://api.openweathermap.org/data/2.5/forecast?q={city}&appid=YOUR_API_KEY&units=metric"
    response = requests.get(url)
    return response.json()

```

## My Analysis of the Initial Code

The function technically works, but it has some limitations:
- Will crash on bad city names, network failure, or bad responses
- No support for alternative data sources or mock data
- Re-fetches the same data repeatedly, causing API overuse
- No docstring, comments, or parameter validation
- Can’t test or extend easily

## My Follow-up Prompt

```
Thanks. Can you improve this function to support mock data generation as a fallback or testing mode, use caching to prevent redundant API calls,
and include docstrings and input validation?
It should also support switching between 'openweathermap' and a 'mocksource' as a parameter.
```

## Improved AI-Generated Code (After)

```python
from datetime import datetime, timedelta
import requests

# Global in-memory cache
weather_cache = {}
CACHE_EXPIRY_MINUTES = 10

def fetch_weather_data(city, source='openweathermap'):
    """Fetch weather data for a city, using cache if available and valid."""
    key = (source.lower(), city.lower())
    now = datetime.utcnow()

    # Use cache if valid
    if key in weather_cache:
        timestamp, cached_data = weather_cache[key]
        if now - timestamp < timedelta(minutes=CACHE_EXPIRY_MINUTES):
            return cached_data

    # Otherwise, fetch from source
    if source.lower() == 'openweathermap':
        data = fetch_openweathermap(city)
    elif source.lower() == 'mocksource':
        data = fetch_mock_source(city)
    else:
        raise ValueError(f"Unsupported weather data source: {source}")

    # Cache the result
    weather_cache[key] = (now, data)
    return data

def fetch_openweathermap(city):
    """Call OpenWeatherMap API to get weather forecast for the given city."""
    url = f"http://api.openweathermap.org/data/2.5/forecast?q={city}&appid={API_KEY}&units=metric"
    response = requests.get(url)
    response.raise_for_status()
    return response.json()

def fetch_mock_source(city):
    """Generate random mock weather data for testing/demo."""
    from random import uniform, randint
    now = datetime.utcnow()
    list_data = []
    for i in range(40):  # 5 days, 3-hour intervals
        dt = now + timedelta(hours=3*i)
        dt_txt = dt.strftime("%Y-%m-%d %H:%M:%S")
        temp = round(uniform(10, 25), 1)
        rain = round(uniform(0, 5), 1) if randint(0, 1) else 0
        snow = round(uniform(0, 2), 1) if randint(0, 3) == 0 else 0
        weather_desc = 'clear sky' if randint(0, 2) == 0 else 'rain'
        entry = {
            'dt_txt': dt_txt,
            'main': {'temp': temp},
            'rain': {'3h': rain} if rain > 0 else {},
            'snow': {'3h': snow} if snow > 0 else {},
            'weather': [{'description': weather_desc}],
            'wind': {'speed': round(uniform(0, 10), 1), 'deg': randint(0, 360)}
        }
        list_data.append(entry)
    return {'list': list_data}

```

## Why My Prompting Strategy Was Effective

My follow-up prompt was effective because:
- It supports multiple sources
- It caches results to reduce API traffic
- It handles errors gracefully through raise_for_status()
- It is modular and extensible
- It can be reused in UI / CLI / API settings
- It is testable


---
title: "Before/After Example: parse_weather_question() function"
author: "Amylynn Sophie"
---


## Initial Prompt

```
Write a function that can interpret a natural language weather question (e.g. 'Will it rain in Paris tomorrow?') and extract the user’s intent:
 weather condition, city, and time frame.
```

## Initial AI-Generated Code (Before)

```python
def parse_weather_question(question):
    match = re.search(r"(rain|snow|temperature).* in (\w+)", question.lower())
    if match:
        condition = match.group(1)
        city = match.group(2).title()
    else:
        condition = "general"
        city = "Unknown"
    return {'condition': condition, 'city': city, 'time_frame': 'today'}

```

## My Analysis of the Initial Code

The initial code has several issues:
- Overly simplistic:	Only handles rain/snow/temperature and assumes basic sentence structure
- Poor city detection:	Doesn’t account for multi-word cities like New York or San Francisco
- No time parsing:	Always defaults to "today"
- No synonyms:	Fails to catch variations like "will it drizzle?" or "windy in Tokyo tomorrow?"
- No error fallback:	Doesn’t gracefully handle missing parts
- No normalization:	Doesn't map cities to more API-friendly formats (like Paris → Paris,FR)


## My Follow-up Prompt

```
Improve the parse_weather_question() function to:

1. Support synonyms like 'drizzle' for 'rain', etc.

2. Detect time frames (today, tomorrow, weekend, next N days)

3. Identify multi-word cities robustly

4. Normalize common cities for API compatibility

5. Return a dictionary with condition, city, and time_frame

## Improved AI-Generated Code (After)

```python
def parse_weather_question(question):
    """Parse user's question to extract weather condition, city, and time frame."""
    question_lower = question.lower()

    # --- Weather Condition Detection ---
    condition_keywords = {
        'rain': ['rain', 'drizzle', 'shower'],
        'snow': ['snow', 'blizzard', 'flurry'],
        'clear': ['clear', 'sunny', 'bright'],
        'cloudy': ['cloudy', 'overcast'],
        'temperature': ['hot', 'cold', 'temperature', 'warm'],
        'wind': ['windy', 'breeze', 'gust'],
        'humidity': ['humid', 'humidity'],
        'storm': ['storm', 'thunder', 'lightning'],
        'fog': ['fog', 'mist', 'haze']
    }

    condition = None
    for cond, synonyms in condition_keywords.items():
        if any(syn in question_lower for syn in synonyms):
            condition = cond
            break
    if not condition:
        condition = 'general'  # Fallback if no keyword matched

    # --- Time Frame Detection ---
    if 'day after tomorrow' in question_lower:
        time_frame = 'day_after_tomorrow'
    elif 'tomorrow' in question_lower:
        time_frame = 'tomorrow'
    elif 'today' in question_lower:
        time_frame = 'today'
    elif 'this weekend' in question_lower:
        time_frame = 'this_weekend'
    elif match := re.search(r'next (\d+) days', question_lower):
        time_frame = f"next_{match.group(1)}_days"
    else:
        time_frame = 'today'  # Default fallback

    # --- City Extraction ---
    city_match = re.search(
        r"in ([a-zA-ZÀ-ÿ\s\-]+?)(?:\s+(?:today|tomorrow|day after tomorrow|next \d+ days|this weekend)|\?|$| ... )",
        question_lower)
    if city_match:
        city = city_match.group(1).strip()
    else:
        # Fallback to last capitalized word group
        capitalized = re.findall(r"\b[A-Z][a-z]+(?:\s[A-Z][a-z]+)*", question)
        city = capitalized[-1] if capitalized else None

    # --- City Normalization ---
    fixes = {
        'paris': 'Paris,FR',
        'new york': 'New York,US',
        'london': 'London,GB',
        'sydney': 'Sydney,AU'
    }

    if city:
        city_key = city.lower()
        city = fixes.get(city_key, city.title())
    else:
        city = None

    return {
        'condition': condition,
        'city': city,
        'time_frame': time_frame
    }

```

## Why My Prompting Strategy Was Effective

My follow-up prompt was effective because:

- Synonym detection:	Supports real-world phrases like "drizzle in Tokyo"
- Time parsing	Understands: today, tomorrow, next 3 days, this weekend
- City extraction:	Robust regex for cities like New York and San Francisco
- City normalization:	Compatible with API requirements like Paris → Paris,FR
- Fallback logic:	Prevents errors when data is missing or malformed

---
title: "Before/After Example: User Interface"
author: "Amylynn Sophie"
---


## Initial Prompt

```
Create an interactive user interface using ipywidgets that allows users to either:

enter a city and get a weather forecast visualization, or

ask a natural language question about the weather.
```

## Initial AI-Generated Code (Before)

```python
def interactive_ui():
    city_input = widgets.Text()
    output = widgets.Output()
    button = widgets.Button(description="Get Weather")

    def on_click(b):
        with output:
            clear_output()
            print(f"Fetching weather for {city_input.value}")

    button.on_click(on_click)
    display(widgets.VBox([city_input, button, output]))

```

## My Analysis of the Initial Code

The initial code has several issues:
- Only supports city input, no question parsing
- Doesn’t leverage temperature or precipitation visualizations
- Empty inputs are not handled
- No button disabling based on input state
- No layout styling,	Very plain structure
- No way to switch between data sources (e.g., mock/openweathermap)


## My Follow-up Prompt

```
Thanks for the initial UI. I’d like to improve the functionality and UX by:

1. Supporting multiple data sources (e.g., OpenWeatherMap and mock data for testing)

2. Adding natural language question parsing so users can ask things like “Will it rain in Paris tomorrow?”

3. Making the temperature and precipitation visualizations interactive (e.g., slider-based exploration)

4. Improving the layout and usability (disable buttons if inputs are empty, clear/reset output, better structure)

5. Handling errors gracefully, especially for missing input, invalid cities, or failed API calls

Also, I want the code to be modular and ready to expand (e.g., plug in new data providers or chart types). Please use ipywidgets for the UI.

## Improved AI-Generated Code (After)

```python
def interactive_ui():
    """Build and display the main weather assistant UI with all widgets and layout."""
    
    # Widgets for inputs
    city_input = widgets.Text(placeholder='Enter city (e.g. Paris)', description='City:')
    question_input = widgets.Text(placeholder='e.g. Will it rain in Paris tomorrow?', description='Ask:')
    source_dropdown = widgets.Dropdown(options=['openweathermap', 'mocksource'], value='openweathermap', description='Source:')
    
    # Output display area
    output_area = widgets.Output()

    # Action buttons
    forecast_btn = widgets.Button(description="📊 Show Forecast Charts", button_style='info')
    question_btn = widgets.Button(description="🧠 Answer My Question", button_style='success')
    clear_btn = widgets.Button(description="🧹 Clear", button_style='warning')

    # Enable/disable logic
    def toggle_buttons(*args):
        forecast_btn.disabled = not bool(city_input.value.strip())
        question_btn.disabled = not bool(question_input.value.strip())

    city_input.observe(toggle_buttons, names='value')
    question_input.observe(toggle_buttons, names='value')
    toggle_buttons()  # Initialize button states

    # Forecast handler
    def on_forecast_button_click(_):
        with output_area:
            clear_output()
            city = city_input.value.strip()
            if not city:
                print("❌ Please enter a city.")
                return
            try:
                data = fetch_weather_data(city, source=source_dropdown.value)
                interactive_temperature_plot(data, city)
                interactive_precipitation_plot(data, city)
            except Exception as e:
                print(f"❌ Error fetching forecast: {e}")

    # Question handler
    def on_question_button_click(_):
        with output_area:
            clear_output()
            question = question_input.value.strip()
            if not question:
                print("❌ Please enter a question.")
                return
            parsed = parse_weather_question(question)
            response = generate_weather_response(parsed, source=source_dropdown.value)
            print(response)

    # Clear handler
    def on_clear_button_click(_):
        city_input.value = ''
        question_input.value = ''
        with output_area:
            clear_output()

    # Attach handlers
    forecast_btn.on_click(on_forecast_button_click)
    question_btn.on_click(on_question_button_click)
    clear_btn.on_click(on_clear_button_click)

    # Layout
    input_row = widgets.HBox([city_input, source_dropdown, forecast_btn])
    question_row = widgets.HBox([question_input, question_btn, clear_btn])
    header = widgets.HTML("<h2 style='color:#2E8B57;'>🌤️ AI-Powered Weather Assistant</h2>")

    main_ui = widgets.VBox([
        header,
        input_row,
        question_row,
        output_area
    ], layout=widgets.Layout(border='2px solid #2E8B57', padding='15px', width='100%', max_width='800px'))

    display(main_ui)

```

## Why My Prompting Strategy Was Effective

- I clearly listed the features I wanted to add or improve (e.g., interactive charts, multiple sources, better input validation).
- I provided numbered requirements, which made it easy for the AI to follow and organize its improvements step-by-step.
- I referred back to the existing function and clarified how it should evolve (not just "make it better" but “add AI-based question handling”).
- I asked for modularity and expandability, showing that the UI wasn’t a one-off — it should scale and evolve.
- By asking for mock data integration and input handling, I made the output more testable and robust, even offline.
- Adding natural language support and slider controls shows I was focused on making the UI user-friendly and engaging — not just technically functional.

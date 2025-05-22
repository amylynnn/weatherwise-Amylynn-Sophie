# 🌦️ WeatherWise - Amylynn Sophie

Welcome to my interactive weather assistant project, powered by real-time weather data and natural language AI! This README will guide you through the project structure, how to run the app, and what each file/folder contains.

---

## 📁 Project Structure

```
├── AmylynnSophieWeatherWise_App.ipynb            # ✅ Main interactive notebook (Run this to use the app)
├── AmylynnSophie_WeatherWise__starter_notebook.ipynb # 📘 Annotated reference notebook with code breakdowns
├── Before-After-Examples/                       # 🔄 Prompting improvements with Before/After code samples
├── ai-conversations/                            # 💬 Example conversations with AI showing evolution of ideas
├── PROMPTING.md                                 # ✍️ Details my prompting strategies and decisions
├── reflection.md                                # 🪞 Final project reflection
└── README.md                                     # 📖 This guide
```

---

## 🚀 How to Use the App (AmylynnSophieWeatherWiseApp.ipynb)

1. **Open the notebook** `AmylynnSophieWeatherWise_App.ipynb` in Google Colab or Jupyter Notebook.
2. **Run the cell**.
3. The interactive UI will appear with input fields.

### 🎯 Features:

* **City Input:** Type a city name (e.g., `Paris`, `London`, `Tokyo`).
* **Weather Visualizations:**

  * Click **📊 Show Forecast Charts** to view interactive temperature and precipitation plots.
  * Use the sliders to control how many forecast points to display.
* **Ask a Weather Question:**

  * Type natural language questions like:

    * “Will it rain in Paris tomorrow?”
    * “Is it going to be cloudy in Sydney today?”
  * Click **🧠 Answer My Question** to get a smart weather response.
* **Choose Weather Data Source:** Select between real API (OpenWeatherMap) or mock source for testing.

---

## 📘 What’s in the Starter Notebook?

The file `AmylynnSophieWeatherWise__starter_notebook.ipynb` breaks the code into logical parts with:

* ✨ **Clear comments** explaining the purpose and logic
* 🔍 **Testing examples**
* 📦 Useful for reviewing or understanding how each module works independently

---

## 🧠 AI Prompting Artifacts

* **Before-After-Examples/**
  Contains snapshots of how prompts evolved and improved specific functions.

* **ai-conversations/**
  Stores transcripts of helpful AI conversations that contributed to the project.

* **PROMPTING.md**
  A write-up of my **prompting strategies**, how I guided the AI, and the improvements made.

* **reflection.md**
  My personal **reflection** on the learning experience, what I achieved, and what I’d do differently.

---

## 💡 Notes for Review

* You will **need an internet connection** to fetch real weather data from the OpenWeatherMap API.
* If the API fails (e.g., due to quota), switch the source to `"mocksource"` to test functionality without live data.
* The `interactive_temperature_plot()` and `interactive_precipitation_plot()` functions use `ipywidgets` for sliders.

---


Thank you for reviewing my project!

— *Amylynn Sophie, 22314110*

---



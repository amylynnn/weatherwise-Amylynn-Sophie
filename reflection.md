# ✍️ Project Reflection

## AI Tools Used
For this project, I primarily used ChatGPT as my AI assistant. 
I also briefly used Perplexity AI for quick inline code suggestions, particularly for syntax and auto-completions within my development environment. 
ChatGPT, however, was my main collaborator—helping me design, implement, and debug the entire weather assistant system. 
It guided me through planning the structure of the codebase, creating reusable functions, and improving features like data caching, natural language parsing, and visualizations. 
I even used it to help me reflect on my code and understand best practices.

## Prompting Techniques
I used intentional and layered prompting:

- I started with clear problem definitions (e.g., "fetch weather data with caching").

- I broke down tasks into subtasks, prompting AI to generate or improve each component (e.g., parsing natural language, visualizing data).

- I provided feedback on AI-generated code (e.g., requesting better error handling or adding input validation) to refine the output.

- I used specific follow-up prompts such as “Add documentation,” “Make it interactive,” and “Explain this part.”
 When I noticed something didn’t work as expected, I used debugging prompts, asking “Why might this API call fail?” or “What’s causing this error?”.
These helped me not only fix bugs but also understand the underlying issues, which was an essential learning experience.

## What Worked Well?
One thing I’m especially proud of is integrating interactive temperature and precipitation plots using ipywidgets and matplotlib.
Another highlight was the natural language question parser. Instead of requiring users to click dropdowns or type in strict formats,
they could simply ask, “Will it rain in Paris tomorrow?” and receive a smart, human-like response. This felt like a real assistant rather than just a script. 
Implementing this feature made me realize the value of user-centered design and the importance of making tools feel intuitive.

## What Would You Do Differently?
Given more time, I would have explored external NLP libraries like spaCy to enhance the AI’s language understanding. 
My current rule-based parser is functional but limited in nuance. 
A machine learning-based model could handle ambiguous or complex phrasing better.
I also would have spent time writing unit tests to ensure the core functions behave correctly under various inputs,
and added logging and performance metrics for the API interactions.
Lastly, turning this into a web application using Streamlit would be the natural next step—making it more polished and accessible for real users.

## Final Thoughts
This project demonstrated the power of combining AI collaboration with human creativity and problem-solving.
I learned how to work iteratively, break down problems, and use AI as a coding partner.
This project taught me more than just Python or API usage, it also taught me how to collaborate with AI effectively. 
I also discovered how rewarding it is to design for real human interaction.
Making the UI clean, the responses friendly, and the plots interactive made the tool feel alive. 
It reminded me that behind every function is a user who wants answers, not just data.
This mindset — to make tools accessible, not just functional, it 
is something I’ll carry forward in every project I build.
If I had to sum it all up, I’d say this project taught me how to think like a developer and communicate like a designer.

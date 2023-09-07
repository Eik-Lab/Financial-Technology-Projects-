# Simple Sentiment Analysis WebApp using Streamlit

This README provides a guide to create a straightforward sentiment analysis web application using the Streamlit module in Python.

## Prerequisites

To get started, you'll need the following Python libraries:

- Streamlit
- TextBlob
- Streamlit_extras (This is optional. It is used to incorporate extra features to the application but doesn't influence the sentiment analysis results.)

Install these modules using pip:

```bash
pip install streamlit
pip install -U textblob
pip install streamlit_extras
```

## Implementation Steps

1. **Setting up the Basic App**:
   Begin by importing the necessary libraries.

   ```python
   import streamlit as st
   from textblob import TextBlob
   ```

   Then, create a title for your web application and provide an input field for the user to enter their text.

   ```python
   st.title("A Simple Sentiment Analysis WebApp.")
   text = st.text_area("Please Enter your text")
   ```

   Now, generate a button that the user can click to analyze the entered sentiment.

   ```python
   if st.button("Analyze the Sentiment"):
     blob = TextBlob(text)
     result = blob.sentiment
     st.write(result)
   ```

2. **Enhancing the Web App with Additional Features**:
   You can make the app more interactive by using the `Streamlit_extras` module. For this, display raining emojis based on the polarity of the sentiment. If the sentiment is positive (polarity >= 0), a smiley emoji will rain. If it's negative, a crying or tensed emoji will appear. Furthermore, instead of a plain text output, you can use different alerts like warnings and success messages to indicate the sentiment.

   First, import the necessary extras:

   ```python
   from streamlit_extras.let_it_rain import rain
   ```

   Here's how you can add these features:

   ```python
   if st.button("Analyze the Sentiment"):
     blob = TextBlob(text)
     result = blob.sentiment
     polarity = result.polarity
     if polarity < 0:
       st.warning("The entered text has negative sentiments associated with it: " + str(polarity))
       rain(
         emoji="🥺",
         font_size=20,
         falling_speed=3,
         animation_length="infinite",
       )
     else:
       st.success("The entered text has positive sentiments associated with it: " + str(polarity))
       rain(
         emoji="😊",
         font_size=20,
         falling_speed=3,
         animation_length="infinite",
       )
     st.success(result)
   ```

## Final Application

By integrating all the code snippets above, you have a complete Sentiment Analysis WebApp using Streamlit. When you run the app, you can input any text, and it will display the sentiment with engaging visuals.

Feel free to customize and enhance the app as you see fit!

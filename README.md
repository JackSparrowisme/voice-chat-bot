# voice-chat-bot
import pyttsx3
import speech_recognition as sr
import datetime
import webbrowser
import random

# Initialize speech engine
def speak(text):
    print(f"🤖 Bot says: {text}")
    engine = pyttsx3.init('sapi5')
    engine.setProperty('rate', 150)
    engine.setProperty('volume', 1.0)

    voices = engine.getProperty('voices')
    engine.setProperty('voice', voices[0].id)  # Use first available voice
    engine.say(text)
    engine.runAndWait()
    engine.stop()

# Listen to user
def listen():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("🎤 Listening...")
        r.adjust_for_ambient_noise(source)
        audio = r.listen(source)
    try:
        command = r.recognize_google(audio)
        print(f"✅ You said: {command}")
        return command.lower()
    except sr.UnknownValueError:
        speak("Sorry, I didn't catch that.")
        return ""
    except sr.RequestError:
        speak("Google speech service is down.")
        return ""

# Respond to commands
def chatbot():
    speak("Hello! I am your AI assistant. How can I help you today?")

    jokes = [
        "Why don’t scientists trust atoms? Because they make up everything.",
        "I told my computer I needed a break, and it said no problem — it’ll go to sleep.",
        "Why was the math book sad? Because it had too many problems."
    ]

    while True:
        command = listen()

        if command == "":
            continue

        if "hello" in command or "hi" in command:
            speak("Hi there! What can I do for you?")
        elif "your name" in command or "who are you" in command:
            speak("I am Jarvis, your Python AI assistant.")
        elif "time" in command:
            time_now = datetime.datetime.now().strftime("%I:%M %p")
            speak(f"The time is {time_now}")
        elif "joke" in command:
            speak(random.choice(jokes))
        elif "google" in command:
            speak("Opening Google")
            webbrowser.open("https://www.google.com")
        elif "youtube" in command:
            speak("Opening YouTube")
            webbrowser.open("https://www.youtube.com")
        elif "bye" in command or "exit" in command:
            speak("Goodbye! Have a great day.")
            break
        else:
            speak("I'm not sure how to respond to that.")

chatbot()

# AI-JARVIS import speech_recognition as sr
import pyttsx3
import webbrowser
import datetime
import os

# Voice engine
engine = pyttsx3.init()
engine.setProperty("rate", 175)

def speak(text):
    print("JARVIS:", text)
    engine.say(text)
    engine.runAndWait()

def listen():
    recognizer = sr.Recognizer()

    with sr.Microphone() as source:
        print("Listening...")
        recognizer.adjust_for_ambient_noise(source, duration=0.5)

        try:
            audio = recognizer.listen(source, timeout=5, phrase_time_limit=8)
            command = recognizer.recognize_google(audio)
            print("You:", command)
            return command.lower()

        except sr.UnknownValueError:
            return ""

        except sr.RequestError:
            speak("I cannot connect to the speech recognition service.")
            return ""

        except sr.WaitTimeoutError:
            return ""

def jarvis():
    speak("Hello. I am JARVIS. How can I help you?")

    while True:
        command = listen()

        if not command:
            continue

        # Exit
        if "exit" in command or "quit" in command or "goodbye" in command:
            speak("Goodbye. See you later.")
            break

        # Time
        elif "time" in command:
            time = datetime.datetime.now().strftime("%I:%M %p")
            speak("The time is " + time)

        # Open YouTube
        elif "open youtube" in command:
            speak("Opening YouTube.")
            webbrowser.open("https://www.youtube.com")

        # Open Google
        elif "open google" in command:
            speak("Opening Google.")
            webbrowser.open("https://www.google.com")

        # Open GitHub
        elif "open github" in command:
            speak("Opening GitHub.")
            webbrowser.open("https://github.com")

        # Search Google
        elif command.startswith("search"):
            search_text = command.replace("search", "", 1).strip()

            if search_text:
                speak("Searching for " + search_text)
                webbrowser.open(
                    "https://www.google.com/search?q=" +
                    search_text.replace(" ", "+")
                )

        # Open calculator
        elif "open calculator" in command:
            speak("Opening calculator.")
            os.system("start calc")

        # Open Notepad
        elif "open notepad" in command:
            speak("Opening Notepad.")
            os.system("start notepad")

        else:
            speak("I heard you say " + command)
            speak("I don't know how to do that yet.")

if __name__ == "__main__":
    jarvis()

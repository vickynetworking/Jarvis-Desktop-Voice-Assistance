# Jarvis-Desktop-Voice-Assistance
#Jarvis program Desktop assistance
import pyttsx3
import os, sys, subprocess
import speech_recognition as sr
import datetime
import wikipedia
import webbrowser
import smtplib

def speek(audio):
    os.system(f'espeak "{audio}"')

def wishMe():
    hour = int(datetime.datetime.now().hour)
    if hour >= 0 and hour < 12:
        speek("Good Morning!")

    elif hour >= 12 and hour < 18:
        speek("Good Afternoon!")

    else:
        speek("Good Evening!")

    speek("I am Jarvis, Sir How may I help You.")

def takeCommand():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        r.pause_threshold = 1        audio = r.listen(source)
    try:
        print("Recognizing...")
        query = r.recognize_google(audio, language = 'en-in')
        print(f"User said: {query}\n")
    except Exception as e:
        print("Say that again please...")
        return "None"    return query

def sendEmail(to, content):
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.ehlo()
    server.starttls()
    server.login('Your.email@gmail.com', 'youremailpassword')
    server.sendmail('Your.email@gmail.com',to, content)
    server.close()

def openFile(filename):
    if sys.platform == "win32":
        os.startfile(filename)
    else:
        opener ='open' if sys.platform == 'drawin' else 'xdg-open'        subprocess.call([opener, filename])





if __name__ == '__main__':
    wishMe()
    while True:
        query = takeCommand().lower()

        if 'wikipedia' in query:
            speek('Searching Wikipedia...')
            query = query.replace("wikipedia", "")
            result = wikipedia.summary(query, sentences=3)
            speek("According to Wikipedia")
            print(result)
            speek(result)
        elif 'open youtube' in query:
            speek("opening Youtube...")
            webbrowser.open("youtube.com")
        elif 'open google' in query:
            speek("Opening Google")
            webbrowser.open("google.com")
        elif 'open staackoverflow' in query:
            speek("Opening StackOverflow")
            webbrowser.open("stackoverflow.com")
        elif 'open gmail' in query:
            speek("Opening gmail...")
            webbrowser.open("gmail.com")
        elif 'the time' in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")
            speek(f"Sir, the time is {strTime}")
        elif 'exit program' in query:
            speek("Exiting from this program")
            exit()
        elif 'play music' in query:
            filename = "/home/vicky/Downloads/Haan Main Galat x If I Can't Have You Mashup(PagalWorld).mp3"            speek("Playing Music!")
            openFile(filename)
        elif 'pycharm' in query:
            filename = '/snap/pycharm-community/current/bin/pycharm.sh'            openFile(filename)
        elif 'email to vivek' in query:
            try:
                speek("What should I say?")
                content = takeCommand()
                to = "Your.email@gmail.com"                sendEmail(to, content)
                speek("Email has been sent!")
            except Exception as e:
                print(e)
                speek("Sorry My friend Vicky Bhai, I am not able to send this email.")


import pyttsx3
import datetime
import speech_recognition as sr
import wikipedia
import webbrowser
import os
import smtplib
from requests import get
import sys
import PyPDF2


engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[0].id)


def speak(audio):
    engine.say(audio)
    engine.runAndWait()


def whishme():
    hour = int(datetime.datetime.now().hour)
    if hour >= 0 and hour < 12:
        speak("good morning  sir")

    elif hour >= 12 and hour < 18:
        speak("good afternoon  sir")

    else:
        speak("good evening  sir")

    speak(" sam at your service. how can i help you ")


def takecommand():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("listening...")
        r.pause_threshold = 1
        audio = r.listen(source, timeout=5)

    try:
        print("recognizing...")
        query = r.recognize_google(audio, language='en-in')
        print(f"user said: {query}\n") 

    except Exception as e:
        print("say that again please")
        speak("say that again please")
        return "none"
    return query

def pdf_reader():
    book = open('E:\Basic Linux Commands.pdf','rb')
    pdfreader =PyPDF2.PdfFileReader(book)
    pages = pdfreader.numPages
    speak(f"total no of pages in this book{pages} ")
    speak("sir please tell the page number i have to read")
    pg = int(input("please tell the page number: "))
    page = pdfreader.getPage(pg)
    text = page.extractText()
    speak(text)





if __name__ == '__main__':
    whishme()
    
    while True:

         query=takecommand().lower()
         if 'search' in query:
             speak('searching wikipedia...')
             query =query.replace("wikipedia", "")
             results = wikipedia.summary(query, sentences=2)
             print(results)
             speak(results)

         elif'open youtube' in query:
             speak("yes let watch some videos     \n     oh sorry sir opening")
             webbrowser.open("youtube.com")

         elif'open google' in query:
             speak("oh you want to search something")
             webbrowser.open("google.com")

         elif 'open stackoverflow' in query:
             speak("opening stackoverflow")
             webbrowser.open("stackoverflow.com")
        
         elif'play music' in query:
                 music_dir = 'H:\\my music\\fav'
                 songs = os.listdir(music_dir)
                 print(songs)
                 os.startfile(os.path.join(music_dir, songs[7]))

         elif'time' in query:
             strtime = datetime.datetime.now().strftime("%H:%M")
             speak(f"sir, the time is {strtime}")

         elif 'open code' in query:
             speak("openig your code sir")
             codePath = "F:\\Python Course with Notes\\karan.py"
             os.startfile(codePath)

         elif 'i want to send mail' in query:
             speak("sure sir opening gmail.com")
             webbrowser.open("https://mail.google.com/mail/u/1/#inbox")

         elif 'show my channels growth' in query:
             speak("sir your channel is doing really great see this")
             webbrowser.open('https://studio.youtube.com/channel/UCQ3YeAWnWNwvPgykWIYg0VA')
        
         elif 'upgrade you' in query:
             speak("you can upgrde me by watching this video")
             webbrowser.open('https://www.youtube.com/watch?v=EztlAyuOg6o')

         elif 'play GTA V' in query:
             speak("opening gta v")
             game = "E:\\Grand Theft Auto V\\gtavlauncher.exe"
             os.startfile(game)

         elif 'play minecraft' in query:
             speak("opening minecraft")
             game2 = "C:\\Users\\ATC\\AppData\Roaming\\.minecraft\\tlauncher.exe"
             os.startfile(game2)

         elif 'edit video' in query:
             speak("opening premier pro sir")
             videoedit = "H:\\New folder (2)\\Adobe Premiere Pro CC 2018\\Adobe Premiere Pro CC 2018\\Adobe Premiere Pro.exe"
             os.startfile(videoedit)
        
         elif 'ip address' in query:
             ip = get('https://api.ipify.org').text
             speak(f"sir you ip address is {ip}")

         elif 'open notepad' in query:
             speak("opening notepad")
             notepad = "G:\\Program Files\\Notepad++\\notepad++.exe"
             os.startfile(notepad)
         
         elif 'open instagram' in query:
             speak("opening instagram sir")
             webbrowser.open('https://www.instagram.com/')

         elif 'arpit on instagram' in query:
             speak("sure sir")
             webbrowser.open('https://www.instagram.com/mahal_arpit/')

         elif 'open facebook' in query:
             speak("opening facebook sir")
             webbrowser.open('https://www.facebook.com/')

         elif 'sleep now' in query:
             speak("sure sir signing off for now ")
             sys.exit() 

         elif 'close notepad' in query:
             speak("ok sir closing notepad")
             os.system("TASKKILL /? /im notepad++.exe")

         elif 'open pycharm' in query:
             speak("opening pycharm sir")
             charm = "C:\\Python 39\\PyCharm Community Edition 2020.3\\bin\\pycharm64.exe"
             os.startfile(charm)

         elif 'close pycharm' in query:
             speak("closing pycharm")
             os.system("TASKKILL /? /im PyCharm Community Edition 2020.3 x64.exe")


         elif "where i am" in query or "where we are" in query:
             speak("wait sir let me check")
             try:
                 ipadd = requests.get('https://api.ipfy.org').text
                 print(ipadd)
                 url = 'https://grt.geojs.io/vl/ip/geo/'+ipadd+'.json'
                 geo_requests =requests.get(url)
                 geo_data = geo_rquests.json()
                 city = geo_data['city']
                 state = geo_data['state']
                 country = geo_data['country']
                 speak(f"sir i am not sure but , i think we are in{city} city state{state} of country{country} ")
             except Exception as e:
                 speak("sorry sir , i am not able to find where we are because of network here. ")
                 pass      
             speak("anything else sir")               
             
         elif "read pdf" in query:
             pdf_reader()


if __name__ == "main":
    start()
                     
    

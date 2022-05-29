
# importujem sve potrebne module
import pyttsx3
import imdb
import speech_recognition as sr
import datetime


# Kreiram "speak" funkciju za govor
def speak(text):
	engine = pyttsx3.init()
	voices = engine.getProperty('voices')
	engine.setProperty('voice', voices[1].id)
	rate = engine.getProperty('rate')

	engine.setProperty('rate', rate-20)

	engine.say(text)
	engine.runAndWait()


# pozivanje speak() funkcije za govor
speak("Say the movie name")


# funkcija za ulaz u audio formatu
def get_audio():
	r = sr.Recognizer()
	with sr.Microphone() as source:
		r.energy_threshold = 50
		r.dynamic_energy_threshold = False
		r.pause_threshold = 1
		r.adjust_for_ambient_noise(source, duration=1)
		audio = r.listen(source)
		said = ""

	try:
		# prepoznaje unos i ispisuje ga
		said = r.recognize_google(audio)
		print(said)

	except:
		# ako ne prepozna audio unos
		speak("Didn't get that")
	# vraca unos malim slovima
	return said.lower()


# funkcija za pretragu filma
def search_movie():

	# sakuplja informacije sa IMDb-ja
	moviesdb = imdb.IMDb()

	# prima ime filma - povratna vrijednost iz get_audio funkcije
	text = get_audio()

	# pretrazuje primljeno ime filma
	movies = moviesdb.search_movie(text)

	speak("Searching for " + text)
	# ako ne nadje ni jedan film 
	if len(movies) == 0:
		speak("No result found")
	else:
		# ako nadje film
		speak("I found these:")

		for movie in movies:

			title = movie['title']
			year = movie['year']
			# izgovara ime filma i godinu izlaska
			speak(f'{title}-{year}')

			info = movie.getID()
			movie = moviesdb.get_movie(info)
			
			# pravimo promjenjive za informacije o filmu
			title = movie['title']
			year = movie['year']
			rating = movie['rating']
			plot = movie['plot outline']

			# ovaj if-else odredjuje da li je film vec izasao ili tek izlazi
			if year < int(datetime.datetime.now().strftime("%Y")):
				speak(
					f'{title} was released in {year} has IMDB rating of {rating}.\
					The plot summary of movie is {plot}')
				print(
					f'{title} was released in {year} has IMDB rating of {rating}.\
					The plot summary of movie is {plot}')
				break
			# filmovi koji ce tek izaci
			else:
				speak(
					f'{title} will release in {year} has IMDB rating of {rating}.\
					The plot summary of movie is {plot}')
				print(
					f'{title} will release in {year} has IMDB rating of {rating}.\
					The plot summary of movie is {plot}')
				break

# pozivanje funkcije
search_movie()

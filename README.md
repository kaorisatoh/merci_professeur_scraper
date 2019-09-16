# "Merci professeur !" Episode Video Scraper

![merci_professeur_program](/merci_professeur_program.jpg)

["Merci professeur !"](http://www.tv5monde.com/emissions/episodes/merci-professeur) is a short linguistic program presented by Bernard CERQUIGLINI on TV5MONDE, the world's leading French-language cultural broadcaster that reaches more than 318 million households and 32 million viewers every week in 200 countries and territories:

![tv5monde](/tv5monde.png)

Each episode of this program presents, with humor and simplicity, and in less than 2 minutes, linguistic, etymological, orthographic, and grammatical difficulties of the French language. Viewers can ask Bernard CERQUIGLINI questions about the French language's subtleties which answer will be directly broadcast.

"Merci professeur !" is probably the most accessible and interesting program about the French language. There are hundreds of episodes available on the Internet. However, these episodes are not greatly highlighted on TV5MONDE web site, while they could be integrated in a free mobile application that would push a new episode to the subscribers, for instance, every morning, to enjoy with a coffee and croissants. :)

Your mission, should you choose to accept it, is to find a way to hack TV5MONDE Web site's data, to download and rebuild the episode videos. As always, should you be caught, the Secretary will disavow any knowledge of your actions. Good luck.

![impossible_mission_wallpaper](/impossible_mission_wallpaper.jpg)

# Waypoint 1: Write a Python Class Episode

We have started to hack TV5MONDE Web site's data and we have discovered that it is using a private API to fetch the list of the episodes that are displayed.

The URL of this endpoint is: http://www.tv5monde.com/emissions/episodes/merci-professeur.json.

This endpoint returns a JSON expression that contains an array of dictionaries, each dictionary corresponds to the information of an episode. We can discover the structure of the response returned by this API's endpoint with the following Shell command:

$ curl --silent http://www.tv5monde.com/emissions/episodes/merci-professeur.json | json_pp

For example, this command returns:

{
  "episodes":[
    {
      "title":"Permaculture",
      "url":"\/emissions\/episode\/merci-professeur-permaculture",
      "image":"https:\/\/vodhdimg.tv5monde.com\/tv5mondeplus\/images\/4927553.jpg",
      "date":"Vendredi 2 ao\u00fbt 2019 (redif. du Mercredi 28 f\u00e9vrier 2018)",
      "duration":"02:00"
    },
    {
      "title":"On est sur...",
      "url":"\/emissions\/episode\/merci-professeur-on-est-sur",
      "image":"https:\/\/vodhdimg.tv5monde.com\/tv5mondeplus\/images\/4832469.jpg",
      "date":"Vendredi 2 ao\u00fbt 2019",
      "duration":"02:32"
    },
    ...
  ],
  "numPages":26
}
We will store the information of an episode in an object.

Write a Python class Episode which constructor takes the following parameters in that particular order:
*title: The title of the episode

*page_url: The Uniform Resource Locator (URL) of the Web page dedicated to this episode

*image_url: The Uniform Resource Locator (URL) of the image (poster) that is shown while the video of the episode is downloading or until the user hits the play button; this is the representative of the episode's video

*broadcasting_date: The date when this episode has been broadcast

Write a static method from_json of this class that takes an argument payload (a JSON expression) and that returns an object Episode.

We provide hereafter an example of the JSON expression that is passed to this static method:

{
  "title": "Kilom\u00e8tre par heure",
  "url": "/emissions/episode/merci-professeur-kilometre-par-heure",
  "image": "https://vodhdimg.tv5monde.com/tv5mondeplus/images/5022428.jpg",
  "date": "Vendredi 21 juin 2019 (redif. du Samedi 28 avril 2018)",
  "duration": "02:08"
}
The class Episode's attributes **MUST** be private. They **MUST** be accessible through the read-only properties title, page_url, image_url, and broadcasting_date.

Also, the private attribute page_url, corresponding to the URL of the episode's Web page, **MUST** start with the string "http://www.tv5monde.com".

For example:

# Build the JSON expression of the information of an episode.
>>> payload = json.loads(
...     """
...     {
...       "title": "Kilom\u00e8tre par heure",
...       "url": "/emissions/episode/merci-professeur-kilometre-par-heure",
...       "image": "https://vodhdimg.tv5monde.com/tv5mondeplus/images/5022428.jpg",
...       "date": "Vendredi 21 juin 2019 (redif. du Samedi 28 avril 2018)",
...       "duration": "02:08"
...     }""")
# Build an object Episode with this JSON expression.
>>> episode = Episode.from_json(payload)
# Read the episode's information using the properties of this object.
>>> episode.title
'Kilomètre par heure'
>>> episode.page_url  # This URL has been prefixed with "http://www.tv5monde.com".
'http://www.tv5monde.com/emissions/episode/merci-professeur-kilometre-par-heure'
>>> episode.image_url
'https://vodhdimg.tv5monde.com/tv5mondeplus/images/5022428.jpg'
>>> episode.broadcasting_date
'Vendredi 21 juin 2019 (redif. du Samedi 28 avril 2018)'
>>> episode.duration
'02:08'
# These properties are read-only; they CANNOT be set.
>>> episode.title = 'Something else'
Traceback (most recent call last):
  File "<input>", line 1, in <module>
AttributeError: can't set attribute

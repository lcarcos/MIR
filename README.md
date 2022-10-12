# MIR: An introduction to music information retrieval using Python and Spotify Web API

### Abstract 

Music Information Retrieval, or MIR, is the science of analyzing and categorizing musical data. MIR is a growing field of research with many real-world applications. Music recommendation systems are examples of application. The music streaming platform Spotify applies artificial intelligence techniques to audio analysis and provides access to this data through its web API. This work presents the development of an application for analyzing and categorizing musical data using Python and Spotify web API.

### 1. Introduction 

Music information retrieval is the science of analyzing and categorizing musical data. Some applications of MIR are closely related to the field of data science, while combining various fields such as psychoacoustics, signal processing, machine learning and computational intelligence. MIR is a growing field of research with many real-world applications. Music recommendation systems are examples of application.

Spotify is a very popular music service that applies machine learning techniques for audio analysis and music information retrieval. Fortunately, Spotify makes this data available through its web API. It is possible to explore the characteristics of the audio and analyze the tracks in detail.

The objective of this work is to create a music analysis and categorization application based on Spotify's recommendation system. We will use Python and the Spotify API in our development.

In this work, we will use the artist Anitta as an example. We want to search for your most popular songs, offering a visual analysis of the audio characteristics.

### 2. Modeling

#### Spotify API Access

To create our application using Spotify's music service, we initially need an account at [spotify.com](www.spotify.com) . The same credentials allow us to set up the developer account on the service [Spotify for Developers](https://developer.spotify.com/). From there, it is possible to create an application using the Spotify API, accessing the server and manipulating data as we wish.

We create the “Music is Data” application and obtain our *Client ID* and *Client Secret*, essential information to access the API.

![spotify_devloper](https://user-images.githubusercontent.com/29662327/194144682-a423d586-633b-4033-ab3b-4e65cb95c9cd.PNG)

#### Python Spotipy Library

We work with Python language and access Spotify music data using Spotipy library. This lightweight library supports all Spotify web API features.

https://spotipy.readthedocs.io

![spotipy](https://user-images.githubusercontent.com/29662327/194144786-8ee7f854-6b36-4c8a-987a-098d8673b5a4.PNG)

```

!pip install spotipy

```

We import the Spotipy library and import SpotifyClientCrendials. Using our Client ID and Client Secret we establish the connection.

```
import spotipy
from spotipy.oauth2 import SpotifyClientCredentials

```
```
SPOTIPY_CLIENT_ID='xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
SPOTIPY_CLIENT_SECRET='xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'

```
```
auth_manager = SpotifyClientCredentials(client_id=SPOTIPY_CLIENT_ID, client_secret=SPOTIPY_CLIENT_SECRET)
sp = spotipy.Spotify(auth_manager=auth_manager)

```
#### Collecting Data

Our initial objective is to search for the most popular songs by singer Anitta in the Spotify system. For this we need to identify your URI, a parameter used by Spotify to identify artists, albums and tracks.

```
artist_name = 'anitta'

```
```
artists = sp.search(q='artist:'+artist_name,type='artist', limit=1)

```
The code above returns a JSON with various information about the artist, including the URI.
```
artist_uri = artists['artists']['items'][0]['uri']
print(artist_uri)
```
With your URI we request your most popular songs. The system also offers us the possibility to access a 30-second preview of the song and a link to the album cover.
```
results = sp.artist_top_tracks(artist_uri)

for track in results['tracks'][:10]:
    print('track : ' + track['name'])
    if track['preview_url'] is not None:
        print('audio : ' + track['preview_url'])
    print('cover art: ' + track['album']['images'][0]['url'])
    print()
      
```
Finally, we create a DataFrame with the most popular songs by importing the respective audio characteristics.

[View code](https://github.com/lcarcos/MIR/blob/main/Intro%20to%20MIR_01.ipynb)


<div style="page-break-after: always;"></div>

### 3. Results

#### Audio Features

Spotify API provides important information about songs. By analyzing the music's key characteristics, Spotify is able to determine its similarity to other music and make recommendations based on how the music sounds compared to others. This is how Spotify recommends smaller artists and newer music so users don't just listen to popular music from bigger artists.
For each track on its platform, Spotify provides data for thirteen audio resources. The [Spotify Web API developer guide](https://developer.spotify.com/documentation/web-api/reference/#/operations/get-audio-features) defines them as follows:

>*Danceability*: Describes how suitable a track is for dancing based on a combination of musical elements, including tempo, rhythm stability, beat strength, and overall regularity.
>
>*Valence*: Describes the musical positivity conveyed by a track. Tracks with high valence sound more positive (eg happy, joyful, euphoric), while tracks with low valence sound more negative (eg sad, depressed, angry).
>
>*Energy*: Represents a perceptual measure of intensity and activity. Typically, energetic tracks feel fast, loud, and noisy. For example, death metal has high energy, while a Bach prelude scores low on the scale.
>
>*Time*: The overall estimated time of a track in beats per minute (BPM). In musical terminology, tempo is the speed or rhythm of a given piece and is directly derived from the average beat duration.
>
>*Loudness*: The overall volume of a track in decibels (dB). Volume values ​​are averaged over the entire track and are useful for comparing the relative volume of tracks.
>
>*Speechiness*: Detects the presence of spoken words in a track. The more uniquely spoken the recording (eg, talk show, audiobook, poetry), the closer the attribute value is to 1.0.
>
>*Instrumentalness*: Predicts whether a track does not contain vocals. The sounds “Ooh” and “aah” are treated as instrumental in this context. Rap or spoken word tracks are clearly “vocals”.
>
>*Liveness*: Detects the presence of an audience in the recording. Higher liveliness values ​​represent a greater probability that the track was performed live.
>
>*Acousticness*: A measure of confidence from 0.0 to 1.0 if the track is acoustic.
>
>*Key*: The estimated overall key of the track. Integers are mapped to pitches using standard Pitch Class notation. For example. 0 = C, 1 = C♯/D♭, 2 = D, and so on.
>
>*Mode*: Indicates the modality (major or minor) of a track, the type of scale from which its melodic content is derived. Major is represented by 1 and minor is 0.
>
>*Duration*: The track's duration in milliseconds.
>
>*Time Signature*: An estimated overall time signature of a track. The time signature (meter) is a notation convention for specifying how many beats there are in each measure (or measure).


#### Data analysis

In our analysis we will work with 7 main characteristics related to popularity: *'acousticness', 'danceability', 'energy', 'instrumentalness', 'liveness', 'speechiness'* and *'valence'* .

We initially focused on the 3 most popular songs by the artist Anitta, “La Loto”, “Dançarina” and “Envolver”. We created a radar chart, which allows us to visualize the characteristics of the audio. From the API we also filter the album cover/*single* and a 30 second audio preview of the tracks.

#### **La Loto**

cover art: ![La Loto](https://i.scdn.co/image/ab67616d0000b2732c4c95e7117b617b3c3c7de1)

audio : [30 second preview](https://p.scdn.co/mp3-preview/f68791908c9ebdcd4feec24f1824297b5d7070cb?cid=16960f7026d64979bf989042a150a0ff)

The song “La Loto” features the following chart:

![la_loto_anitta](https://user-images.githubusercontent.com/29662327/194650318-218c6068-31a1-4a19-a938-6e500576714d.png)

The graphic shows us a song that stands out in some characteristics like **energy, danceability** and **valence**. According to Spotify, "Usually, energetic tracks sound fast, loud and loud." A high score of **danceability**, “describes how suitable a track is for dancing.” **Valence**” describes the musical positivity conveyed by a track. Tracks with high valence sound more positive.


<div style="page-break-after: always;"></div>

#### **DANÇARINA (feat. Nicky Jam, MC Pedrinho) - Remix**

cover art: ![Dancer](https://i.scdn.co/image/ab67616d0000b27386ec217718905b9eeb9f712f)

audio : [30 second preview](https://p.scdn.co/mp3-preview/9e573962771c2d3d07ad3c788fd8dc37d6f5e1a7?cid=16960f7026d64979bf989042a150a0ff)

The song “DANÇARINA” features the following chart:

![dancarina_anitta](https://user-images.githubusercontent.com/29662327/194653141-4b4929be-2ca0-4d5a-ab36-19937f15e192.png)

The graphic for the song “Dançarina” features a similar “drawing” highlighting the same characteristics as the previous song. The chart below presents a comparison between the two songs:

![la_loto_dancarina_comapracao](https://user-images.githubusercontent.com/29662327/194665857-ec7e4f07-f404-4888-b469-6a568f092e40.PNG)

The comparison shows that the song “La Loto” (characteristics 1) has greater valence, that is, it sounds more positive, is more danceable, and more “energetic”,

<div style="page-break-after: always;"></div>

#### **To involve**

cover art: ![Envolver](https://i.scdn.co/image/ab67616d0000b27396e2642422ad16661e673fa2)

audio : [30 second preview](https://p.scdn.co/mp3-preview/784196314fb6965fbe3a326ff543c8dd6e944ab4?cid=16960f7026d64979bf989042a150a0ff)

The song “Envolver” features the following graphic:

![envolver_anitta](https://user-images.githubusercontent.com/29662327/194654328-7d33a44b-e029-4425-a22b-5c84b7fdcc77.png)

Comparison between the song "La Loto" and "Envolver":

![la_loto_envolver_comapracao](https://user-images.githubusercontent.com/29662327/194661493-200e107f-a6ec-46e6-831b-380aed0f53d7.PNG)

The comparison, as in the previous case, shows that the song “La Loto” (characteristics 1) has greater valence, sounds more positive, is more danceable, and more “energetic” than the song “Envolver”.

Our application makes it possible to create graphics relating certain audio characteristics and popularity. The following chart presents the 10 most popular songs by Anitta on the Spotify platform, relating to the *"danceability"* feature.

![picture1](https://user-images.githubusercontent.com/29662327/194157483-b6a8d83b-5d2a-4d71-9334-a5b58907c26e.png)

The graphic above shows highly danceable songs as a characteristic of the artist Anitta.

The following chart presents the 10 most popular songs by Anitta on the Spotify platform, relating to the *"energy"* feature.

![energy](https://user-images.githubusercontent.com/29662327/194668352-c625ba42-b6dc-4292-8dd6-38f15d3ec0f8.png)

The chart below shows the 10 most popular songs by Anitta on the Spotify platform, relating to the feature *"valence"*.

![valence](https://user-images.githubusercontent.com/29662327/194668414-183466ea-2ff0-4d21-8645-c8b3019d02c5.png)

As we can see from the analysis of the graphs above, some characteristics are constant in the songs of the artist Anitta and contribute to consolidate her musical identity. Highly danceable songs, with a high energy index, indicating "agitated" songs. The parameter *valence* with high rates is also one of the outstanding features, indicating happy, happy songs that sound positive.

<div style="page-break-after: always;"></div>

### 4. Conclusions

In the digital age, music is data. According presented, *music information retrieval* is a growing and essential field of research for *streaming* services such as Spotify.

More studies are needed to directly relate the characteristics of the audio and the popularity of the songs. But looking at the release date of the songs, it is clear that the latest releases are also the most popular songs. Given the way Spotify's algorithm works, and seeing volatility as a characteristic of the music industry, we understand the current market preference for *singles* release.

From our analysis it was possible to prove through data some intuitive perceptions about the artist Anitta. Her repertoire is marked by dancing songs, with few acoustic characteristics, more electronic elements, “energetic” or “agitated” songs, which sound positively.

With the development of the application using Spotify's web API, it was possible to understand a little more about music metadata and how data science, psychoacoustics, signal processing, machine learning and computational intelligence combine for the analysis and categorization of music. musical data.

The project continues in development exploring the numerous possibilities offered by the Spotify API. The objective is to expand the possibilities of analysis and create a web app with an interactive interface using Streamlit.

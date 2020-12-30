# K-pop on the Billboard Charts Data Biography


# K-pop on the Billboard Hot 100


## Data sources



*   The list of K-pop songs and chart positions was [retrieved from Wikipedia](https://en.wikipedia.org/wiki/List_of_K-pop_songs_on_the_Billboard_charts#Billboard_Hot_100_(Complete)). I’d like to thank Wikipedia editor Bonnielou2013 for maintaining this list. The complete Billboard charts are behind a paywall, so I did not independently verify the positions Bonnielou2013 reported. 
    *   Note: The Wikipedia list currently includes the children’s song “Baby Shark” because it was created by a South Korean entertainment company. I originally decided to remove it from the list because, while it is indeed a popular song created by a South Korean group, it isn’t perceived as a Korean song by most listeners and certainly doesn’t fit the mold for idol music. [The discussion section of the Wikipedia page](https://en.wikipedia.org/wiki/Talk:List_of_K-pop_songs_on_the_Billboard_charts) includes notes on whether “Baby Shark” is K-pop or not, as well as the editor’s rationale for not including “Seoul Town Road”, a remix of “Old Town Road” which featured RM of BTS. I later decided to add "Baby Shark" back to the dataset (it is included now) to draw attention to the blurry and contested boundaries of the "K-pop" category.
*   Original and translated lyrics were mostly retrieved from Genius. I manually collected Genius URLs for each song and then web scraped the lyrics. If lyrics were not on Genius, I manually searched for and copied the relevant information from other lyrics websites; the URL of the lyric source for each song is included in the resulting dataset. The code used to collect the lyrics can be found in “‘filled lyrics scrape.ipynb”. 
*   Audio feature data for each song was retrieved from Spotify using the website’s API and the spotiPy Python interface library. The API search feature was used to find the Spotify IDs for each track, which was then used to look up the estimated valence and danceability for each song. If a song was not successfully located via search, I manually located the song’s ID. The code used to collect Spotify data can be found in “spotify data.ipynb”. 


## Data processing



*   All 34 songs included in the Wikipedia list, minus “Baby Shark”, are included in the final dataset. 
*   Textual data was processed to standardize spacing and remove extra elements, like parentheticals in song names and bracketed indications of verse changes in lyrics. Peak chart positions were processed so that songs that entered the chart multiple times only had their highest position stored. 
*   English lyrics were extracted from original lyrics by writing a regular expression rule to remove all characters that were not punctuation or roman characters. 
*   The sentiment of translated lyrics and lyrics originally in English was calculated using TextBlob’s sentiment method. The result is a sentiment polarity score that ranges from -1 to 1. TextBlob’s sentiment methodology unfortunately does not seem to be very well documented, so I cannot go into further detail on how it works. I experimented with using [VADER](https://github.com/cjhutto/vaderSentiment) after seeing that it was better documented and apparently better at handling nuance, but it did not seem to work as well on song lyrics given that they are not sentences, the expected input for VADER. Given that TextBlob’s model, like most sentiment analysis models, is likely trained on product reviews, the results should be treated skeptically. I instead tried to focus on them as a method for comparing different groups of lyrics to each other using the same arbitrary results, rather than focusing  on the specific polarity results returned by the algorithm.  
*   Full code used for data processing is in “lyrics processing bbh100.ipynb”.


# K-pop on the World Digital Song Sales Chart:


## Data sources



*   The list of K-pop songs and chart positions was also [retrieved from Wikipedia](https://en.wikipedia.org/wiki/List_of_K-pop_songs_on_the_Billboard_charts#Billboard_Hot_100_(Complete)) (same as above). 
*   Original and translated lyrics were mostly retrieved from Genius. I used the Genius API and the Lyrics Genius Python interface to search for each artist’s Genius page and then locate the relevant original lyrics for each song. If the search API failed to find an artist with multiple songs on the chart, I attempted to manually modify the search query. Translated lyrics were retrieved from Genius’ “English Translations” artist page. The Genius English Translations page was searched for each song, and lyrics were stored if the result met a fuzzy matching cutoff (to avoid collecting lyrics from false matches). Full code for retrieving lyrics is in “world song sales scrape.ipynb”. 
*   Audio feature data for each song was retrieved from Spotify using the website’s API and the spotiPy Python interface library (same as above). If the Spotify API search did not locate a song, I dropped it from the dataset.


## Data processing



*   113 of the songs included in the Wikipedia list are included in the final dataset. The songs were initially narrowed down to only those that reached the top 3 positions on the World Digital Song Sales chart. Of those, only songs that were successfully matched to original lyrics, translated lyrics, and Spotify data were kept in the final dataset.
*   Other processing details are the same as above.
*   Full code used for data processing is in “wdss lyrics processing.ipynb”.

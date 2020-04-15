-------------------------------------------------------
Music recommendation from key words extracted from text
-------------------------------------------------------
INPUT/OUTPUT:
Input files for music KB are located at: s3://ag-aws-ml-sagemaker-challenge/AWS-ML-Challenge/data/music/Adele/*.json. In the folder accompanying this notebook, they are located at: .\data\Adele\*.json
Input files for curated model is located at: s3://ag-aws-ml-sagemaker-challenge/AWS-ML-Challenge/data/music/Adele/AboutAdele/*.txt. In the folder accompanying this notebook, they are located at: .\data\Adele\AboutAdele\*.txt

Output is currently being generated in the Jupyter notebook.

================================================================================================================================

## Inspiration
Music recommendation is hard and this solution attempts to make it better. There are various aspects to a person's musical tastes and preferences. Some are very adventurous while others prefer to stick to what they like and few others are influenced by what their friends and family are listening to. In order to cater to such a wide variety of tastes, it is important to consider many aspects before a ML system can be considered successful in recommending music. Some of the aspects that can be considered are: 
A. Collaborative recommendation:
    1. social network (friends and family)

B. Content-based recommendation:
   1. genre of music 
   2. characteristics of the music (beats, tempo, liveness, instruments)
   3. lyrics (positive lyrics or those that evoke a certain emotion)
   4. online habits (musical sites, blogs that users read)

The solution submitted considers the last aspect listed above (B4). 

## What it does
A music knowledge base (KB) is used (based on https://www.upf.edu/web/mtg/elmd) from which dictionaries are built for various artists. Some of the values in this dictionary are: the artists who have collaborated with said artist, songs and albums sung or performed and recording labels. The solution then passes text collected from sites and blogs that users have visited, to the curated model "Mphasis DeepInsights Keyphrase Extractor". This model outputs key words which are ranked based on their count of occurrences and is a fair indicator of a user's tastes. The key words are passed into AWS Comprehend to generate entities. Some of the entities are: TITLE, PERSON, EVENT and PLACE. The key words generated from Comprehend are compared against the music KB dictionary and recommendations are made to the user. The recommendations can be other artists who have worked with the artist the user has listened to as well as other songs/albums from same artist.

## How I built it
We initially collected sample text for various artists and then passed them through the curated model "Mphasis DeepInsights Keyphrase Extractor". The key words that were generated were passed through a word cloud to visually verify if it is represented the text. The key words were then sorted based on their count of occurrence. 

Since we did not have the expertise to create a KB exclusively for music, we found ELMD. ([link](https://repositori.upf.edu/bitstream/handle/10230/27835/oramas_lrec16_elmd.pdf?sequence=1&isAllowed=y); [link](https://www.upf.edu/web/mtg/elmd)). We used ELMD to create dictionaries for artists. 

We then find if there is an intersection between the artist dictionary and the key words. If a match is found, singers and songs from the dictionary are recommended to the users. The recommendation is currently straight-forward and picks the top-n recommendations. Future versions can be made more intelligent by enhancing the dictionary with metadata on artists (such as count of times artists have collaborated, their success rates, artists that people in user's social network have listened to).

## Challenges I ran into
The main challenge was creating a music KB and an efficient way to search through them. 

## Accomplishments that I'm proud of
Creating a dictionary for artists for efficient search and retrieval. Creating a process to recommend music based on text read by users.

## What I learned
While exploring how keyword extraction works, I understood how LDA can be used for this purpose. I also learnt the importance of creating appropriate knowledge bases with the right tags.

## What's next for Music recommendation from key words extracted from text
1. To create a more efficient search and retrieval process. Some of the ways can be creating a dictionary for each and every artist or by making use of a graphDB such as Neo4j to create and search for connections between singers and their songs.
2. We already have a working solution to recommend music based on a song's musical parameters such as liveness, tempo, beats, instrumentalness. Next step will be to combine submitted approach with existing approach and arrive at a holistic solution.
3. Existing ELMD annotates only artists, songs, albums and labels but does not tag music genres. Next step will be to enhance ELMD to annotate genres such as rock, jazz and pop-music.
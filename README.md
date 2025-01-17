

# Portfolio by Mariëlle Baelemans
## Week 6: Spotify API 



For my portfolio project I want to use the findings out of an earlier Musicology project. In my research on what makes music 'sad' I found that the minor aspect is the thing what makes music sad. Besides that aspect, also slowness and low pitch are important for giving a listener a sad feeling. My big goal for this portfolio project is to make a widget that measures how sad someone's playlist is by comparing these factors in a 3:2:1 ratio. With these outcomes I want to make a day by day scheme for how 'sad' your day was according to the songs you listened that day. A calendar that shows your mood based on the music you listen ;). 
For now this is a too big goal, so my first step is to compare the major/minor- ratio and the mean tempo of my own playlist 'Most listened 2018' to 'Top 50 Nederland' playlist.  I have not found a way yet to measure the pitch of the songs in the playlists, so for now I use the 'loudness' as extra indicator. 

So far it seems that my own playlist is less sad than the Top 50 Nederland. 
Minor is 31% against 40% in Top 50 Nederland.
My mean tempo is 120 BPM (sd= 31.7) against 116 BPM (sd= 24,0) 
For loudness the outcome for my most listened 2018 has a mean of -8,06 ; sd= 3,55. For Top50NL it has a mean of -6,64  with sd= 2,84.

I can make a formule to calculate the sadness (because of the different in numbers this formulate is not really accurate, but it is a sketch for futher steps).

Sadness= 3mode -2(tempo/1000) -loudness/10. 

Sadness(Top50NL)= 3x0,40 - 2x0,116 + 0,664 = 1.632

Sadness(My2018)= 3x0,31 - 2x0,120 + 0,806 = 1.976

My goal is to create a formule that which takes these aspects in a 3:2:1 ratio and is based on a 1 to 100 scale. 

Imported to keep in mind for the next weeks is that aspects as tempo and loudness are not well measured for songs with long silent intros, like You- The 1975. Those kind of songs are better left behind. While googling for these statistics I found out that Spotify has a correctness chance, this is something to use in the next weeks. 

I would also prefer to use Last.fm for my statistics, because I spend a big amount of my music listening on YouTube. 

## Week 7 Data Visualisation

First of all, I decided to change the playlist I'm comparing. I have a playlist where I put in all the music I listen to. This playlist consist of almost 6,000 songs. I compare this playlist with the 'Top2000' of the Netherlands (of 2018, I made my own playlist also in 2018). Good to notice is that my own playlist (called 'mijn') is 3x as big as the Top2000. It is hard to decide what kind of playlist is best fitting for my project. Because I'm comparing my own music to 'normal' music, I should have a playlist that consist of songs that are most listened. So far I can't find a playlist like that. The Top2000 of the Netherlands is best fitting in a way that it is first of all relative big. Secondly it shows songs that are most known by the Dutch population, which I'm a part of.  
Dr. Burgoyne told us in the last lecture that Energy and Valence are mostly used in music cognition for measuring emotion is music. This made me change my way of doing it in the research, so for now on I'll use energy, valence, mode and tempo as variables.The example visualisation dr. Burgoyne made for our lecture was luckily for me fitting for my portfolio!
 


 ### Here are the visualisations.
 
 
I started off with making a easy bargraph to compare the major/minor difference per playlist: 
![BarGraphColor](BarGraphColor.png)

Because the playlists differ in amount of songs, it is way better to visualise this with percentages. 


As dr. Burgoyne told in the lecture, for this research I will use: mode, tempo, energy and valence. Luckily for me, dr. Burgoyne already made a really good visualisation using these factors, so the only thing I had to do was chancing the visualisation to my own playlists.
![PlaylistVxE](PlaylistVxE.png) 
 
 Because I was not completely happy with the way the major/minor differences was visualised, I changed the pallette to 'Set1', which gave me a red/blue distinction. It kinda looks like an abstract painting now! 
  ![PlaylistVxE](PlaylistVxER.png)
  
  While the Top2000 makes a clear line from the left bottum corner to the right top corner, my playlist really clusters at the left. There is a cluster at the left bottum corner, which would mean that my music is partly 'sad'.  There's also a cluster at the middle of the buttom of this graph, what would be an 'anger' cluster. 
  
 In the graphics it is clear to see that my own playlist is alot bigger than the Top2000. For now it is not that big of deal, because the clusters are visible. 
  
  What this graph still misses is the 'tempo' factor. I don't know how I can add this variable without making my visualisation less clear. 

 
 ### Some codings I need to remember for myself


.# Load libraries (every time)

library(tidyverse)
library(spotifyr)

.# Set Spotify access variables (every time)

Sys.setenv(SPOTIFY_CLIENT_ID = 'e944328ebc754d5dae4607ba0b9b7aa0')
Sys.setenv(SPOTIFY_CLIENT_SECRET = '32c46cc6766a44dfb68e24f7a1aae5b6')

.# Download Grammy and Edison award playlists (pop) for 2019

Top2000 <- get_playlist_audio_features('radio2nl', '1DTzz7Nh2rJBnyFbjsH1Mh')
Mijn<- get_playlist_audio_features('11122548577','76MRLl4f6AvyYCrvs7X11P')

.# Combine data sets with a labelling variable

Mood <-
  Top2000 %>% mutate(playlist = "Top2000") %>%
  bind_rows(Mijn %>% mutate(playlist = "Mijn"))

.# Start with histogram or bar for showing one variable.

Mijn %>% ggplot(aes(x = mode)) + geom_histogram(sat= 'count')
ggplot(Mood, aes(x=mode), col=playlist) + geom_histogram(stat='count')+
  facet_wrap(~ playlist)


.# Code by dr. Burgoyne -> my research

award_labels <-
  tibble(
    label = c("By the Bright of Night", "Immaterial"),
    playlist = c("Edisons", "Grammys"),
    valence = c(0.151, 0.828),
    energy = c(0.119, 0.717),
  )
  
  
Mood %>%                       # Start with awards.
  ggplot(                      # Set up the plot.
    aes(
      x = valence,
      y = energy,
      size = loudness,
      colour = mode
    )
    
    
  ) +
  geom_point() +               # Scatter plot.
  geom_rug(size = 0.1) +       # Add 'fringes' to show data distribution.
  facet_wrap(~ playlist) +     # Separate charts per playlist.
  scale_x_continuous(          # Fine-tune the x axis.
    limits = c(0, 1),
    breaks = c(0, 0.50, 1),  # Use grid-lines for quadrants only.
    minor_breaks = NULL      # Remove 'minor' grid-lines.
    
    
  ) +
  scale_y_continuous(          # Fine-tune the y axis in the same way.
    limits = c(0, 1),
    breaks = c(0, 0.50, 1),
    minor_breaks = NULL
    
    
  ) +
  scale_colour_brewer(         # Use the Color Brewer to choose a palette.
    type = "qual",           # Qualitative set.
    palette = "Paired"       # Name of the palette is 'Paired'.
    
    
  ) +
  scale_size_continuous(       # Fine-tune the sizes of each point.
    trans = "exp",           # Use an exp transformation to emphasise loud.
    guide = "none"           # Remove the legend for size.
    
    
  ) +
  theme_light() +              # Use a simpler them.
  labs(                        # Make the titles nice.
    x = "Valence",
    y = "Energy",
    colour = "Mode"
  )
           
           
           
        
           

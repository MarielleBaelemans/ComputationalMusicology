# Computational Musicology

## Portfolio by Mariëlle Baelemans



For my portfolio project I want to use the findings out of an earlier Musicology project. In my research on what makes music 'sad' I found that the minor aspect is the thing what makes music sad. Besides that aspect, also slowness and low pitch are important for giving a listener a sad feeling. My big goal for this portfolio project is to make a widget that measures how sad someone's playlist is by comparing these factors in a 3:2:1 ratio. With these outcomes I want to make a day by day scheme for how 'sad' your day was according to the songs you listened that day. A calendar that shows your mood based on the music you listen ;). 
For now this is a too big goal, so my first step is to compare the major/minor- ratio and the mean tempo of my own playlist 'Most listened 2018' to 'Top 50 Nederland' playlist.  I have not found a way yet to measure the pitch of the songs in the playlists, so for now I use the 'loudness' as extra indicator. 

So far it seems that my own playlist is less sad than the Top 50 Nederland. 
Minor is 31% against 40% in Top 50 Nederland.
My mean tempo is 120 BPM (sd= 31.7) against 116 BPM (sd= 24,0) 
For loudness the outcome for my most listened 2018 has a mean of -8,06 ; sd= 3,55. For Top50NL it has a mean of -6,64  with sd= 2,84.

I can make a formule to calculate the sadness (because of the different in numbers this formulate is not really accurate, but it is a sketch for futher steps). Sadness= 3mode -2(tempo/1000) -loudness/10. 
Sadness(Top50NL)= 3x0,40 - 2x0,116 + 0,664 = 1.632
Sadness(My2018)= 3x0,31 - 2x0,120 + 0,806 = 1.976


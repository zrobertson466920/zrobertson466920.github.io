---
layout: post
title: Digital Analysis of SOSC Readings
---

![_config.yml]({{ site.baseurl }}/images/wordcloud_normalized.png)

![_config.yml]({{ site.baseurl }}/images/wordcloud.png)

![_config.yml]({{ site.baseurl }}/images/radarchar_overall.png)

![_config.yml]({{ site.baseurl }}/images/radarchart_MM.png)

![_config.yml]({{ site.baseurl }}/images/radarchart_GM.png)

![_config.yml]({{ site.baseurl }}/images/radarchart_CD.png)

![_config.yml]({{ site.baseurl }}/images/radarchart_DW.png)

![_config.yml]({{ site.baseurl }}/images/radarchart_SS.png)

![_config.yml]({{ site.baseurl }}/images/radarchart_HS.png)

Sentiment analysis is extensible. We can also analyze sentiments about 'disgust'. For this we look at Freud's 'Civilization and it's Discontents'. We see that the biggest spike happens about 20% into the book. This corresponds roughly with pages 30 through 36. This maps directly to the discussion of the 'oceanic feeling'...

![_config.yml]({{ site.baseurl }}/images/figure_3.png)

Of course, we also have simpler methods of analysis available. Assume you haven't read 'History of Sexuality' but you have read 'Civilization and it's Discontents'. If you plot a word cloud of the most frequently used words for both texts it'll become very clear which book corresponds to which plot. 

![_config.yml]({{ site.baseurl }}/images/figure_4.png)

![_config.yml]({{ site.baseurl }}/images/figure_5.png)
 
Ok, so this guy is historical...besides that, this seems to be a book that relates a wide swath of material together. The main things handeled seem to be the 'repressive hypthesis', 'social body', 'power relations', and 'force relations'. Say, you're getting desperate, you  r e a l l y  need to think like this guy. Now is the time to start 'copying' how this guy talks. We'll use a Markov Chain, it's a model of all the conditional probabilities of the words. Put simply, it gives likely predictions for the next word an author would use, given that he already said something. Let's find out about this 'repressive hypothesis'. We construct our model, the predict what Focoult would say if he started a sentence with our key relation. We get,

'The' repressive hypothesis and the discourse on sex for economic reasons appears to dominate us and our own have been designated as the principle which made it possible for perverse behavior patterns to arise and made to disengage the juridical dimension in the privilege of city dwellers and libertines, but was made of it.

Discourse looked important, what's that about Focoult?

"Discourse transmits and produces power; it enables one to make one’s voice tremble, for an opposing strategy." - Definitely Focoult

Nice. What happens if you started taling with Freud? Let's have Freud respond to this idea of discourse (we seed with enables/one's).

"One may say, of the very nature of the existence of smaller communities, through which the evolution of culture, therefore, is justice — that is, genetic explanation of this kind is indispensable." - Definitely Freud

"Existence of a master? This is rather to show how this association was formed, and one defines its effects are perhaps not so much talk about such things, how those who discounted it out in the field of sexuality." - Focoult

"Ego and this task ; and that ought not to regard another as an instinct of destruction first made its appearance in psycho-analytical literature and how far away from its value and not belonging to culture aU the activities aimed at controlling the forces of cohesion consist predominantly of identifications of the tension between ego and what originates in the end it can also hold himself guilty" - Freud

![_config.yml]({{ site.baseurl }}/images/bigram.png)

![_config.yml]({{ site.baseurl }}/images/tf_idf.png)




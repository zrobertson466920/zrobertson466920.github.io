---
layout: post
title: Digital Analysis of SOSC Readings
---

We have a progression of the sentiment in 'Second Sex' as the book progresses. There's a statistically significant decline in 'positive' sentiment as a function of the narrative. The correlation is weak only at 20%. 

![_config.yml]({{ site.baseurl }}/images/figure_1.png)

![_config.yml]({{ site.baseurl }}/images/figure_2.png)

Sentiment analysis is extensible. We can also analyze sentiments about 'disgust'. For this we look at Freud's 'Civilization and it's Discontents'. We see that the biggest spike happens about 20% into the book. This corresponds roughly with pages 30 through 36. This maps directly to the discussion of the 'oceanic feeling'...

![_config.yml]({{ site.baseurl }}/images/figure_3.png)

Of course, we also have simpler methods of analysis available. Assume you haven't read 'History of Sexuality' but you have read 'Civilization and it's Discontents'. If you plot a word cloud of the most frequently used words for both texts it'll become very clear which book corresponds to which plot. 

![_config.yml]({{ site.baseurl }}/images/figure_4.png)

![_config.yml]({{ site.baseurl }}/images/figure_5.png)

Let's take this 'analogy' further. Maybe you haven't actually read 'History of Sexuality' at all. Maybe you got distracted writing a blog post or something...then maybe you'd like to have some talking points for class. How might you go about that? Well, you're going to want to know the key terms for the text. We have the key words, 'Power', 'Sexuality', 'Sex', 'Discourse', 'Deployment', 'Century'. That's not really enough to start talking without sounding like an idiot. Let's try finding related words. These are words that frequenty appear together. 

 1 nineteenth  century       47
 2 eighteenth  century       39
 3 repressive  hypothesis    22
 4 scientia    sexualis      19
 5 social      body          16
 6 power       relations     12
 7 seventeenth century       11
 8 force       relations      9
 9 ars         erotica        8
 
Ok, so this guy is historical...besides that, this seems to be a book that relates a wide swath of material together. The main things handeled seem to be the 'repressive hypthesis', 'social body', 'power relations', and 'force relations'. Say, you're getting desperate, you  r e a l l y  need to think like this guy. Now is the time to start 'copying' how this guy talks. We'll use a Markov Chain, it's a model of all the conditional probabilities of the words. Put simply, it gives likely predictions for the next word an author would use, given that he already said something. Let's find out about this 'repressive hypothesis'. We construct our model, the predict what Focoult would say if he started a sentence with our key relation. We get,

'The' repressive hypothesis and the discourse on sex for economic reasons appears to dominate us and our own have been designated as the principle which made it possible for perverse behavior patterns to arise and made to disengage the juridical dimension in the privilege of city dwellers and libertines, but was made of it.

Discourse looked important, what's that about Focoult?

"Discourse transmits and produces power; it enables one to make one’s voice tremble, for an opposing strategy." - Definitely Focoult

Nice. What happens if you started taling with Freud? Let's have Freud respond to this idea of discourse (we seed with enables/one's).

"One may say, of the very nature of the existence of smaller communities, through which the evolution of culture, therefore, is justice — that is, genetic explanation of this kind is indispensable." - Definitely Freud


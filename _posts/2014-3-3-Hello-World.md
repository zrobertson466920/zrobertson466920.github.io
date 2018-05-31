---
layout: post
title: Digital Analysis of SOSC Readings
published: true
---

![_config.yml]({{ site.baseurl }}/images/wordcloud_normalized.png)

This is a quick tour of the basic methods used in [Digital Humanities](https://en.wikipedia.org/wiki/Digital_humanities) to analyze textual data. This field lies at the intersection of computing technology and the humanities with high potential to provide novel insights into the study of textual data. For example, what you see in the figure above is a collection of the most important words used in the texts we've been reading this quarter.
    
## Sentiment

This form of analysis combines both traditional and novel data analytic techniques. One of the more innovative methods introduced from the field of Natural Language Proccessing is 'Sentiment Analysis'. By assigning subjective qualia such as emotion to words in a text, it's possible to gauge how much impact a text has along any given subjective dimension. Of course in order to this, you need to have a fairly accurate idea of which words go with which emotions and so on. Nevertheless, once the data bases have been compiled, it's a simple coding problem to start applying these sentiment dictionaries. Below, are a series of radarcharts showing how each of the books we've read this quarter fills out the ten dimensions of sentiment available. In order we have: Metaphysics of Morality, Geneology of Morality, Civilization and its Discontents, Dark Water, Second Sex, and History of Sexuality. The first chart is the overall sentiment shape for the texts we've read.

![_config.yml]({{ site.baseurl }}/images/radarchar_overall.png)

![_config.yml]({{ site.baseurl }}/images/radarchart_MM .png)

![_config.yml]({{ site.baseurl }}/images/radarchart_GM .png)

![_config.yml]({{ site.baseurl }}/images/radarchart_CD .png)

![_config.yml]({{ site.baseurl }}/images/radarchart_DW .png)

![_config.yml]({{ site.baseurl }}/images/radarchart_SS .png)

![_config.yml]({{ site.baseurl }}/images/radarchart_HS .png)

Moreover, sentiment analysis is extensible in that we can track sentiment as a function of narrative progression. For example we can analyze sentiments about 'disgust' in Freud's 'Civilization and it's Discontents'. We see that the biggest spike happens about 20% into the book. This corresponds roughly with pages 30 through 36. This corresponds roughly with his discussion of the 'oceanic feeling'. Earlier in the class, I'd suggested that Freud's negative opinion of the feeling might have less to do with its non-existence and more to do with his bias. This analysis would offer evidence in support of that view.

![_config.yml]({{ site.baseurl }}/images/figure_3.png)

## Conventional Methods

Of course, we also have simpler methods of analysis available. Assume you haven't read 'History of Sexuality' or 'Civilization and it's Discontents' and you need to be able to tell if some text you've been reading corresponds more with the former or the latter. If you were to plot a word cloud of the most frequently used words for both texts you might find the challenge easier than you first thought.

![_config.yml]({{ site.baseurl }}/images/figure_4.png)

![_config.yml]({{ site.baseurl }}/images/figure_5.png)
 
Alternatively, perhaps you need to start sounding like one of these authors. For instance, perhaps you need to sound good in your SOSC class :) . Then it's also possible to create models of the relations between all the words in the text. Put simply, it gives likely predictions for the next word an author would use, given what they previously said. For instance, we can create one such Markov model for History of Sexuality. It outputs,

> 'The' repressive hypothesis and the discourse on sex for economic reasons appears to dominate us and our own have been designated as the principle which made it possible for perverse behavior patterns to arise and made to disengage the juridical dimension in the privilege of city dwellers and libertines, but was made of it.

You can go further and have the output of one model input into the feed of the next. Taking Focoult and Freud as a pair we can get,

>"Discourse transmits and produces power; it enables one to make one’s voice tremble, for an opposing strategy." - Definitely Focoult

> "One may say, of the very nature of the existence of smaller communities, through which the evolution of culture, therefore, is justice — that is, genetic explanation of this kind is indispensable." - Definitely Freud

> "Existence of a master? This is rather to show how this association was formed, and one defines its effects are perhaps not so much talk about such things, how those who discounted it out in the field of sexuality." - Focoult

> "Ego and this task ; and that ought not to regard another as an instinct of destruction first made its appearance in psycho-analytical literature and how far away from its value and not belonging to culture aU the activities aimed at controlling the forces of cohesion consist predominantly of identifications of the tension between ego and what originates in the end it can also hold himself guilty" - Freud

## The Bigger Picture

At the end of the day, this sort of analysis is only as useful as the conclusions it can help us reach. Wrapping up, it's a very informative to know what makes each of the authors we've read stand out this quarter. It's very easy to have each author slide into the next without any larger context for what's going on. We can start to answer this question by looking at which words were particular to a specific author. It's possible to weight words by their 'exclusivity' to a specific author. Intuitively, this assings low exclusivity to common words (e.g 'the', 'of', 'a') and assigns high exclusivity to key words (e.g. 'sexuality', 'woman', 'ego'). We can perform this analysis over all the texts we've read this quarter to start drawing conclusions about the major topics dealt with by each author.

![_config.yml]({{ site.baseurl }}/images/tf_idf.png)

We can further as well. By looking at the corpus of texts that are included in the readings we can isolate words that are well connected with other words. By analyzing the 'matrix' of word connections we can see a visualization of the major themes of the quarter. In conclusion, it's clear that the computational methods available can serve as a valuable stepping stone when trying to critically examine text.

![_config.yml]({{ site.baseurl }}/images/bigram.png)

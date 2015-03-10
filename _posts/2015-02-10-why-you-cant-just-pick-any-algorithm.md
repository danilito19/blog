---
layout: post
title: "Why you can't just pick ANY algorithm"
modified: 2015-02-10 10:09:52 -0600
tags: [statistics, data science, Python, R]
image:
  feature: 
  credit: 
  creditlink: 
comments: 
share: 
---

I can't begin to count the number of times I downloaded a new dataset to work on or describe the accompanying feeling of excitement, the rush to find new answers. Anyone who gets turned on by data has felt it. And amidst that rush, you forget about the basics. You jump at it and apply any algorithm to see what you come up with. But this could be the beginning of a big data science mistake. 

Who doesn't love a yhat blog post? I certainly do, but a section of [this post](http://blog.yhathq.com/posts/random-forests-in-python.html) really struck me. It describes Random Forests as a good-for-all algorithm that you can apply to any dataset and will get good results. Now, this is not a criticism of the algorithm itself, but a criticism of the idea that when in a doubt we should turn to the most reliable of models. What about the assumptions? Did you consider overfitting, bias, variance, etc? Just because a model has been proven to fit data well does not mean we should abuse it. 

This read, in addition to the many conversations I have had with more advanced data scientists and the comments I read all over the Internet telling aspiring data scientists to just "start playing with the models," makes me question the direction that the field is turning. Do we want people who can simply code the algorithm and make hasty conclusions, or do we want trained statisticians and analysts who can best choose the models to use?

And I get it. Data science has opened its arms for open-source work and newcomers (including me). Moreover, the huge demand for data scientists results in new degrees and bootcamps are popping up everyday to meet this demand, but we cannot allow for data "science" to lose its rigor. For in fact it is a science with a methodology.

That said, this comes from a currently inexperienced data analyst, one who has not yet delved deeply into the mathematical assumptions of these models. But my lack of experience doesn't make me gulliable. I wince every time I hear a data scientist say "oh, just download some data and apply some models - that's how you learn". Yes, that's how you learn to clean data and to code a model, but did you learn much about what this model is best for?

Here is my biggest concern. When we communicate to people who are learning data science that it is ok to just throw models indiscriminately on data or when we unexplicidly say that you can become a data scientist by doing a 12-week bootcamp, we are putting the data and results at risks. We are telling people who may not understand the models (including me right now) that it is fine to make conclusions - even to make policy choices - just by applying a model to the data. This is risky and can lead to bad science, bad journalism, and bad public policy (and business) decisions. 



I don't want to be good at coding a model - I want to be a good applicant of theoretical assumptions to solve problems in the real world.
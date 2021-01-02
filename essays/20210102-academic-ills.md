# Triaging Boring Publications

This short essay is a response to [Daniel Lemire's post](https://lemire.me/blog/2021/01/01/peer-reviewed-papers-are-getting-increasingly-boring/). 
Originally, I intended for this to be quick tweet, but after line 35 of my response, I realized that it's too long for Twitter and needs to be an essay on its own.

## Tl;dr
Interesting read; I do have some reservations, though. Generally speaking, I see the same symptoms, but think the diagnosis is too narrow to be generally applicable. 
Instead of the mono-causal need for "customers," I'd attest a melange of a variety of shortcomings, which in combination lead to slow pace: 
- fashion, 
- conferences vs journals, and 
- a good dose of [Goodhart's law](https://en.wikipedia.org/wiki/Goodhart%27s_law).

## Opposing view: Need for customers
I am opposing the view that science *needs* to have a customer. 
There are practical fields, where this clearly is useful and reasonable.
But there are also fields that are at odds with this requirement. 
Many subjects have theoretical fields, where there may not be a customer available right now. 
A customer may emerge in 100 or 200 years in the future. 
To mandate a customer could effectively stifle future research endeavors.

## Multi-causal vs mono-causal

### Fashion
Over the past ten years, I've often felt that parts of computer science I am familiar with---programming languages and computer security, both to a different degree---have been subjected to fashions.
Among the non-CS scientists I know, the feeling is pretty much the same.
The fashion aspect in science is pervasive: it influences choice of topics, relative ranking of topics, being "first," importance of venues, and certainly a lot of other things. 
The key downside of this fashion circuit is that it often moves to fast for science to matter and/or mature: 
When one gets past the low-hanging fruit and the fashion circuit moved elsewhere, what are you supposed to do?

### Conferences
CS conferences need reconsideration, too. 
I've always held the opinion that CS is *not* special and that the conference-focus is merely an artifact of its age. 
Conferences are also subject to fashion, but the major issue---in my opinion---is that the function of conferences seems to be defunct. 

When you take 100pct of the papers, probably half that are not a good fit (premature submission, subpar language, etc.) can be identified relatively quickly. 
But what about the rest? Depending on the acceptance rate, some papers get in, others don't. 
The acceptance rate has become a proxy for selectivity and prestigiousness, but in reality it is often random and more of a social process of the PC members. 
(A couple of studies seem to confirm the random nature of PC decisions, cf. [NIPS experiment](http://blog.mrtz.org/2014/12/15/the-nips-experiment.html).)
Another big downside of conferences is PC selection, which is also affected by social and sometimes also institutional processes and biases.
In combination with scientist tendencies of being "experts" in all subfields of a certain field. 
I have seen the most blatant mistakes from so-called "expert" reviews that are obviously and objectively false.
Reviewers are also not accountable for writing bad reviews, meaning that there is simply no downside to writing bad reviews.

### Goodhart's law
Goodhart's law is also complicit. Scientists are smart people and will figure out a way to game the system. If the number of publications is relevant, they will figure out way to maximize publication count. Replace publications with any kind of goal, then that will be optimized for.

### Perils of systems research
[Michael Stonebraker](https://www.youtube.com/watch?v=DJFKl_5JTnA) summarized his "fears" for database research a couple of years ago, and I feel his diagnosis holds for systems research in general. 
I talked to several senior members of the PL community and they confirmed that PLDI before the 00s was a lot more about ideas without a solid, industrial-strength implementation and full-blown evaluation. 
Nowadays, a grad student has to implement something for two years to obtain interesting results, only to be shot down by a reviewer in what often appears to amount to less than one hour of work by a reviewer.

Another downside of systems research is that the community is often acting in a self-destructive way.
Some people prefer to reject a great idea demonstrated in a sub-par implementation. 
Others argue that implementation in framework A is stupid, and the authors should "in good taste" use framework B. 
There are infinite ways of constructing such arguments, but the truth of the matter is: people tend to lose sight of the forrest for the trees. 
A good idea with a shoddy implementation is much better than a shoddy idea with a good implementation. 

# What are we to do?
We need an environment conducive to and fostering research. Personally, I believe that we need to stop the nonsensical and gigantic waste of tax payer money to maintain the conference circuit that shows diminishing returns. 
If we had journals, with smaller focus and smaller, but actual expert reviewers, we could ensure progress in **all** subfields regardless of fashion. 
By having two or three journals in a sub-field, one could focus on early-stage/prototypical v late-stage/finished work.
Goodhart's law ensures that this switch is not ideal and will also have downsides. 
The alternative is to maintain the status quo.
Whether this is wise or not, I do not know. 
What I do know, though, is that Einstein's famous definition of insanity may be relevant:
"*Doing the same thing over and over again and expecting different results.*" 


(c) stefan brunthaler, 2021-01-02, 15:01
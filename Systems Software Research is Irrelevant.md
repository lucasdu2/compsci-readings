date: 2023-05-12
tags: #compsci #systems #notes 

[link](http://doc.cat-v.org/bell_labs/utah2000/utah2000.pdf)
A presentation by Rob Pike in 2000 giving his perspective on systems software research. He is pretty pessimistic about systems research–in fact, he calls the talk a “polemic,” and believes "the situation is genuinely bad and requires action.”

## Quick notes on the talk
### His thesis
- Systems software research is an afterthought excitement around computing
- Systems research in academia and industry is becoming irrelevant
	- Really no new operating systems or languages being widely adopted in industry (which he claims was not the case in the 70s and 80s)
 - Industry largely ignores research, research community writes papers and not software
 - Take Linux–*not* an innovation, just a copy of old things (aka Unix), but proves a point:
 > Linux’s success may indeed be the single strongest argument for my thesis: The excitement generated by a clone of a decades-old operating system demonstrates the void that the systems software research community has failed to fill.
- Systems research these days is too “scientific”–too much measurement and comparison, no new ideas, art, or novelty
### The causes
- Homogenized architectures caused by rise of PC, major area of systems research (new architectures, portability across architectures) now made mostly irrelevant
- Rise of web/internet–research has not contributed much
- Growing number of standard that must be honored, very annoying external structure that stifles novelty
- Change of scale–systems work has begun to require increasingly more effort (easy things already done, large number of external constraints), academia is not equipped to take on long-term projects
	- So only industry is really able to undertake large-scale systems work (and industry is bound to their investors, making money, ROI)
	- Academia largely: measures things (instead of building them), microspecializes in a niche (not *systems* work), tweaks existing things
- Startups and “industrialization” of research–everything is predicated on what will make the most money most quickly, long-term work and work that shakes the orthodoxy are not funded
	- This is true of both government and industry funding
	- Metric of merit = money-making potential, instead of what it should be: a good idea
### The cure
- Maybe “cure” is too strong a word for what Pike does, but he does list some things that will help
	- Stop focusing on short-term time scale and practical results, *try new things*
	- *Build systems*–“narrowness is irrelevant; breadth is relevant”
	- Work on how systems actually behave and work (e.g. interfaces, architecture), not how they compare
	- Make industry want your work (by having good ideas)–don’t let industry dictate what you should work on
- Pike also lists a number of ideas for things to work on/build in systems–I actually think these are really interesting because many of them are actual research areas that modern labs are still working on (e.g. languages for distributed computation, component architectures)

## My thoughts at the moment
- I do think that many things have changed since 2000 in the systems research area, although a good number of things in this talk are still relevant and actually quite prescient
- Systems research is still a relative “afterthought,” in my view, in the grand scope of computing research
	- There doesn’t seem to be any real way around this–Pike already lists a number of the causes
	- Homogenization and standardization of operating systems/protocols (and the idea that these are “good enough” and are mostly solved problems)
	 - Focus on making money, and on applications that can be monetized across the widest demographic (e.g. the grandmas that Pike references in his talk)
	 - Systems research and innovation doesn’t have the intended effect of making money, nor is it directed at the “average” consumer
	 - Instead, things like AI are center-stage in computing these days–consumers interact with them directly, they directly contribute to money-making
- I do think that some of Pike’s talk rings of nostalgia–the “pure” systems research that he once knew has fallen by the wayside, and I don’t really think there is a way out of that
	- There are many things (as he points out) that are now just accepted as “the way things are” (e.g. in operating systems, standards and protocols)–it is an orthodoxy
	- And I believe him when he says that there are better ways to do these things that many younger people (like me) view as foundational
	- Unix has many flaws, but it’s “good enough”–fixing some of these flaws would be great, but how would it ever get wide adoption to be “relevant”?
	- IP is a deeply flawed protocol, especially in the modern era of mobile computing, but is it ever going to be possible to replace it with something better?
	- Some things have become entrenched in computing (another example: Javascript sucks, but the entire internet uses it), for better or worse–I do appreciate Pike’s exhortation to try and make some of these things better from the ground up, to try and do things in a novel way, but I do wonder how feasible that is nowadays
- However, the importance of AI, and more generally, large-scale data acquisition and storage, has made some parts of systems research–“the cloud,” distributed data systems, databases–quite important
	- So in this area, I think systems research is quite well-funded by industry and very actively worked on in academia as well
	- And “good ideas” are important here, because “good ideas” actively contribute to making more money
	- It’s a different enough paradigm that small, incremental changes and observance of the “orthodoxy” that Pike rails against may actually be outweighed in terms of financial benefit by good, novel ideas
	 - Related tangent: distributed systems are great but Kubernetes is a horrible hack in many ways, is there a better, more novel way of doing things?
- I do think a lot of Pike’s points on “what to do” about the state of systems research are still very relevant today–e.g. the focus on trying new things, instead of just measuring or tweaking existing things
	- Do something exciting! Or give an exciting demo! as Pike puts it.
	- Again, I don’t know how feasible some of these “novel ideas” will be, but at least it makes me excited about the future of computing



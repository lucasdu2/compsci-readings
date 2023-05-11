tags: #compsci #programminglanguages #notes 

Some notes on a set of webpages talking about "little languages", e.g.
languages designed to solve a very specific problem (as opposed to general-purpose
languages, which are the vast majority of popular languages today).

# Little Languages Are The Future Of Programming
[link](https://chreke.com/little-languages.html#steps2012-32)
- Main blog post that links to a lot of other interesting things w.r.t language
design and the problem of complexity
- Little language = language specialized to a particular problem domain, that 
does not include many features found in conventional languages
    - e.g. SQL (sort of), or Dhall for configurations
    - some other names: domain-specific languages (DSLs) or problem-oriented  
languages
- Main problem that little languages are intended to solve: code complexity
    - Modern codebases have grown increasingly enormous, our human capacity to
comprehend these codebases is still fixed
    - "big code" like this has a number of problems: hard to add new things 
since dependencies are unclear (code appears fragile), hard to onboard new hires
(and generally hard to understand for everyone, which results in bugs, security 
vulnerabilites, and hacked designs)
Interesting quote by Alan Kay on this point, from "A Conversation with Alan Kay"
linked below:
> If you look at software today, through the lens of the history of engineering, 
> it’s certainly engineering of a sort—but it’s the kind of engineering that people without the concept of the arch did. Most software today is very much like an Egyptian pyramid with millions of bricks piled on top of each other, with no structural integrity, but just done by brute force and thousands of slaves.
- Idea that all of the boilerplate, "noise" of modern software can be boiled 
down to a few primitives, ala Maxwell's Equations in physics, is very appealing
    - On this point, see Alan Kay's STEPS project report [here](http://www.vpri.org/pdf/tr2012001_steps.pdf)
    - The idea is this: you can boil down all the patterns internal to your 
specific problem domain, say graphics rendering, and make these patterns a core
primitive of your language--this allows for much greater compactness in code,
since you can simply express a complex idea with the primitive (something
equivalent to Del notation in vector calculus), instead of having to express it
in so many lines with a "conventional" general-purpose language
- On general-purpose, high-level languages, and why they still won't solve the
problem of bloated code
    - Sure, a high-level language will provide more nice abstractions
    - But there is an inherent problem with general-purpose languages, see quote
below
> The problem with general-purpose languages is that you still have to translate your problem to an algorithm, and then express the algorithm in your target language. Now, high-level languages are great at describing algorithms, but unless the goal was to implement the algorithm then it’s just accidental complexity.
- Generally, small languages give further benefits that are related to smaller
code size/greater comprehensibility
    1. Better static analysis, because reasoning about the program becomes much easier
    2. Potential performance speedups: limited design scope b/c program is not
expressed in terms of algorithms means that the runtime is free to choose 
whatever execution plan it wants (as long as it returns the desired result). Thus,
the runtime can theoretically always choose the fastest known algorithm/execution
plan for the job.
# The end of history for programming
[link](https://www.haskellforall.com/2021/04/the-end-of-history-for-programming.html)
- Maybe the most interesting post in this sequence of documents, talks about 
the author's vision for the future of programming languages, e.g. "the end of
history" in this area
- Primary theory: programming languages will increasingly move towards "mathematical
DSLs"
- "Mathematical DSLs" comprise 2 parts: a pure mathematical programming language
and a backend runtime
    - In a sense, this the logical extreme of the current paradigm, where modern
languages still incorporate pure execution logic with certain runtime concerns,
like memory management
    - Gonzalez's central claim is that these two core programming language 
concerns will become more sharply divided over time: runtimes vs. mathematical 
expressions
- This post does not directly address the idea of "little languages", but it
is easy to extrapolate the relation, which is noted below
    - "Pure mathematical programs" embody the "essence" of a program (using 
terminology introduced in "No Silver Bullet", linked below), while the runtimes
embody the "accident" (this may be a simplification, but generally should be
an apt analogy)
    - "Little languages" also aim to embody the essence of a program, in the same
way that a "pure mathematical program" in Gonzalez's sense aims, and aims to 
push the accidental complexity of a program, e.g. the specific execution algorithms 
used, to the runtime backend, which is then abstracted into a core primitive
- This post also links to an interesting snippet from Dijkstra that talks about
"natural language programming"
    - Gonzalez and Dijkstra both regard "natural language programming" (which
seems one focus of AI research these days) as foolish
    - "Symbols are a privilege" not a burden, and make expressing ideas far
easier and clearer, according to Dijkstra
    - Trying to program simply by expressing an idea in "natural language" and
having an AI do the rest is a step backwards in some sense, from this perspective

# A Conversation with Alan Kay
[link](https://queue.acm.org/detail.cfm?id=1039523)
- Lots of things in here on programming languages and software development as a
whole, I'll list some of them I found most interesting (at least on the first
couple passes) below
- Idea of "pop culture" vs. "literary culture" in programming
    - A lot what makes a language popular is not necessarily intrinsic merit,
but rather how it was marketed or explained to the public
    - Commercialization and rapid dispersion of computing led to a sort of 
lowest common denominator spread of ideas in software, see large quote below
> One could actually argue—as I sometimes do—that the success of commercial personal computing and operating systems has actually led to a considerable retrogression in many, many respects.

> You could think of it as putting a low-pass filter on some of the good ideas from the ’60s and ’70s, as computing spread out much, much faster than educating unsophisticated people can happen. In the last 25 years or so, we actually got something like a pop culture, similar to what happened when television came on the scene and some of its inventors thought it would be a way of getting Shakespeare to the masses. But they forgot that you have to be more sophisticated and have more perspective to understand Shakespeare. What television was able to do was to capture people as they were.

> So I think the lack of a real computer science today, and the lack of real software engineering today, is partly due to this pop culture.
- The idea of a "pop culture" in computer science/programming that has led to a
decrease in quality of software and a lack of "real" computer science seems a 
little arrogant, but I think there is truth there, e.g. Javascript being an
objectively terrible language but perhaps also the most widely interacted with
- Kay believes a programming language is primarily a user interface and should
be elegant, rather than an "agglutination" of features (UI should be understandable,
instead of looking like a nuclear reactor control panel)
    - Languages can be divided into "agglutinative languages" (where features
are simply stacked together without a core connecting thread), and "style languages",
which have "a real center and imputed style to how you’re supposed to do everything"
    - Agglutinative languages leads to agglutinative (read: messy and bloated) code,
while style languages lead to more elegant results

# No Silver Bullet
[link](http://worrydream.com/refs/Brooks-NoSilverBullet.pdf)
- Introduces the idea of "essence" vs. "accident" in software engineering
    - Essence = the essential difficulties (which cannot be removed)
    - Accident = the accidental complexities (which should be removed)
    - This can be seen as a direct parallel to the arguments for "little 
languages" above, although Brooks does not explictly suggest that particular solution
- Focus of the article is on how to improve software quality and programmer
productivity, which means removing as much accident as possible and making the 
essence easier
- On AI: AI in the sense of "natural language programming" (which Brooks
calls "facilitation of expression") is not useful. However, AI as an "expert 
system" (which basically entails a programming advisor program) could be very
useful.
    - "The hard thing about building software is deciding what to say, not saying it."
    - An "expert system" can try to distill knowledge from human experts into
generalizable heuristics and act as expert guidance by suggesting best practices,
helping digest codebase complexity, etc.
- Grow, not build software
    - Probably what I found most interesting, since the idea of "building" 
software is so prevalent today (it's what everyone seems to say, perhaps
unconsciously)
    - "Building" software has led to overly complex, bloated software that lacks
cohesiveness (similar to "agglutination" in the words of Alan Kay)
    - Software today is too complex to be specified in advance and be built
flawlessly
    - Should instead try to grow software organically, fleshed out bit-by-bit,
but should run at each stage of its development 
    - Brooks claims that designing programs to be "grown" organically, from the
outset, has resulted in "dramatic results" in his classes

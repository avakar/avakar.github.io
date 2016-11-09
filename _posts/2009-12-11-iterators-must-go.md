---
title: Iterators Must Go
tags: [stl, c++]
---

This is the catchphrase associated with the [presentation][1] given by Andrei Alexandrescu during boostcon 2009. I heard about that blasphemy a while ago, but I had neither the  time, nor the interest to read it back then.

I finally read it today. You see, I was implementing an incidence graph (as defined by [Boost Graph Library][2]) yesterday; to do that one has to write an iterator that, for a given vertex, traverses a sequence of its outgoing edges. Since the graph was potentially very huge, it wasn't represented explicitly. Instead, the set of adjacent vertices was calculated on-the-fly.

  [1]: http://www.boostcon.com/site-media/var/sphene/sphwiki/attachment/2009/05/08/iterators-must-go.pdf
  [2]: http://www.boost.org/doc/libs/release/libs/graph/

It was quite cumbersome. The amount of state I had to keep inside the iterator was non-trivial, which, while certainly unavoidable, gave me a headache when I realized

 1. that I'd have to keep the same state twice, once for each bracket of the sequence, and
 2. that I would have to abuse the state to denote the end-of-sequence marker.

I eventually ended up writing an enumerator that provided `valid`, `next` and `target` member functions (which happen to exactly coincide with Alexandrescu's `empty`, `popFront` and `front` members of the `InputRange` concept) that I would use internally and then wrapping it in a pair of iterators for the sake of BGL.

After writing that iterator-from-hell yesterday, I came to the same conclusion as Andrei --- iterators should probably be replaced with a more straightforward concept of a range. Ultimately, even my blind devotion to C++ and STL design didn't prevent me from agreeing with the slides on almost all accounts.

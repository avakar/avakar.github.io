---
title: Chaotic attractors
tags: [chaotic attractors]
mathjax: true
---

This Monday there was a deadline for submission of bachelor's theses at my faculty. My girlfriend submitted a thesis on chaotic attractors. It concentrated more on their visual and aesthetic aspect rather than the scientific one, but I think it's still worth reading through even for those, who can't appreciate art.


## Phase space

The state of every physical system can be precisely described by a bunch of numbers. In fact, for every [degree of freedom][wiki:degfree] of the system, we need exactly two values: a position and [a momentum][wiki:momentum].

To illustrate, consider a simple system containing [an ideal spring][wiki:spring], whose one end is fixed at the origin, while the other is attached to a mass and can move freely towards or away from the origin. Such a system only has a single degree of freedom, since the state of the system can be completely described by the displacement of the spring from its neutral position, and the velocity at which the mass is moving (which corresponds to its momentum).

With time, of course, the system will evolve and the state of the system will change. However, if we know the complete state of the system at some point in time, we can precisely predict how the system will evolve. The spring will, for example, exert force on the mass, moving it to its neutral position. At that point the deviation will be zero, but its momentum maximal. Due to inertia the mass will continue moving, slowly decelerating to a full stop, then returning back to its neutral position, and eventually reaching its initial state.

For systems with only a single degree of freedom, we can easily plot their evolution. Let a horizontal axis represent a position and let vertical axis represent a momentum. Every point in this *phase space* represents a unique state of the system. An evolution of the system corresponds to a curve in the phase space. In our spring example, the system will plot a closed curve---an ellipse.

Systems with $n$ degrees of freedom have a corresponding $2n$-dimensional phase space. It is, of course, much harder to plot.

### Attractors

Let's assume that the spring from our system is not ideal and loses some of its energy to heat. Such a damped spring will eventually reach a no-energy state with no deviation and zero momentum (a state at the origin of the phase space). The system will of course no longer plot a closed ellipse, instead, the system will slowly spiral towards the origin---we'll always end up there eventually, no matter where in the phase space we start.

The no-energy state attracts neighboring states towards itself. A set of points with such a behavior is called *an attractor*. I will forgo the precise definition, you can [look it up on Wikipedia][wiki:attractor]. In the case of a damped spring, a single point forms an attractor. However, attractors in a more complicated systems can have various sizes, shapes and dimensionalities. In fact, the attractor can even form a fractal. This can happen in systems that evolve rather chaotically, such attractors are therefore called *chaotic* or *strange*.

Besides being attractive (no pun intended) to physicists, chaotic attractors have artistic value as well, as they tend to have eye-pleasing shapes and non-trivial structure.

## Chaotic attractors

The thesis that I mentioned at the beginning of the post concerns itself with chaotic attractors, or, more precisely, with various methods of rendering them. Since all theses are available to public, you can [read it][muni:thesis] (provided of course that you understand the Czech language).

There's also [a program][muni:program] that was developed as a part of the thesis, that you can use to generate and render some of the types of chaotic attractors. Note, that you'll need Windows and OpenGL-enabled graphics drivers. I haven't tested under Wine. The interface is in English, but would have been pretty intuitive even if it were in Czech.

If you don't want to bother yourself with downloading anything, just take a look at [my girlfriend's flickr page][otx:flickr]. I think the pictures are pretty amazing and that you might find them amazing as well.

  [wiki:spring]: http://en.wikipedia.org/wiki/Spring_(device)
  [wiki:degfree]: http://en.wikipedia.org/wiki/Degrees_of_freedom_(physics_and_chemistry)
  [wiki:momentum]: http://en.wikipedia.org/wiki/Momentum
  [wiki:attractor]: http://en.wikipedia.org/wiki/Attractor
  [muni:thesis]: http://is.muni.cz/th/173268/fi_b/atraktory.pdf
  [muni:program]: http://is.muni.cz/th/173268/fi_b/aTraktor.zip
  [otx:flickr]: http://www.flickr.com/photos/otx/sets/72157618824250647/

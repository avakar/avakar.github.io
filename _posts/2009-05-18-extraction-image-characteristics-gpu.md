---
title: Extraction of Image Characteristics on GPU
tags: [thesis, similarity search, mufin, gpu]
---

While walking from school today, I ran into my bachelor's thesis' opponent (opponent is just the name of one of the members of thesis committee here in Czech Republic) and we chatted for a while. He told me that there is an interesting problem for me to tackle for my master's thesis.
<!--break-->
There is an interesting project that my opponent and his colleagues work on at my university. It's called [MUFIN][mufin]: Multi-feature Indexing Network. It's basically a distributed database that is optimized to perform indexing in high-dimensional spaces. For an interested reader, there is [a live demo of MUFIN][mufin-demo] containing a moderately large database of images and capable of answering similarity queries.

The image search itself is not a very involved subject. First, a vector is extracted from every image. This vector contains about 250 components, each of which describes certain characteristic property of the image (for example its average color, shapes that are present etc.). These vectors are then indexed in a [metric tree][metric-tree].

Apparently, the problem at hand is the extraction of the characteristic vectors. Currently, it takes about 2 seconds to process one image, which is too long considering that the plan is to have over 1,000,000,000 images in the database. And this is where GPU comes in---in theory, it might be possible to significantly speed up the extraction using the streaming processors that are embedded on almost all modern graphics cards.

I'm very excited about the prospect of working on this project. It has the potential to teach me something new and would also be useful to others. I was quite tempted to accept the thesis right away.

  [mufin]: http://mufin.fi.muni.cz/
  [mufin-demo]: http://mufin.fi.muni.cz/imgsearch/
  [metric-tree]: http://en.wikipedia.org/wiki/Metric_tree

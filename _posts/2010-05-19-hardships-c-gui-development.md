---
title: The Hardships of C++ GUI Development
tags: [widget toolkit, gui development, gui, ggui, c++]
---

This post was intended to be about the development of GUI applications with C++, but if you read forward, you'll soon realize that it became more of rant, a monument to my frustration with existing C++ GUI toolkits (sometimes referred to as widget toolkits. Applying a bit of a self-censorship, I've managed to tune down the tone of the article a bit, though I had to remove a lot of "craps" from the text in the process.

First question to answer is why would one even want to use a GUI toolkit. As far as I'm concerned, the answer is portability -- I work on both Windows and Linux systems and I want my applications to work on both. The standard C++ library provides no support for GUI development, which is actually fine with me, as I believe that such a complex functionality should not be part of any standard library. (Java, I'm looking at you. The quality of your GUIs is pathetic.) On the other hand, a good place where I would expect a GUI toolkit to appear is Boost. Unfortunately, there is no Boost.Gui at the moment and there won't be one in the foreseeable future.


I've browsed through several [publicly available libraries][8] to find one that would suit my needs. I was aware of the fact that the process would likely take a long time as the libraries are no doubt large and complicated and to evaluate their pros and cons I would have to spend a considerable amount of time on each one. My fears were further intensified by the fact that I'm not an expert in GUI development. I currently don't do GUIs at work, and in the past I've only worked with an in-house GUI toolkit (we worked on a finger-touch-enabled application for Windows CE 3.0, where custom-drawn controls are more-less a must).

Surprisingly, the task of evaluating a GUI library is not daunting at all, even for a person with limited GUI experience. With common sense and healthy appreciation for C++, I discarded half of the listed toolkits in the very first pass, just by looking at the hello-world program on their respective home pages.

The problem with these toolkits in general is that while they tout themselves as being C++ libraries, their design is often not idiomatic -- the language the libraries are written in could be more accurately described as "C with classes" rather than C++. Even worse are those libraries that are extremely large and framework-like, which tend to reinvent the wheel and in my opinion often end up with a square one.

Eventually, I was forced to declare a defeat. There seems to be no library which would satisfy all of my requirements. I've picked some of the libraries -- those that I consider to be popular since I've heard of them before -- and I'll try to explain the reasons for which I consider that particular library is unsuitable.

 * [GTK+][2] is a C toolkit based on glib, which is a library built probably to convince C developers that it is time to switch to C++ (doing object orientation in C, though technically possible, is unnecessarily verbose, and would make a C++ developer slightly nauseous). The C developers didn't quite get the message. Not only they happily use the library, they even base popular widget toolkits off of it. The only reason I'm including GTK+ in this list is that there are C++ bindings to it. However good the binding are though, GTK+ is a toolkit that was originally designed to work over X server and DirectFB. As a result, it paints its own controls, and those paintable rectangles of screen are the point of abstraction. Unfortunately the abstraction is set too low for Windows, as Windows offers a standard set of controls. Moreover, the design of the GTK+ main program loop, while very flexible, is completely incompatible with Windows. This is why you rarely see a Windows common dialog in GTK applications -- you have to run the dialog from a separate thread and it is not exactly easy to get right.
 * [Qt][3] seems to work well on Windows, which is a good sign for a framework originally designed for Linux. I have two problems with it though. First, it implements its own containers (STL containers are apparently not good enough). As if that were not enough, it requires you to run the so-called MOC preprocessor on your source files before you can pass them to a C++ compiler. Note, that the preprocessor is there only to add glue for Qt's signal/slot mechanism, a task completely unnecessary in modern C++. If Boost can do signals within the boundaries of C++, so could Qt.
 * [Ultimate++][4] is a framework (which as I explained is bad) and furthermore it is the ultimate (pardon the pun) example of how you definitely don't want to design your containers.
 * I didn't even want to include [wxWidgets][1], which, while being a popular and multiplatform framework for some reason, has managed to copy the absolutely worst qualities of ATL/WTL, requiring event maps (macros!) and other stuff which one would have hoped is now a distant and forgotten past.

What I've done some years ago is that I've built [my own GUI library][5]. This is how you would use it.

    int main()
    {
        gui::context ctx;

        gui::window wnd(ctx);
        wnd.caption("title");
        wnd.resizable(true);
        wnd.on_close(boost::bind(&gui::context::stop, &ctx, 0));
        wnd.on_resize(/*...*/);
      
        gui::button btn(wnd);

        // You'd probably do this in window's on_resize handler
        btn.position(10, 10, 100, 100);

        btn.text("click me");
        btn.show();

        wnd.show();
        return ctx.run();
    }

The code above is what I would call idiomatic; there are no macros, no unnecessary dynamic allocations, and as a bonus there are no zombie states for objects. Also note that I'm passing plain char strings, which are assumed to be utf-8 encoded.

The library is used in the client application for my [tiny AVR bootloader][6]. I maintained the application for quite some time (not anymore though, not since I moved to Linux) and thus had a chance to evaluate the design of my library. I was happy with it at first of course, only with time I've been able to observe some disadvantages as well.

 * It is annoying having to attach an on_close handler to windows just to stop the main loop from executing.
 * It is also annoying having to pass the parent window to every control you instantiate. It may not seem to be such a big deal in the program above, but once you make your main window into a class, you'll find out that you have to mention every one control in your constructor's initializer list; that is a lot of typing even when you have only one constructor.
 * You position your controls into a fixed layout, which -- while typical on Windows platforms -- is not recommended by GTK+ since it is difficult to get right with varying DPI and font sizes. I think it would be better to introduce an extensible mechanism for control positioning. The controls should be positioned automatically; having to write on_resize handler is a chore.
 * I've only implemented the toolkit for Windows, assuring myself that I can port it to Linux when necessary. Too late I've realized I was wrong and the port will never happen -- partly due to control positioning, partly due to wrong design decisions regarding paintbox control.

As my responsibilities allow, I will try to work on [a new toolkit][7], which for the lack of imagination on my part ended up being called "ggui". There are some very early sketches (this time compilable on both Linux and Windows alike) in the repository, but the library is not very useful at the moment. I'll surely blog about ggui in later posts as I think it would be beneficial to lay out the design there so as to have a reference for further development.

  [1]: http://www.wxwidgets.org/
  [2]: http://www.gtk.org/
  [3]: http://qt.nokia.com/
  [4]: http://www.ultimatepp.org/
  [5]: http://ratatanek.cz/hg/gui
  [6]: http://technika.junior.cz/~avakar/avr232boot
  [7]: http://ratatanek.cz/hg/ggui
  [8]: http://en.wikipedia.org/wiki/List_of_widget_toolkits

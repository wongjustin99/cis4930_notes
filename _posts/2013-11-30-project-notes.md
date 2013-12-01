---
layout: post 
title: "Project Notes" 
description: "" 
category: personal
tags: [project] 
--- 
{% include JB/setup %}

John's Section of the __FINAL PROJECT__

# Elastic Tab Stops

* How to arrange text and graphics into lines and columns? 
  - What objects are responsible for carrying out different formatting
    policies?
  - How do these policies interact with the document's internal
    representation?
- elastic tab stops possible solution: 
    - a `Strategy` can encapsulate formatting behaviour
      * TODO: fill in the interface of a `Strategy`
  - maybe "MonoSpace"/"Proportional" fonts have some kinda thing that
    specifies what kinda strategy to use for them? 

## Text Formatting

* compositor does this: groups of text get grouped into "columns" based
  on the number of tabs between them 
  - "rows" are created based on the user's line-wrapping
    preference

* use composite for representation 
  - rows, lines, and characters all meet this interface
    * they know how to draw themselves
    * their dimensions / coordinates
    * child management features / parent
  - each composite is called a `Glyph`
  - what this interface looks like
    - glyph photo
* Auto-indentation
  - press tab one time, and comments line up on the end use tab key to
    set up formatting Variables lining up with types
    * decorator for line tab lengths; decorator adds positive kerning
  - TODO: find a way to pass control back down to the bottom (the
    composites) such that all of the elements in a "column" are at the
same indent level
    - something that inverts control, what about something like an
      template method? like a way for each composite child to pull
something from its parent

# Font Metrics

## suppport both mono and non-monospacing
  * TODO: how to support both? what pattern used?
  - word spacing for proportional fonts & metrics handled by decorators
    * kerning: space between particular *pairs* of letters
      - add positive kerning to get rows to align to the longest one in
        a "tab level"
        - gotten by asking for the dimensions (which are appropriately
          modified by any decorator objects)
    * leading: space between baselines of text
      - is accomplished with a decorator, adding onto the height to the
        results of calls to its children
        - decorator intercepts calls to the dimensions & adds its own
          height
    * tracking: kerning between all letters, equally

* how to deal with kerning pairs? Do we just grab it from the font or
  implement it ourselves? Do we give the user that choice? 
  - allow users to choose their own kerning, which would add additional
    kerning in between glyphs using a decorator
    * kerning applied only on right side in order to keep horisontal
      alignment

Outside Events
==============

Outside events are YUI 3 synthetic DOM events that fire when a corresponding
DOM event occurs outside a bound element.

Outside events for many common native DOM events are pre-defined and ready to
use, and new outside events are a cinch to define. New outside events are named
`<event>outside` by default, but may be given any name.

Outside event handlers receive the originating DOM event as an argument.

Examples
--------

The `examples` directory contains a few working examples, but the basics are
broken down here.

### Subscribing to Outside Events

Subscribe to outside events as you would any other DOM event.

    Y.one('#dialog').on('clickoutside', function (e) {
        this.addClass('hidden');
    });

### Defining Outside Events

Outside events can be defined for any native or synthetic DOM events.

#### Native DOM Events

One [native DOM event][1] that **doesn't** have a pre-defined outside event is
`reset`. If `resetoutside` is needed, defining it is easy:

    Y.Event.defineOutside('reset');

#### Synthetic DOM Events

First define a [synthetic DOM event][2]:

    Y.Event.define('foo', function ({
        detach: function () { ... },
        on: function () { ... }
    });

Then define a corresponding outside event:

    Y.Event.defineOutside('foo');

Outside events can have custom names:

    Y.Event.defineOutside('foo', 'customfoo');

How It Works
------------

When an outside event subscription is made, the `document` subscribes to the
corresponding DOM event and waits for it to bubble. When the DOM event reaches
the `document`, the event target is compared to the outside event subscriber. If
the originating target is *outside* the subscriber, the outside event will fire.

An originating target is considered *outside* the subscriber if it is not the
subscriber itself, or any of the subscriber's ancestors.

Pre-defined Outside Events
--------------------------

The following outside events are pre-defined and ready to use:

    bluroutside       keydownoutside     mouseoutoutside
    changeoutside     keypressoutside    mouseoveroutside
    clickoutside      keyupoutside       mouseupoutside
    dblclickoutside   mousedownoutside   selectoutside
    focusoutside      mousemoveoutside   submitoutside

Caveats
-------

Outside events require DOM events to bubble up to the `document`, so there are a
few things to watch out for:

1. DOM event handlers on originating targets will run **before** the event
   reaches the `document`.

   See `examples/dialog.html` for a working example.

2. If `stopPropagation` is called on a DOM event before it reaches the
   `document`, any corresponding outside events **will not** fire.

   See `examples/dialog.html` for a working example.

3. DOM structure plays a key role in determining an element's *outsideness*.
   Pay close attention to this when using outside events.
   
   See `examples/ancestors.html` for a working example.

4. Some DOM events by definition do not (or should not) bubble, like
   `mousewheel` and `scroll`. Outside events will **never** fire for DOM events
   that do not bubble.

Known Issues
------------

IE 6, IE 7 and IE 8 all incorrectly fail to bubble the `change`, `reset`,
`select` and `submit` DOM  events, which prevents the corresponding outside 
events from working.

Acknowledgements
----------------

Outside Events was inspired by the YUI 3 [`clickoutside` example][2], and Ben
Alman's [jQuery Outside Events][4].

License
-------

Copyright (c) 2010 Brett Stimmerman <brettstimmerman@gmail.com>
All rights reserved.

Redistribution and use in source and binary forms, with or without modification,
are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this
  list of conditions and the following disclaimer.
* Redistributions in binary form must reproduce the above copyright notice, this
  list of conditions and the following disclaimer in the documentation and/or
  other materials provided with the distribution.
* Neither the name of this project nor the names of its contributors may be used
  to endorse or promote products derived from this software without specific
  prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR
ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON
ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

[1]: http://en.wikipedia.org/wiki/DOM_events
[2]: http://developer.yahoo.com/yui/3/event/#define
[3]: http://developer.yahoo.com/yui/3/event/#customevent
[4]: http://benalman.com/projects/jquery-outside-events-plugin/
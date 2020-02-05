---
title: "I forgot to clock-out"
description: "What to do when you forgot to clock-out from current task"
date: "2020-02-05"
---

I keep forgetting to clock-out from the working task when I suspend
the machine and go home. I only see this when I go home and open the
laptop. Sometimes it is few days later if I went on a vacation or had
to attend to something. Dumb way to fix this is clocking out using
`C-c C-x C-o` and editing the log entry for the clock manually. This
is what I did until recently.

_If you are wandering what all this is about, I am using the [org
mode](https://orgmode.org/manual/Clocking-Work-Time.html#Clocking-Work-Time)
to keep track of how I spend my time on certain things._

I wanted to do this more efficiently and it turns our
`org-resolve-clock` (or `C-c C-x C-z`) is the better way to do it.
Here is how I do it now.

1. Use `C-c C-x C-z`

2. Press `K` when I get the prompt. Remember that it will clock out
   only when the uppercase `K` is used.
   
   ```
   Select a Clock Resolution Command:

   i/q/C-g  Ignore this question; the same as keeping all the idle time.
   
   k/K      Keep X minutes of the idle time (default is all).  If this
            amount is less than the default, you will be clocked out
            that many minutes after the time that idling began, and then
            clocked back in at the present time.
   g/G      Indicate that you "got back" X minutes ago.  This is quite
            different from 'k': it clocks you out from the beginning of
            the idle period and clock you back in X minutes ago.
   s/S      Subtract the idle time from the current clock.  This is the
            same as keeping 0 minutes.
   C        Cancel the open timer altogether.  It will be as though you
            never clocked in.
   j/J      Jump to the current clock, to make manual adjustments.
   
   For all these options, using uppercase makes your final state
   to be CLOCKED OUT.
   ```

3. Enter the number of minutes that I want to keep when I get the
   following prompt.
   ```
   Keep how many minutes? (default 2743) 
   ```
   
That's it, I am good to start a new task now.

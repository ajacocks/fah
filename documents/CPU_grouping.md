CPU Grouping
============

At the moment, the all sockets/threads visible will be assimilated into one big CPU group.

To set these down to smaller groups (ie: to keep CPU and memory aligned ) set CPU number to the number of cores or threads per socket.

Experiment with Hyperthreading. See if it helps.

Bear in mind that each GPU will consume one entire CPU or Thread.
bear in mind that System response may be sluggish, so you may want to leave one CPU for system overhead.

Likely: Set CPU count in FAHControl GUI by picking server in left Pane, selecting Configure from top tab. 
Select CPU to review/change. 
Hit Edit
Change -1 to say... 8 or 16 or (Sum of CPU or thread -1 (for OS) and -N (for N GPU)).
Hit save.

Oh.. and .. -1 means.. take all resources automagically.

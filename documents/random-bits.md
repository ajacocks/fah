Random bits: 
 
================================ 

Join F@H support forum for great information: https://foldingathome.org/support/

================================

Check Professor Bowmens video: https://www.youtube.com/watch?v=bjYS0UKA4dE
Twitter feed: https://twitter.com/drgregbowman 
Official install guides: https://foldingathome.org/support/faq/installation-guides/

================================ 
 
Its advised to use static or DHCP/reserved IP addresses for all systems involved, unless you run internal DNS.

================================

Items to experiment with: Hyperthreading, CPU-per-slot (-1 = auto), grouping of CPUs if you more than a few.

================================

Top is your friend: 

top | grep fahclient 
...
9044  fahclie+  39  19  735740 142532  13520 R 798.0   0.1 897:15.29 FahCore_a7                                               
19322 fahclie+  39  19  751448 145624  13680 R 798.0   0.1 686:46.79 FahCore_a7                                              
20453 fahclie+  39  19  715624  71784  13524 R 797.4   0.1  59:29.34 FahCore_a7                                             
...

   Above show 3 CPU groups running at ~8 cores
   
================================

Each GPU will saturate one core/thread. or.. performance will drop.

================================

Review GPU for Points per day, Cost of card, and cost of energy. 
Review this with whoever pays the bills!

Google is your friend.. search for trends. 

https://forums.anandtech.com/threads/folding-home-rtx-2060-performance.2559663/

GPU jobs seem to be 10x as rewarding as CPU jobs. every bit helps!

================================


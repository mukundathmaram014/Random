This Idea was inspired by learning about caches in my digital logic and computer organization class. We learnt about the idea of spatial locality that basically meant that if the cache fetched a specific address from the DRAM, it was likely to also fetch nearby addresses soon so the cache brought a whole block of addresses from the DRAM into the cache. 

However, right now the cache is just fetching whatever addresses are directly adjacent to one another, but woulden't it be more optimal if the addresses that are used more are grouped together? An analogy is your keyboard, as the keys are not just listed alphabetically but are grouped together based on what is more frequently used together. Similarly, the following explores whether the DRAM dynamically shifting itself to group more frequently used addresses closer together results in a lower cache miss rate for instruction sets.

I first used a regular cache and dram, and visualized and calculated the miss rate for a small set of instructions:



https://github.com/user-attachments/assets/7754b384-5e5d-4502-9fff-1f16507f9c1d



Link to slideshow if you want to manually go through frames : [Link](https://link.excalidraw.com/p/readonly/oZOqtFrUrBwepl1FeALm).



With the regular dram for this instruction set. We see that the Miss Rate is 5/6 = 83.33%. 


Now lets add the dynamic dram functionality. It basically functions the same, except the dram dynamically reorganizes itself by the access count of its addresses (more frequent addresses at the top). Aditionally, to make sure we can still use the same address locations when we want to access data, I added a virtual-to-physical mapping table that keeps track of the swaps. The same instruction set with the dynamic dram and mapping table is shown below.



https://github.com/user-attachments/assets/dc94ba37-3f4f-4823-8605-c301e89a84b8




Link to slideshow if you want to manually go through frames : [Link](https://link.excalidraw.com/p/readonly/gLZKlq4l82Gbpm2TbiHr).


We see that for the same instruction set on this implementation with the dynamic dram, the Miss Rate is now 4/6 = 66.66%.


Hence, this design improved the cache performance by 1.25x. However, this is only shown for a small instruction set of 6 instructions. I am currently working on implementing this on verilog and integrating this with my RISC-V-Microprocessor to see if this reduces the miss rate for longer instruction sets. 

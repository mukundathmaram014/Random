This idea was inspired by learning about caches in my Digital Logic and Computer Organization class. We studied the concept of spatial locality, which means that if the cache fetches a particular address from DRAM, it’s likely to access nearby addresses soon after. To take advantage of this, the cache fetches not just one address but an entire block of consecutive addresses.

Currently, the cache simply fetches addresses that are physically adjacent to one another. But wouldn’t it be more efficient if addresses that are frequently used together were also stored close together? A good analogy is a keyboard: the keys aren’t arranged alphabetically, but instead grouped based on which ones are often used together to improve typing efficiency.

Similarly, this project explores whether dynamically reorganizing DRAM so that frequently accessed addresses are grouped together can lead to a lower cache miss rate for instruction sets.

I began by simulating a standard cache–DRAM setup and visualizing the resulting miss rate for a small set of instructions:



https://github.com/user-attachments/assets/7754b384-5e5d-4502-9fff-1f16507f9c1d



Link to slideshow to manually go through frames : [Link](https://link.excalidraw.com/p/readonly/oZOqtFrUrBwepl1FeALm).



Using the regular DRAM with this instruction set, we see the cache has a miss rate of 5/6 = 83.33%.

Now, let’s introduce the dynamic DRAM functionality. It operates similarly to the regular version, except the DRAM reorganizes itself dynamically based on the access frequency of its addresses, moving the more frequently accessed ones toward the top. It is basically resorting itself based on the access count of each of its address.

To ensure that data can still be accessed through the same virtual addresses from the processor's side, I implemented a virtual-to-physical mapping table that tracks these address swaps in real time.

The same instruction set, now running with the dynamic DRAM and mapping table, is shown below:



https://github.com/user-attachments/assets/dc94ba37-3f4f-4823-8605-c301e89a84b8




Link to slideshow to manually go through frames : [Link](https://link.excalidraw.com/p/readonly/gLZKlq4l82Gbpm2TbiHr).


With the dynamic DRAM implementation, the same instruction set now yields a miss rate of 4/6 = 66.66%.

This represents a 1.25× improvement in cache performance compared to the regular DRAM design. 

However, this result is based on a small instruction set of only six instructions. I’m currently working on implementing this design in Verilog and integrating it with my RISC-V microprocessor to evaluate whether the improvement in cache performance holds for larger and more complex instruction sets.

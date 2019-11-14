# Performance FAQ



#### Which tool do I choose to profile?

Depends on what you want to achieve, and how deep you need to extract information. Most of the time, you will need basic tools first to pinpoint a performance problem : your engine comes with many tools to display timing information, sort and filter these values so you can have a clearer view of what is happening.

If these tools are not sufficient, you can start digging up using more external tools such as Intel GPA, RenderDoc, or more dedicated tools if you develop on console platforms such as Playstation 4 or Xbox One.



#### I cannot figure out a bottleneck, my timings seem right.

There are some times where the sum of all timings does not equal the measured frame time. This can be due to a problem of latency : in layman terms, stuff waiting for other stuff to finish before starting. As many engines tend to do multi-threading, some processing can be executed in parallel in two or more threads in order to save time. 

Most of the time, while doing multithreading, the main goal is to have everything running in parallel without  having anything waiting for other to finish, but this is only the theory. 



#### I profiled but I do not have any millisecond left for my effects.

That's too bad for you.

... More seriously, we can get away with a global understanding of the problem and which job overlapped with our precious millisecond budget. Most of the time, the CTO will come at the culprits and try to figure out with them a solution, but in the end we will be the ones to come at them to discuss things.


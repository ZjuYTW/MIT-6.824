<h1>Conclusion

After finish 3 parts of Lab, I have implemented a basic raft protocol and it can used to build a  K-V storage server. It is fascinating for its understandability and straight idea to build this consensus system. In past 3 parts, I achieved timeout election, append entries, persist and many subtle details to ensure the consistency. My implementation may seem somehow ungraceful especially when I looked the tutorial course 'Go thread and Raft', so I do recommend you to have a look at it before you start your first line of code.



<h2>TODO

I still have some issues need to be addressed, some are performance related and some are many bugs(I don't know for sure, for me, I'd rather blame these FAIL to too strict test time restriction...)

* Some time, many 1 out of 10 the test will fail, for the program can not reach agreement. And the reason is easy, timeout election can sometime spend much time to elect a leader. And followings are solutions, from my point.
  * Check if two peer are racing like, *one elected in Term x but in Term x + 1, another timeout and start a new round election*. For this situation, consider if timer triggers before you reset it
  * Sleep time is a mysterious number I've changed them for many times but still have some problem... Currently a fair sleep time is [200, 400] for heartbeat timeout and [50, 100] for election timeout 
  * Some threads' synchronization is implemented heavily so need a more soft way to achieve it(As I will describe afterwards)
* Some terrible code need to refactoring like substitute all channel by using condition variable and merge analogous code into one function, etc
* To be update...
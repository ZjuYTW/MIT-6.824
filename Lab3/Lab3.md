<h1> Lab3 </h1>

<h2>Overview</h2>

In this Lab, we are gonna build a Key/Value storage system based on raft, AKA KVRaft. In part A, just maintain a in memory Hashmap of Key -> Value and In part B, we need to deal with the growing Hashmap and use Snapshot to discard old log entries. Some details may be discussed in each section.



<h2>Lab3A</h2>

To implement a KVRaft, we use *clerk* to denote a interface the client calls to use K/V storage, *server* to denote  a peer among  K/V storage system which is based on Raft protocol. 

Particularly, we need to address of some detailed problems

* Leader find
  * Whether make server report to clerk which is leader(Not applicable for this lab)
* Repeat request
  * Like, send *append* request to a raft while due to the unreliable RPC, before receiving a commit server receives a "timeout". So when you are trying append secondly, you should  be careful not to append twice.
* Be careful of deadlock



<h3>Implementation</h3>

* To find leader, clerk just use randomized generate server_id to query for true leader, and once get positive response *clerk* will record this id for later use. 

  Furthermore, I want to explain why server not send a leader_id to clerk. In this lab3a,  the server list every clerk maintains are different as I *Dprint(ck.leader)* shows, which means there are not a standard leader_id for all clerk

* Generate a random id for each clerk and mark each operation a monotonic increasing sequence number to erase repeat.

  The erase repeating step should be processed in *server* level, not in *raft* level. 

  For these out-dated operation, because the sequence number for each clerk is monotonically increasing, server just skip manipulating the data but instead return positive



![Test3A](..//image//Test3A.png)
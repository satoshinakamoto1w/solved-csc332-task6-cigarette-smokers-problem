Download Link: https://assignmentchef.com/product/solved-csc332-task6-cigarette-smokers-problem
<br>
Consider a system with 3 smoker processes and 1 agent process. Each smoker continuously rolls a cigarette and then smokes it. The smoker needs three ingredients: tobacco, paper, and matches. One of the smokers has paper, another has tobacco, and the third has matches. The agent has an infinite supply of all three materials and (randomly) places two of the ingredients on the table each time. The smoker who has the remaining ingredient then makes and smokes a cigarette, signaling the agent on completion. The agent then puts out another two of the three ingredients, and the cycle repeats.

<strong>TO DO: </strong>Write a program to synchronize the agent and smoker processes using the ​         <strong>pthread library and</strong>​        <strong> pthread_mutex. </strong>

<strong>Instructions</strong>​

<ul>

 <li>​Although the problem description gives the agent an infinite supply of ingredients, you may set a</li>

</ul>

reasonable finite number of iterations.

<ul>

 <li>​You need to include an explanation of how your code succeeds in synchronizing the agent and the smokers. Provide output showing the sequence of events for several iterations.</li>

 <li>​See the pseudocode below.</li>

</ul>

Zip your source &amp; explanation text files into a single folder as: task6_<em>lastname</em>​     ​.zip. Email your zip file with the subject line, “Task 6 – CSc332G- <em>lastname</em>​         ​”.

<strong>Cigarette Smoker’s Problem as presented by Prof. Jeffrey Hollingsworth, University of Maryland  </strong>

<h1>Problem</h1>

There are four processes in this problem: three smoker processes and an agent process. Each of the smoker processes will make a cigarette and smoke it. To make a cigarette requires tobacco, paper, and matches. Each smoker process has one of the three items. I.e., one process has tobacco, another has paper, and a third has matches. The agent has an infinite supply of all three. The agent places two of the three items on the table, and the smoker that has the third item makes the cigarette. Synchronize the processes.




Please note this is the formulation of the problem used in recent OS texbooks, but not the problem as originally described by Parnas in CACM March 1975.

<h1>Solution</h1>

This seems like a fairly easy solution. The three smoker processes will make a cigarette and smoke it. If they can’t make a cigarette, then they will go to sleep. The agent process will place two items on the table, and wake up the appropriate smoker, and then go to sleep. All semaphores except lock are initialized to 0. lock is initialized to 1, and is a mutex variable.

Here’s the code for the agent process.

1 ​  do forever {

​2​     P( lock );

​3​     randNum = rand( ​1​, ​3​ ); // Pick a random number from ​1​-​3

​4​     ​if​ ( randNum == ​1​ ) {

​5​        // ​Put​ tobacco ​on table​

​6​        // ​Put​ paper ​on​ table

​7​        V( smoker_match );  // Wake up smoker with match

​8​      } ​else​ ​if​ ( randNum == ​2​ ) {

​9​        // ​Put​ tobacco ​on table​

<ul>

 <li>​ // ​Put​ match ​on​ table</li>

 <li>​ V( smoker_paper );  // Wake up smoker with paper</li>

 <li>​ } ​else​ {</li>

 <li>​ // ​Put​ match ​on​ table</li>

 <li>​ // ​Put​ paper ​on​ table</li>

 <li>​ V( smoker_tobacco ); } // Wake up smoker with tobacco</li>

 <li>​ V( lock );</li>

 <li>​ P( agent );  //  Agent sleeps</li>

 <li>​ }  // ​end​ forever loop</li>

</ul>




I will give code to one of the smokers. The others are analogous.




1 ​  do forever {

​2​      P( smoker_tobacco );  // Sleep right away

​3​      P( lock );

​4​      // Pick up match

​5​      // Pick up paper

​6​      V( agent );

​7​      V( lock );

​8​      // Smoke (but don​’t inhale).

​9​   }




The smoker immediately sleeps. When the agent puts the two items on the table, then the agent will wake up the appropriate smoker. The smoker will then grab the items, and wake the agent. While the smoker is smoking, the agent can place two items on the table, and wake a different smoker (if the items placed aren’t the same). The agent sleeps immediately after placing the items out. This is something like the producer-consumer problem except the producer can only produce 1 item (although a choice of 3 kinds of items) at a time.



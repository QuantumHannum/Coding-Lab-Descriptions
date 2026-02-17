# Part 3: Single SAT Problem
When studying the basic quantum algorithms (Deutsch-Jozsa, Simon, Grover, etc) they all start to look a bit contrived.  They all make use of oracles, but to generate the oracle you need to "know the answer" already.  In the case of Deutsch-Jozsa - is the function constant or balanced, with Simon's - you need to know the secret key, and as we have seen with Grover's you need to know the state you are looking for, and then find it.  Its time to look at an example of Grover's algorithm where we use an oracle to confirm (or discover) a solution we don't know before we start the problem.

---
## Activity 3: Satisfiablity Problems with Grover's


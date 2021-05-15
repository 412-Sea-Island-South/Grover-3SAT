🦈 Solving 3SAT Problems using Grover's Algorithm!🦈
=====================================================
What is the 3SAT Problem?
-------------------------
We will explain the 3SAT problem with an example to help with the understanding.
It's actually a quite simple problem.
It involves a [boolean](https://en.wikipedia.org/wiki/Boolean) function, *f*, with three boolean variables (the same kinds of variables in algebra).
We will call those variables var1, var2, and var3.
Suppose *f* is the function:  
<img src="https://render.githubusercontent.com/render/math?math=\color{white}f(v1, v2, v3)=(\neg v1 \lor \neg v2 \lor \neg v3) \land (v1 \lor v2 \lor v3) \land (\neg v1 \lor v2 \lor v3) \land (v1 \lor \neg v2 \lor v3) \land (v1 \lor v2 \lor \neg v3)" align="center">
  
This boolean function is comprised of 5 clauses, which start with '(' and end with ')'.
In general, there can be *N* clauses.
In a k-SAT problem, each clause has k clauses.
Since k=3 in this case, each clause has 3 literals.
A literal is defined as a variable possibly with the negation symbol in front.
For example, in the first clause, the literals are:
<img src="https://render.githubusercontent.com/render/math?math=\color{white}(\neg v1, \neg v2, \neg v3)">.
The <img src="https://render.githubusercontent.com/render/math?math=\color{white}\land"> and
<img src="https://render.githubusercontent.com/render/math?math=\color{white}\lor"> symbols are the boolean and and or symbols.
*F* is satisfiable if an assignment of the literals to the boolean values causes *f* to evaluate to true.

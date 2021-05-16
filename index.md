🦈 Solving 3SAT Problems using Grover's Algorithm!🦈
=====================================================
## What is the 3SAT Problem?
We will explain the 3SAT problem with an example to help with the understanding.
It's actually a quite simple problem.
It involves a [boolean](https://en.wikipedia.org/wiki/Boolean) function, *f*, with three boolean variables (the same kinds of variables in algebra).
We will call those variables var1, var2, and var3.
Suppose *f* is the function:  
<img src="https://render.githubusercontent.com/render/math?math=\color{white}f(v1, v2, v3)=(\neg v1 \lor \neg v2 \lor \neg v3) \land (v1 \lor v2 \lor v3) \land (\neg v1 \lor v2 \lor v3) \land (v1 \lor \neg v2 \lor v3) \land (v1 \lor v2 \lor \neg v3)" >
  
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

## An Obvious Solution
Of course, the easiest way is to just try every single possible value for each of the variables.
This will run in time <img src="https://render.githubusercontent.com/render/math?math=\color{white}2^n">.
Suitable for small values of *n*, but for huge values, tough luck.
In our case,
<img src="https://render.githubusercontent.com/render/math?math=\color{white}n=3">
so
<img src="https://render.githubusercontent.com/render/math?math=\color{white}2^n=8">
and this will run almost instantly.
This means the brute-force solution is viable.
And indeed, after trying everythingm we find that there are not one, not two, but *three* solutions.
  
### Brute-Force Code
```python
def func(v1, v2, v3):
    c1 = (not v1) or (not v2) or (not v3)
    c2 = v1 or (not v2) or v3
    c3 = v1 or v2 or (not v3)
    c4 = v1 or (not v2) or (not v3)
    c5 = (not v1) or v2 or v3
    return c1 and c2 and c3 and c4 and c5
    
found = False
for v1 in range(2):
    for v2 in range(2):
        for v3 in range(2):
            if func(v1, v2, v3):
                print("A satisfying assignment was found;", end = " ")
                print("it is:", end = " ")
                print("v1 =",v1, ", v2 =",v2, ", v3 =",v3)
                found = True
if (not found):
    print("No Satisfying assignments found.", end = "\n")
```
### Brute-Force Solutions
<table>
  <tr>
    <th>V1</th>
    <th>V2</th>
    <th>V3</th>
    <th>Is Solution?</th>
  </tr>
  <tr>
    <td>False</td>
    <td>False</td>
    <td>False</td>
    <td>Solution</td>
  </tr>
  <tr>
    <td>False</td>
    <td>False</td>
    <td>True</td>
    <td>Not a Solution</td>
  </tr>
  <tr>
    <td>False</td>
    <td>True</td>
    <td>False</td>
    <td>Not a Solution</td>
  </tr>
  <tr>
    <td>False</td>
    <td>True</td>
    <td>True</td>
    <td>Not a Solution</td>
  </tr>
  <tr>
    <td>True</td>
    <td>False</td>
    <td>False</td>
    <td>Not A Solution</td>
  </tr>
  <tr>
    <td>True</td>
    <td>False</td>
    <td>True</td>
    <td>Solution</td>
  </tr>
  <tr>
    <td>True</td>
    <td>True</td>
    <td>False</td>
    <td>Solution</td>
  </tr>
  <tr>
    <td>True</td>
    <td>True</td>
    <td>True</td>
    <td>Not a Solution</td>
  </tr>
</table>

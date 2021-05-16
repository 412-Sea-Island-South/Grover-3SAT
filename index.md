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
And indeed, after trying everything we find that there are not one, not two, but *three* solutions.
  
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
<table align="center">
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
  
## A Quantum Solution
We will use Qiskit Aqua (a "submodule" of Qiskit), a Python-based programming language created by IBM for programming quantumc computers.
The problems requires a special format, called DIMACS CNF.
You can read about it [here](http://www.satcompetition.org/2009/format-benchmarks2009.html).
  
We will create the oracle for the Grover's Search algorithm, which is the LogicalExpressionOracle component which is helpfully already provided in Qiskit Aqua. It also supports DIMACS CNF format strings and construction of the oracle circuit.
The quantum solution is just the brute-force solution but sped up quadratically by Grover's Algorithm.
  
### Understanding DIMACS CNF
```
c example DIMACS CNF 3-SAT
p cnf 3 5
-1 -2 -3 0
1 -2 3 0
1 2 -3 0
1 -2 -3 0
-1 2 3 0
```
- lines that start with c are comments (c is for comments)
- the first line that's not a comment needs to be ```p cnf number_of_variables number_of_clauses```. The cnf meanss cnf format.
- each line after that is a clause, which consists of number_of_variables values, which must be integers and not null. The line ends with 0, and the literals ```literal``` and ```-literal```. Negative integers are the negation of the variable.
- For example, a solution can be written as <img src="https://render.githubusercontent.com/render/math?math=\color{white}(\neg v1 \lor \neg v2 \lor \neg v3)">
  
### Quantum Code
```python
import numpy as np
from qiskit import *
from qiskit.visualization import *
from qiskit.aqua import *
from qiskit.aqua.algorithms *
from qiskit.aqua.components.oracles import *

input_3sat = '''
c example DIMACS-CNF 3-SAT
p cnf 3 5
-1 -2 -3 0
1 -2 3 0
1 2 -3 0
1 -2 -3 0
-1 2 3 0
'''

oracle = LogicalExpressionOracle(input_3sat)
grover = Grover(oracle)

backend = BasicAer.get_backend('qasm_simulator')
quantum_instance = QuantumInstance(backend, shots=1024)
result = grover.run(quantum_instance)
print(result['assignment'])
plot_histogram(result['measurement'])
```
  
## Project Gallery Showcase
<h4 align="center">3SAT Grover's Solutions</h4>
<p align="center">
  <img src="https://user-images.githubusercontent.com/81530826/118385477-fd6d9700-b5c3-11eb-8d90-ca2e2b3cb788.png">
</p>
  
## About Me
Hello there! I'm Max, an innovator at [The Knowledge Society aka TKS](https://tks.world).
I'm interested in a lot of different emerging technologies, but my main interests right now are Quantum Computing ⚛️, Blockchain 💵, Artificial Intelligence 🤖, and last but not least, Space Tech/Exploration 🚀.
So yeah, that's basically who I am. You can check out my social media profiles below.  
<p>
  <a href="https://www.linkedin.com/in/max-cui-9889641b7/" rel="nofollow noreferrer">
    <img src = "https://i.stack.imgur.com/gVE0j.png" alt="linkedin">
    LinkedIn
  </a> &nbsp;
  <a href = "https://github.com/TKSMax" rel="nofollow noreferrer">
    <img src = "https://i.stack.imgur.com/tskMh.png" alt="github">
    GitHub
  </a> &nbsp;
  <a href="https://max-c.medium.com" rel="nofollow noreferrer">
    Medium
  </a> &nbsp;
  <a href = "https://maxmcui.substack.com" rel="nofollow noreferrer">
    Sub to my Newsletter
  </a>
  <a href="https://tksmax.github.io/FocusProjects" rel="nofollow noreferrer">
    Back to Main Page
  </a>
</p>

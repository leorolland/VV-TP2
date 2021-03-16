# TCC *vs* LCC

Explain under which circumstances *Tight Class Cohesion* (TCC) and *Loose Class Cohesion* (LCC) metrics produce the same value for a given Java class. Build an example of such as class and include the code below or find one example in an open-source project from Github and include the link to the class below. Could LCC be lower than TCC for any given class? Explain.

## Answer


According to https://www.aivosto.com/project/help/pm-oo-cohesion.html


> Cohesion metrics measure how well the methods of a class are related to each other. A cohesive class performs one function. A non-cohesive class performs two or more unrelated functions. A non-cohesive class may need to be restructured into two or more smaller classes.

> The metrics TCC (Tight Class Cohesion) and LCC (Loose Class Cohesion) provide another way to measure the cohesion of a class. The TCC and LCC metrics are closely related to the idea of LCOM4, even though are are some differences. **The higher TCC and LCC, the more cohesive and thus better the class.**

Definition :

NP = maximum number of possible connections

= N * (N − 1) / 2 where N is the number of methods

NDC = number of direct connections (number of edges in the connection graph)

NID = number of indirect connections

Tight class cohesion TCC = NDC / NP

Loose class cohesion LCC = (NDC + NIC) / NP

### Answer 
TCC = LCC => NIC = 0 => 0 indirect connections

Example : When 2 methods are not directly connected, but they are connected via other methods, we call them indirectly connected. Example: A - B - C are direct connections. A is indirectly connected to C (via B).

```
class A {
	method foo() {
		B().foo()
	}
}
class B {
	method foo() {
		C().foo()
	}
}
class C {
	method foo() {
		...
	}
}


```

# **MEDS**
## *Digital design and Computer Architecture*
### ***Lecture # 2: Combinational Logic***

We've now learned that **CMOS technology can be used to make logic gates*, such as the CMOS NOT, CMOS NAND, CMOS AND and more. *Inputs are passed through a combination of pMOS pull-up networks and nMOS pull-down networks.*

**General form to constructing any inverting logic gate:**
- Networks may consist of transistors in series or in parallel
- In parallel, the network is ON if one transistor is ON
- In series, the network is ON if all transistors are ON

**Exactly one network should be ON at any given time, with the other being OFF.</br>**
If both are ON --> Short circuit</br>
If both are OFF --> Floating

**Why?**</br>
pMOS pass 1's (holes carry charge) --> Good at pulling up the output</br>
nMOS pass 0's (electrons carry charge) --> Good at pulling down the output

**Latency:**</br>
Series connections are slower than parallel, more wire resistance

**Power:**</br>
Dynamic power consumption:
- Used to change capacitor as signals change 0 --> 1
- C * V * f^2

Static power consumption:
- Power used when signals don't change
- V * I (leakage)

**Energy:**</br>
Energy consumption: P * t

**Moore's Law:**</br>
Number of transistors on integrated circuits doubles every 2 years.</br>
*How do we keep this?*
- Manufacturing smaller transistors
- Better properties materials
- Enabling precision manufacturing
- Creating new device technologies

Now that we understand workings of vasic logic gates, we can build some of the logic structures that are important components of computer architecture.

**Logic circuits:**
> Input --> Func spec & Time spec --> Output

Functional specification describes relationship between I/O</br>
Timing specification describes the delay between Input changing and Output responding

**Combinational Logic:**</br>
Memoryless. Outputs are strictly dependant on the conditions of input values that are being applied to circuit right now.

**Boolean Logic Equations:**</br>
Functional specification of output in terms of inputs.

'Function' uniquely maps inputs to output and has no memory, for same sets of input we are guaranteed to have the same output. For example a full 1 bit adder.

*Simple Equations:*
1. NOT: A' (~ A) 1 iff A = 0
2. AND: AB (A & B) 1 iff A = 1, B = 1
3. OR: A + B (A | B) 1 iff A = 1/B = 1

**Boolean Algebara:**</br>
Algebra on 1's and 0's. Start with axioms, derive laws and theorems first which are simplified.

*Axioms:*
1. B contains at least two elements: 0 and 1.
2. Closure: a, b belong to B:
    - a + b belong to B
    - ab belong to B
3. Commutative Laws:
    - a + b = b + a
    - ab = ba
4. Identities:
    - a + 0 = a
    - a * 1 = a
5. Distributive Laws:
    - a + (bc) = (a + b) * (a + c)
    - a * (b + a) = ab + ac
6. Complement:
    - a + a' = 1
    - aa' = 0


*Useful Laws:*
1. Idempotent:
    - x + 0 = x | x * 0 = x
    - x + 1 = 1 | x * 1 = 0
2. Involution:
    - (x')' = x
3. Laws of complemantarity:
    - x + x' = 1 | x * x' = 0
4. Commutative:
    - x + y = y + x | x * y = y  * x
5. Associative:
    - (x + y) + z = x + (y + z) | (x * y) * z = x * (y * z)
6. Distributive:
    - x * (y + z) = (x + y) * (x + z) | x + (y * z) = (x * y) + (x * z)
7. Simplifying:
    - (x * y) + (x * y') = x | (x + y) * (x + y') = x
    - x + (x * y) = x | x * (x + y) = x
    - (x + y') * y = x * y | (x * y') + y = x + y

Duality: Replacing all ANDs with ORs and all 1's with 0's still keeps all of the above valid.

*DeMorgan's Laws:*</br>
Sum of complements and complements of sum.
1. (x + y + z ...)' =  x' * y' * z' ...
2. (x * y * z ...)' = x' + y' + z' ...

*Sum of Products (SOP) Form:*</br>
F = OR for all input combinations that give output 1</br>
aka Minterm Expansion

*Products of Sum (POS) Form:*</br>
F = AND for all input combinations that give output 0</br>
aka Maxterm Expansion

**Combinational Building Blocks:**</br>
1. Decoder:
    - 'Input pattern decoder'
    - N inputs 2*N outputs
    - Exactly 1 output is 1 at a specific input pattern
2. Multiplexer:
    - A MUX selects one of the N inputs to connect to the output
    - Output C for instance is connected to inputs A & B depending on select line S
    - Used on lookup tables to perform logic functions
3. Full Adder:
    - Performs binary addition
    - A 4-bit adder can be created using 1 bit adders
4. Equality checker:
    - Checks if two N input values are exactly the same.
5. Less than checker:
    - Checks if one of the N input values is less than the other.
6. Arithmetic & Logic Unit:
    - Combines a bunch of arithmetic and logic operations into a single unit performing one function at a time
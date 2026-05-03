# **MEDS**
## *Digital design and Computer Architecture*
### ***Lecture # 5 (b) and 6: Timing and Verification***

**Circuit Designing is a tradeoff between various features:**
1. Area:
    - Circuit area is proportional to the cost of the device
2. Speed/Throughput:
    - We want faster, more capable circuits
3. Power/Energy:
    - Mobile devices need to work with a limited power supply and high performance devices dissipate loads of power
4. Design Time:
    - Expensive in time and money

*Remember:* Requirements and Goals Depend On Application

**Combinational Circuit Timing:**</br>

*Digital Logic Abstraction:* Output changes immediately with the input</br>
*Combination Logic delay:* In reality, outputs are delayed from inputs Transistors take a finite amount of time to switch

*Delay is caused by:*
- Capacitance and resistance in a circuit
- Finite speed of light

*Quantities that can change delay:*
- Rising (i.e., 0 -> 1) vs. falling (i.e., 1 -> 0) inputs
- Different inputs have different delays
- Changes in environment (e.g., temperature)
- Aging of the circuit

*Types of Delays:*
1. Contamination delay (tcd): delay until Y starts changing
2. Propagation delay (tpd): delay until Y finishes changing

*Delay Paths:*
1. Critical (Longest) Path: tpd = 2 tpd_AND + tpd_OR
2. Shortest Path: tcd = tcd_AND

*Remember:* Designers assume “worst-case” conditions and run many
statistical simulations to balance yield/performances because not all input transistions affect the output and there can be multiple different paths from inputs to output. Delays also vary gate to gate, have additional wire delays and temperature/voltage can change the entire path.

**Output Glitches:**</br>

*Glitch:* one input transition causes multiple output transitions. Glitches are visible in K-maps.

Fixing glitches is undesirable:
- More chip area
- More power consumption
- More design effort
- The circuit is eventually guaranteed to converge to the right
value regardless of glitchiness

Therefore If we only care about the long-term steady state output,
we can safely ignore glitches.

**Sequential Circuit Timing:**</br>

*D flip-flop Input Timing Constraint:*</br>

D must be stable when sampled at active clock edge.

1. Setup time (tsetup): time before the clock edge that data must be stable (i.e. not changing)
2. Hold time (thold): time after the clock edge that data must be stable
3. Aperture time (ta): time around clock edge that data must be stable (ta = tsetup + thold)

If D is changing when sampled, metastability can occur

*Flip-Flop Output Timing:*</br>
1. Contamination delay clock-to-q (tccq): earliest time after the clock edge that Q starts to change (i.e., is unstable)
2. Propagation delay clock-to-q (tpcq): latest time after the clock edge that Q stops changing (i.e., is stable)

*Combinational Logic:*</br>

Multiple flip-flops are connected with combinational logic, Clock runs with period Tc (cycle time). Must meet timing requirements for both flip-flops (R1 & R2).

- Need to ensure correct input timing on 2nd flip-flop. It must be stable at least tsetup before the clock edge and at least until thold after the clock edge.
- This means there is both a minimum and maximum delay between two flip-flops. CL too fast -> R2 thold violation, CL too slow -> R2 tsetup violation
- Safe timing depends on the maximum delay from R1 to R2.
- *Sequencing overhead:* Amount of time wasted each cycle due to sequencing element timing requirements
- Critical path: path with the longest tpd
- Overall design performance is determined by the critical path tpd
    - Determines the minimum clock period (i.e., max operating frequency)
    - If the critical path is too long, the design will run slowly
    - If critical path is too short, each cycle will do very little useful work i.e., most of the cycle will be wasted in sequencing overhead
    
    *Setup time constraint:*</br>
   
    ~~~
    Tc > tpcq + tpd + tsetup
    ~~~

    *Hold time constraint:*<br>
   
    ~~~
    tcd > thold - tccq
    ~~~

*Clock Skew:*</br>


Time difference between two clock edges. The clock does not reach all parts of the chip at the same time

Safe timing requires considering the worst-case skew:

*Setup time constraint:*
- Clock arrives at R2 before R1
- Leaves as little time as possible for the combinational logic</br>

    This effectively increases tsetup:

    ~~~
    Tc > tpcq + tpd + tsetup + tskew

    Tc > tpcq + tpd + tsetup, effective
    ~~~

*Hold time constraint:*
- Clock arrives at R2 after R1
- Increases the minimum required delay for the combinational logic

    This effectively increases thold:

    ~~~
    tcd + tccq > thold + tskew

    tcd + tccq > thold, effective
    ~~~

**Circuit Verification:**</br>

Once you've designed a circuit, you need to test it for functionality and timing.</br>
For this we use simulation tools.

Testing can be the most time consuming design stage. It requires testing for functional correctness of all logic paths and timing, power, etc. of all circuit elements.

*Functional Verification:*</br>

Our goal is to check the logical correctness of the design.

Testbench: a module created specifically to test a design. Tested design is called the “device under test (DUT)”</br>
Testbench provides inputs (test patterns) to the DUT and checks outputs of the DUT against hand-crafted values.

A testbench can be:
- HDL code written to test other HDL modules
- Circuit schematic used to test other circuit designs

~~~
module example_syntax;

  logic a;

  initial begin
    a = 0;
    #10;
    a = 1;
    $display("printf() style message!");
  end

endmodule
~~~

Testing Output: Most common method is to look at waveform diagrams. This requires manually checking that the output is correct at all times. You can also do self-checking testbenches with testvectors.

*Timing Verification:*</br>

Theres two approaches:
1. High-level simulation
    - Can model timing using “#x” statements in the DUT
    - Useful for hierarchical modeling
2. Circuit-level timing verification
    - Need to first synthesize your design to actual circuits
    - Better accuracy

Usually tools provide a ‘timing report’ or ‘timing summary’ containing:
- Worst-case delay paths
- Maximum operation frequency
- Any timing errors that were found

However there can be issues with tools therefore this is often a manual, iterative process.
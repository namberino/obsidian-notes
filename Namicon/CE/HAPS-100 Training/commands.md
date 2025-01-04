load database:
`database set_state {m0}`

When you synthesize the design, the tool generates the compiled or synthesized netlist (.vm). Different stages generate different files for simulation

export the files needed for simulation:
`export netlist -path directoryPath`

If running simulation on a mapped design, instantiate the `hapsinit.sim` module in the testbench. Make sure to connect `clk_0` to the inverted clock net. For example: Required testbench changes:
```verilog
dut dut_inst (  
.clk (clk_net),

.reset (reset),  
.clk_0 (~clk_net), ........);

hapsinit_sim hapsinit_sim_uut ();
```

This module is only generated after mapping. It is not required for simulation at earlier stages of the design

Launch VCS software and run simulation:
- This first step is optional. If you have a mixed language design or need to analyze libraries, use the `vhdlan` or `vlogan` commands to run analysis
- If you are simulating a mapped design, make sure to specify the `design_hapsinit.vm` file as input to the `vcs` command. Specify it after the model file. For example:
```
vcs design.vm hapsinit.vm testbench libraries
```
You do not need this file to run simulation after compile.
# FPGA Learning Note
## _10132024, Wei He_
[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)


## Topic
- basic execution order in testbench


## Switch & LED demo
Below is a simple testbench implementation from a book, to show the stimulus and checking of the testing.
```sh
timescale 1ns/ 100ps;

module tb;
  logic [1:0] SW;
  logic [3:0] LED;
  logic_ex u_logic_ex (.*);

  // Stimulus
  initial begin
    $printtimescale(tb);
    SW = 2`b00;
    for (int i = 0; i < 4; i++) begin
      $display("Setting switches to %2b", i[1:0]);
      SW  = i[1:0];
      #100; //delay by 100 ns
    end
    $display("PASS: logic_ex test PASSED!");
    $stop;
  end

  // Checking
  always @(SW, LED) begin
    if (!SW[0] !== LED[0]) begin
      $display("FAIL: NOT Gate mismatch");
      $stop;
    end
    if (&SW[1:0] !== LED[1]) begin
      $display("FAIL: AND Gate mismatch");
      $stop;
    end
    if (|SW[1:0] !== LED[2]) begin
      $display("FAIL: OR Gate mismatch");
      $stop;
    end
    if (^SW[1:0] !== LED[3]) begin
      $display("FAIL: XOR Gate mismatch");
      $stop;
    end
  end
endmodule // tb
```

Interaction between initial and always:

At time 0: The initial block runs first and sets the initial value of SW. It runs sequentially and pauses if it encounters delays (e.g., #100).

Always block triggers: Once SW is assigned a value (or any time SW or LED changes), the always @(SW, LED) block will trigger and execute its code.

Looping in initial block: After the #100 delay, the initial block assigns the next value to SW. This again triggers the always block, which checks the output and reacts if there are mismatches.

Final message: Once the initial block has finished assigning values to SW and all delays have passed, it will print the final "PASS: logic_ex test PASSED!" message if no errors were encountered. The simulation will stop after $stop.





##
##
##
## License: MIT

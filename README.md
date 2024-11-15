# 4-bit-Ripple-Carry-Adder-using-Task-and-4-bit-Ripple-Counter-using-Function-with-Testbench
Aim:
To design and simulate a 4-bit Ripple Carry Adder using Verilog HDL with a task to implement the full adder functionality and verify its output using a testbench.
To design and simulate a 4-bit Ripple Counter using Verilog HDL with a function to calculate the next state and verify its functionality using a testbench.

Apparatus Required:
Computer with Vivado or any Verilog simulation software.
Verilog HDL compiler.

// Verilog Code
module RippleCarryAdder_4bit_tb;
module RippleCarryAdder_4bit (
    input [3:0] a,        // 4-bit input A
    input [3:0] b,        // 4-bit input B
    input cin,            // Carry input
    output [3:0] sum,     // 4-bit sum output
    output cout           // Carry output
);

    reg [3:0] sum_reg;
    reg carry_out;
    
    // Output assignment
    assign sum = sum_reg;
    assign cout = carry_out;

    // Task to perform full adder operation for one bit
    task full_adder;
        input a_bit, b_bit, carry_in;
        output sum_bit, carry_out_bit;
        
        begin
            sum_bit = a_bit ^ b_bit ^ carry_in;
            carry_out_bit = (a_bit & b_bit) | (carry_in & (a_bit ^ b_bit));
        end
    endtask

    // Intermediate signals for carry between stages
    reg c1, c2, c3;

    always @(*) begin
        // Bit 0
        full_adder(a[0], b[0], cin, sum_reg[0], c1);
        
        // Bit 1
        full_adder(a[1], b[1], c1, sum_reg[1], c2);
        
        // Bit 2
        full_adder(a[2], b[2], c2, sum_reg[2], c3);
        
        // Bit 3
        full_adder(a[3], b[3], c3, sum_reg[3], carry_out);
    end

endmodule

 

// Test bench for Ripple carry adder
module RippleCarryAdder_4bit_tb;

    reg [3:0] a, b;
    reg cin;
    wire [3:0] sum;
    wire cout;

    RippleCarryAdder_4bit uut (
        .a(a),
        .b(b),
        .cin(cin),
        .sum(sum),
        .cout(cout)
    );

    initial begin
        // Test case 1
        a = 4'b0001; b = 4'b0010; cin = 1'b0;
        #10;
        
        // Test case 2
        a = 4'b0101; b = 4'b0011; cin = 1'b1;
        #10;
        
        // Test case 3
        a = 4'b1111; b = 4'b1111; cin = 1'b1;
        #10;

        // Test case 4
        a = 4'b1010; b = 4'b0101; cin = 1'b0;
        #10;

        $stop;
    end

endmodule
OUTPUT:![test bencher ripple](https://github.com/user-attachments/assets/dab64114-6724-4e9c-9dd3-c954ecec2c36)



     




// Verilog Code ripple counter

module ripple_counter_4bit (
    input clk,           // Clock signal
    input reset,         // Reset signal
    output reg [3:0] Q   // 4-bit output for the counter value
);
4-bit Ripple Counter Code

module RippleCounter_4bit (
    input clk,           // Clock signal
    input rst,           // Reset signal
    output [3:0] count   // 4-bit count output
);

    reg [3:0] count_reg; // Register to hold the counter value

    // Output assignment
    assign count = count_reg;

    // Function to increment the counter
    function [3:0] increment;
        input [3:0] value;
        begin
            increment = value + 1;
        end
    endfunction

    // Always block triggered on the positive edge of the clock or reset
    always @(posedge clk or posedge rst) begin
        if (rst)
            count_reg <= 4'b0000;           // Reset the counter to 0
        else
            count_reg <= increment(count_reg); // Increment the counter
    end

endmodule
// TestBench

Testbench for 4-bit Ripple Counter

module RippleCounter_4bit_tb;

    reg clk;
    reg rst;
    wire [3:0] count;

    // Instantiate the Ripple Counter
    RippleCounter_4bit uut (
        .clk(clk),
        .rst(rst),
        .count(count)
    );

    // Clock generation
    initial begin
        clk = 0;
        forever #5 clk = ~clk; // 10 time units period (50 MHz)
    end

    // Test sequence
    initial begin
        // Test case: Reset counter
        rst = 1;
        #10;
        rst = 0;
        
        // Let the counter run for a few clock cycles
        #100;

        // Apply reset again
        rst = 1;
        #10;
        rst = 0;
        
        // Run for a few more cycles to observe the count behavior
        #100;

        $stop; // Stop simulation
    end

endmodule

OUTPUT:![ripple counter](https://github.com/user-attachments/assets/d49f670a-7522-49ff-b465-759755716113)

Conclusion:
The 4-bit Ripple Carry Adder was successfully designed and implemented using Verilog HDL with the help of a task for the full adder logic. The testbench verified that the ripple carry adder correctly computes the 4-bit sum and carry-out for various input combinations. The simulation results matched the expected outputs.

The 4-bit Ripple Counter was successfully designed and implemented using Verilog HDL. A function was used to calculate the next state of the counter.


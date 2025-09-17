# SIMULATION AND IMPLEMENTATION OF 4:1 MULTIPLEXER

## AIM
To design and simulate a 4:1 Multiplexer (MUX) using Verilog HDL in four different modeling styles—Gate-Level, Data Flow, Behavioral, and Structural—and to verify its functionality through a testbench using the Vivado 2023.1 simulation environment. The experiment aims to understand how different abstraction levels in Verilog can be used to describe the same digital logic circuit and analyze their performance.

## APPARATUS REQUIRED
- **Vivado 2023.1**

## Procedure

1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** and give a name (e.g., `Mux4_to_1`).  
3. Add/create your Verilog files and testbench.  
4. Select an FPGA part (e.g., `xc7a35ticsg324-1L`).  
5. Run **Synthesis** to check for errors.  
6. Run **Simulation** → **Run Behavioral Simulation**.  
7. Observe the waveforms of inputs and outputs.  
8. Adjust simulation time if needed (e.g., 1000ns).  
9. Save the project and take screenshots of results.  
10. Close simulation.  

---

## Logic Diagram
![image](https://github.com/user-attachments/assets/d4ab4bc3-12b0-44dc-8edb-9d586d8ba856)

---

## Truth Table
![image](https://github.com/user-attachments/assets/c850506c-3f6e-4d6b-8574-939a914b2a5f)

---

## Verilog Code

### 4:1 MUX Gate-Level Implementation
```verilog
// Gate Level Modelling - Skeleton
module mux4_gate (
    input  wire I0, I1, I2, I3,
    input  wire S0, S1,
    output wire Y
);
    wire w1, w2, w3, w4;
    and g1(w1, I0, ~S0, ~S1);
    and g2(w2, I1, ~S0,  S1);
    and g3(w3, I2,  S0, ~S1);
    and g4(w4, I3,  S0,  S1);
    or g5(Y, w1, w2, w3, w4);

endmodule

```
### 4:1 MUX Gate-Level Implementation- Testbench
```verilog
// Testbench Skeleton
`timescale 1ns/1ps
module tb_mux4_gate;

    // Declare testbench signals
    reg I0, I1, I2, I3;
    reg S0, S1;
    wire Y;

    // Instantiate DUT
    mux4_gate uut (.I(I), .S(S), .Y(Y));

    initial begin
        $monitor("Time=%0t | I0=%b I1=%b I2=%b I3=%b | S1S0=%b%b | Y=%b",
                 $time, I0, I1, I2, I3, S1, S0, Y);

        // First test case
        I0=1; I1=0; I2=1; I3=0;
        S0=0; S1=0; #10;
        S0=1; S1=0; #10;
        S0=0; S1=1; #10;
        S0=1; S1=1; #10;

        // Second test case
        I0=0; I1=0; I2=1; I3=0;
        S0=0; S1=0; #10;
        S0=1; S1=0; #10;
        S0=0; S1=1; #10;
        S0=1; S1=1; #10;

        $finish;
    end
endmodule
```
## Simulated Output Gate Level Modelling

<img width="1918" height="1079" alt="Screenshot 2025-09-17 201103" src="https://github.com/user-attachments/assets/bc2ad232-71b6-423b-87ce-81b743d262fb" />


---
### 4:1 MUX Data flow Modelling
```verilog
// Dataflow Modelling - Skeleton
module mux4_dataflow (
    input  wire I0, I1, I2, I3,
    input  wire S0, S1,
    output wire Y
);
    wire [4:1] w;
    assign w[1] = I0 & ~S1 & ~S0; 
    assign w[2] = I1 & ~S1 &  S0; 
    assign w[3] = I2 &  S1 & ~S0; 
    assign w[4] = I3 &  S1 &  S0; 

    assign Y = w[1] | w[2] | w[3] | w[4];
endmodule
```
### 4:1 MUX Data flow Modelling- Testbench
```verilog
// Testbench Skeleton
`timescale 1ns/1ps
module tb_mux4_dataflow;

    // Declare testbench signals
    reg [3:0] I;
    reg [1:0] S;
    wire Y;


    // Instantiate DUT
  mux4_dataflow  uut (.I(I), .S(S), .Y(Y));
    initial begin
        $monitor("Time=%0t | I=%b | S=%b | Y=%b", $time, I, S, Y);

        I = 4'b1010;
        S = 2'b00; #10;
        S = 2'b01; #10;
        S = 2'b10; #10;
        S = 2'b11; #10;

        I = 4'b0010;
        S = 2'b00; #10;
        S = 2'b01; #10;
        S = 2'b10; #10;
        S = 2'b11; #10;

        $finish;
    end
endmodule

```
## Simulated Output Dataflow Modelling
<img width="1912" height="1077" alt="image" src="https://github.com/user-attachments/assets/fbb5b84b-5d53-4047-a293-b9512186890c" />


---
### 4:1 MUX Behavioral Implementation
```verilog
module mux4_to_1_behavioral (
    input  wire A, B, C, D,   
    input  wire S0, S1,       
    output reg  Y            
);
    always @(*) begin
        case ({S1, S0})  
            2'b00: Y = A;
            2'b01: Y = B;
            2'b10: Y = C;
            2'b11: Y = D;
            default: Y = 1'b0;  
        endcase
    end
endmodule

```
### 4:1 MUX Behavioral Modelling- Testbench
```verilog
// Testbench Skeleton
`timescale 1ns/1ps
module tb_mux4_dataflow;

    // Declare testbench signals
    reg [3:0] I;
    reg [1:0] S;
    wire Y;


    // Instantiate DUT
  mux4_to_1_behavioral uut (.I(I), .S(S), .Y(Y));
    initial begin
        $monitor("Time=%0t | I=%b | S=%b | Y=%b", $time, I, S, Y);

        I = 4'b1010;
        S = 2'b00; #10;
        S = 2'b01; #10;
        S = 2'b10; #10;
        S = 2'b11; #10;

        I = 4'b0010;
        S = 2'b00; #10;
        S = 2'b01; #10;
        S = 2'b10; #10;
        S = 2'b11; #10;

        $finish;
    end
endmodule

```
## Simulated Output Behavioral Modelling

<img width="1913" height="1078" alt="image" src="https://github.com/user-attachments/assets/ed9a8e2d-cf54-400c-825f-8ce8efe32f58" />



### 4:1 MUX Structural Implementation

![image](https://github.com/user-attachments/assets/eea81c2c-7dfa-43aa-9cea-1ab4ed54db6c)


```verilog
module MUX_2_1(a, b, s, z);
input a, b, s;
output z;
wire ns, w1, w2;
not g0(ns, s);
and g1(w1, a, ns);
and g2(w2, b, s);
or  g3(z, w1, w2);
endmodule

module MUX_4_1(I,S,Y);
input [3:0] I;
input [1:0] S;
output Y;
wire w1,w2;
  MUX_2_1 m1(I[0], I[1], S[0], w1);  
  MUX_2_1 m2(I[2], I[3], S[0], w2);  
  MUX_2_1 m3(w1, w2, S[1], Y);
endmodule
```
### Testbench Implementation
```verilog
`timescale 1ns / 1ps
module tb_mux4_dataflow;

    reg [3:0] I;
    reg [1:0] S;
    wire Y;


    // Instantiate DUT
  MUX_4_1 uut (.I(I), .S(S), .Y(Y));
    initial begin
        $monitor("Time=%0t | I=%b | S=%b | Y=%b", $time, I, S, Y);

        I = 4'b1010;
        S = 2'b00; #10;
        S = 2'b01; #10;
        S = 2'b10; #10;
        S = 2'b11; #10;

        I = 4'b0010;
        S = 2'b00; #10;
        S = 2'b01; #10;
        S = 2'b10; #10;
        S = 2'b11; #10;

        $finish;
    end
endmodule
```
## Simulated Output Structural Modelling
<img width="1916" height="1078" alt="image" src="https://github.com/user-attachments/assets/4392a8cc-68c3-4bad-b07a-f6b4b73b26ba" />


---
### CONCLUSION

In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural.The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions.


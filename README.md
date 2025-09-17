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
    input  wire [3:0] I,   
    input  wire [1:0] S,   
    output wire Y         
);
    wire w1, w2, w3, w4;

    // AND gates with ~ usage
    and g1(w1, I[0], ~S[0], ~S[1]); // Select I0
    and g2(w2, I[1], ~S[0],  S[1]); // Select I1
    and g3(w3, I[2],  S[0], ~S[1]); // Select I2
    and g4(w4, I[3],  S[0],  S[1]); // Select I3

    // OR gate
    or g5(Y, w1, w2, w3, w4);
endmodule

```
### 4:1 MUX Gate-Level Implementation- Testbench
```verilog
`timescale 1ns / 1ps
module tb_mux_4_1;
    reg [3:0] I;
    reg [1:0] S;
    wire Y;

    // Instantiate DUT
    mux4_gate uut (.I(I), .S(S), .Y(Y));

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
## Simulated Output Gate Level Modelling

<img width="1918" height="1079" alt="Screenshot 2025-09-17 201103" src="https://github.com/user-attachments/assets/4e31a3b4-84e8-4b3b-8823-780677895237" />


---
### 4:1 MUX Data flow Modelling
```verilog
module mux4_dataflow (
    input  wire I0, I1, I2, I3,
    input  wire S0, S1,
    output wire Y
);
    wire [4:1] w;

    assign w[1] = I0 & ~S1 & ~S0; // S=00 → I0
    assign w[2] = I1 & ~S1 &  S0; // S=01 → I1
    assign w[3] = I2 &  S1 & ~S0; // S=10 → I2
    assign w[4] = I3 &  S1 &  S0; // S=11 → I3

    assign Y = w[1] | w[2] | w[3] | w[4];
endmodule

```
### 4:1 MUX Data flow Modelling- Testbench
```verilog
`timescale 1ns / 1ps
    module tb_mux_4_1;
    reg [3:0] I;
    reg [1:0] S;
    wire Y;

    // Instantiate DUT
   mux4_dataflow uut (.I(I), .S(S), .Y(Y));

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

<img width="1913" height="1078" alt="Screenshot 2025-09-17 202606" src="https://github.com/user-attachments/assets/bdba5f25-826a-49a8-85b5-2b38eddbb4e4" />


---
### 4:1 MUX Behavioral Implementation
```verilog
module mux4_to_1_behavioral (
    input  wire A, B, C, D,   // Data inputs
    input  wire S0, S1,       // Select inputs
    output reg  Y             // Output
);
    always @(*) begin
        case ({S1, S0})   // Concatenate select lines
            2'b00: Y = A;
            2'b01: Y = B;
            2'b10: Y = C;
            2'b11: Y = D;
            default: Y = 1'b0;  // Safe default
        endcase
    end
endmodule
```
### 4:1 MUX Behavioral Modelling- Testbench
```verilog
`timescale 1ns / 1ps
module tb_mux_4_1;
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

<img width="1914" height="1079" alt="Screenshot 2025-09-17 203246" src="https://github.com/user-attachments/assets/6acaa0c2-9d37-4302-bf39-7aa100fc375f" />


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
module tb_mux_4_1;
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

<img width="1916" height="1079" alt="Screenshot 2025-09-17 203907" src="https://github.com/user-attachments/assets/ea96a947-dc4a-4662-b368-4f7a95137122" />


---
### CONCLUSION

In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural.The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions.


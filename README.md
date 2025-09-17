# EXPERIMENT 3: Simulation of All Flip-Flops using Blocking Statement

## AIM
To design and simulate basic flip-flops (SR, D, JK, and T) using **blocking statements** in Verilog HDL, and verify their functionality through simulation in Vivado 2023.1.

## APPARATUS REQUIRED
- Vivado 2023.1
- Computer with HDL Simulator

## DESCRIPTION
Flip-flops are the basic memory elements in sequential circuits.  
In this experiment, different types of flip-flops (SR, D, JK, T) are modeled using **behavioral modeling** with **blocking assignment (`=`)** inside the `always` block.  
Blocking assignments execute sequentially in the given order, which makes it easier to describe simple synchronous circuits.

## PROCEDURE
1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** (e.g., `FlipFlop_Simulation`).  
3. Add Verilog source files for each flip-flop (SR, D, JK, T).  
4. Add a testbench file to verify all flip-flops.  
5. Run **Behavioral Simulation**.  
6. Observe waveforms of inputs and outputs for each flip-flop.  
7. Verify that outputs match the truth table.  
8. Save results and capture simulation screenshots.

---

## VERILOG CODE

### SR Flip-Flop (Blocking)
```verilog
module sr_ff (input clk,input S,input R,output reg Q);
always @(posedge clk)
 begin
    case ({S,R})
      2'b00: Q <= Q;    
      2'b01: Q <= 0;    
      2'b10: Q <= 1;    
      2'b11: Q <= 1'bx; 
 endcase
 end
endmodule
```
### SR Flip-Flop Test bench 
```verilog
module sr_ff_tb;
  reg clk, S, R;
  wire Q;
  sr_ff uut (.clk(clk),.S(S),.R(R),.Q(Q));
  initial begin
   clk = 0;
  forever #10 clk = ~clk; 
  end
  initial begin
    S = 0; R = 0;
    #100 S = 1; R = 0;   
    #100 S = 0; R = 0;   
    #100 S = 0; R = 1;   
    #100 S = 1; R = 1;  
    #100 S = 0; R = 0;
 end
endmodule


```
#### SIMULATION OUTPUT

<img width="1037" height="539" alt="Screenshot 2025-09-17 182044" src="https://github.com/user-attachments/assets/e2827205-338c-4d88-9590-1c9e5305cf79" />

---

### JK Flip-Flop (Blocking)
```verilog
module jk_ff(input clk,J,K, output reg Q);
always @(posedge clk) begin
case({J,K})
2'b00: Q<=Q;
2'b01: Q<=0;
2'b10: Q<=1;
2'b11: Q<=~Q;
endcase
end
endmodule
```
### JK Flip-Flop Test bench 
```verilog
module tb_jk_ff;
  reg clk;
  reg J, K;
  wire Q;
  jk_ff uut (.clk(clk),.J(J),.K(K),.Q(Q));
initial begin
clk=0;
forever #20 clk=~clk;
end
initial begin
 J = 0; K = 0;
    #100 J=0; K=0;  
    #100 J=0; K=1; 
    #100 J=1; K=0;  
    #100 J=1; K=1;  
    #100 J=0; K=1; 
    #100 J=1; K=0;  
    #100 J=1; K=1;  
end
endmodule


```
#### SIMULATION OUTPUT

<img width="1038" height="558" alt="Screenshot 2025-09-17 182222" src="https://github.com/user-attachments/assets/5e68459f-7efe-4973-aabc-78d1e0ff1233" />

---
### D Flip-Flop (Blocking)
```verilog
module D_flip_flop(clk,rst,d,dout);
input clk,rst,d;
output reg dout;
always @(posedge clk)
begin 
if ((rst)||rst==1'b1)
dout=1'b0;
else
dout=d;
end
endmodule
```
### D Flip-Flop Test bench 
```verilog
module D_flip_flop_tb;
reg clk_t,rst_t,d_t;
wire dout_t;
D_flip_flop dut(.clk(clk_t),.rst(rst_t),.d(d_t),.dout(dout_t));
initial
begin 
clk_t=1'b0;
rst_t=1'b0;
#20
rst_t=1'b1;
d_t=1'b0;
#20
d_t=1'b1;
end 
always
#10
clk_t=~clk_t;
endmodule


```

#### SIMULATION OUTPUT

<img width="1030" height="549" alt="Screenshot 2025-09-17 181759" src="https://github.com/user-attachments/assets/6d0fb9a0-e2ac-4846-a59b-912b4495e2e2" />

---
### T Flip-Flop (Blocking)
```verilog
module T_flip_flop(clk,rst,T,Tout);
input clk,rst,T;
output reg Tout;
always @(posedge clk)
begin 
if (rst)
Tout=1'b0;
else if (T)
Tout=~Tout;
else
Tout=Tout;
end 
endmodule
```
### T Flip-Flop Test bench 
```verilog
module T_flip_flop_tb;
reg clk_t,rst_t,T_t;
wire Tout_t;
T_flip_flop dut(.clk(clk_t),.rst(rst_t),.T(T_t),.Tout(Tout_t));
initial
begin
    clk_t=1'b0;
    rst_t=1'b1;
    #20
    rst_t=1'b0;
    T_t=1'b0;
    #20
    T_t=1'b1;
    end 
    always
    #10 
clk_t=~clk_t;  
endmodule 


```

#### SIMULATION OUTPUT

<img width="1041" height="552" alt="Screenshot 2025-09-17 181920" src="https://github.com/user-attachments/assets/f3e97792-7a23-4a17-ba0e-397cab46e77b" />


---

### RESULT

All flip-flops (SR, D, JK, T) were successfully simulated using blocking statements in Verilog HDL.
The outputs matched the expected truth table values, demonstrating correct sequential behavior.

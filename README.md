# Exp-6a.-Finite-State-Machine-for-Sequence-Detector-1011
# Aim 
To design and simulate a Finite-State-Machine-for-Sequence-Detector-1011 using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 environment. 

# Apparatus Required 

Vivado 2023.1 

# Procedure

Launch Vivado 2023.1 Open Vivado and create a new project.
Design the Verilog Code Write the Verilog code for the RAM,ROM,FIFO
Create the Testbench Write a testbench to simulate the memory behavior. The testbench should apply various and monitor the corresponding output.
Create the Verilog Files Create both the design module and the testbench in the Vivado project.
Run Simulation Run the behavioral simulation to verify the output.
Observe the Waveforms Analyze the output waveforms in the simulation window, and verify that the correct read and write operation.
Save and Document Results Capture screenshots of the waveform and save the simulation logs. These will be included in the lab report.
# Block Diagram
# Mealy 1011

<img width="373" height="657" alt="image" src="https://github.com/user-attachments/assets/c1905341-83c8-42f4-b839-1f102b75fa49" />

# Moore 1011

<img width="144" height="300" alt="image" src="https://github.com/user-attachments/assets/ec255e61-3c47-4a62-8505-38e12efd2e6c" />

# Code
# Mealy 1011
# Verilog code

```
module mealy(input clk, rst, xin, output reg z);
parameter [1:0] s0= 2'b00, s1= 2'b01, s2= 2'b10, s3= 2'b11;
reg[1:0] ps,ns;
always @(posedge clk)
begin
    if(rst)
       ps<=0;
    else
       ps<=ns;
    end
always @(ps,xin)
begin 
   case (ps)
   s0: if(xin) begin
         ns<=s1; z<=0;
         end
        else begin
         ns<=s0; z<=0;
         end
    s1: if(xin) begin
          ns<=s1; z<=0;
          end
         else begin
          ns<=s2; z<=0;
          end
     s2: if(xin) begin
           ns<=s3; z<=0;
           end
         else begin
           ns<=s0; z<=0;
           end
      s3: if(xin) begin
             ns<=s0; z<=1;
             end
           else begin
              ns<=s0; z<=0;
              end
              endcase
              end
endmodule
```
# Test bench
```
module mealy_tb;
reg clk,rst,xin;
wire z;
mealy dut(clk,rst,xin,z);
initial
    begin
      clk=1'b1;
      rst=1'b1;
   #10
      rst=1'b0;
      xin=1'b1;
    #10
       xin=1'b0;
    #10
       xin=1'b1;
    #10
       xin=1'b1;
    end
       always #5 clk=~clk;
       endmodule
```
# Output Waveform

<img width="1920" height="1200" alt="Screenshot 2025-10-17 111053" src="https://github.com/user-attachments/assets/298d66eb-56c9-462a-9e44-51e992cf0706" />

# Moore 1011
# Verilog Code

```
module moore_2(clk,rst,in,out);
input clk,in,rst;
output reg out;
parameter [2:0] s0=3'b000,
                s1=3'b001,
                s2=3'b010,
                s3=3'b011,
                s4=3'b100;
reg [2:0] current_state,next_state;
always @(posedge clk)
begin
if(rst)
current_state<=s0;
else
current_state<=next_state;
end
always @(current_state,in)begin
    case(current_state)
        s0: if(in) begin
        next_state=s1; out=0;
        end 
        else begin
        next_state=s0; out=0;
         end
        s1: if(in) begin
        next_state=s1; out=0;
        end
        else begin
        next_state=s2; out=0;
        end
        s2: if(in) begin
        next_state=s3; out=0;
        end
        else begin
        next_state=s0; out=0;
        end
        s3: if(in) begin
        next_state=s4; out=0;
        end 
        else begin
        next_state=s2; out=0;
        end
        s4: if(in) begin 
        next_state=s1; out=1;
        end
        else begin
        next_state=s0; out=1;
        end
        endcase
        end
endmodule
```
# Test Bench
```
module moore_2_tb;
reg clk,rst,in;
wire out;
moore_2 uut(clk,rst,in,out);
initial
begin
    clk=0;
    forever #5clk=~clk;
end
initial
begin
rst=1;
in=0;
#10 rst=0;
#10 in=1;
#10 in=0;
#10 in=1;
#10 in=1;
#10 $finish;
end
endmodule
```
# Output Waveform

<img width="1920" height="1200" alt="Screenshot 2025-10-17 112325" src="https://github.com/user-attachments/assets/93e8f324-1363-4e33-84db-ca73651373ee" />

# Conclusion
The Mealy and Moore state machine for sequence 1011 was designed and successfully simulated using Verilog HDL. The testbench verified both the write and read functionalities by simulating the sequence operations and observing the output waveforms.

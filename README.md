# HW5


PRPG design :
```script=
module Pseudo_Random_Pattern_Generator (clk,Load);

input clk;
input Load;
output [2:0] Din;
output [2:0] Q_out;
output Q1, Q2, Q3;

reg [2:0] Din;
reg [2:0] Q_out;
reg Q1, Q2, Q3;
reg temp;

initial
begin
    temp = 0;
    Q1 = 1;
    Q2 = 0;
    Q3 = 0;
    Q_out = 3'b001;
    Din = 3'b111;
end

always@ (posedge clk or posedge Load)
begin
    if(Load)
    begin
        Q1 = Din[0];
        Q2 = Din[1];
        Q3 = Din[2];
    end

    else
    begin
        temp = Q1;
        Q1 = Q1 ^ Q3;
        Q3 = Q2;
        Q2 = temp;
    end

    Q_out[0] = Q1;
    Q_out[1] = Q2;
    Q_out[2] = Q3;
end
endmodule
```
PRPG testbench :
```
`timescale 1ns/1ps
module Pseudo_Random_Pattern_Generator_tb;
reg clk_tb;
reg Load_tb;
reg Din_tb;

initial
begin
    clk_tb = 0;
    Din_tb = 4;
    Load_tb = 1'b0;
end

always
begin
    #10 clk_tb = ~clk_tb;
end

initial
begin
    #30 Load_tb = 1'b0;
    #80 Load_tb = 1'b1;
    #5 Load_tb = 1'b0;
    #250 $finish;
end

initial
begin
    $dumpfile("Pseudo_Random_Pattern_Generator.vcd");
    $dumpvars(0, Pseudo_Random_Pattern_Generator_tb);
end

Pseudo_Random_Pattern_Generator Pseudo_Random_Pattern_Generator_tb
(
    .clk(clk_tb),
    .Load(Load_tb)
);
endmodule 
```
![image](https://user-images.githubusercontent.com/101077336/173273153-83d3277c-100f-4ccd-aee1-562cc43d35f7.png)

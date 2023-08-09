# ASIC_Design_ProjectWork
This repository highlights the work done by Nitesh Sharma pretaining to ASIC Design course.
<br>
Quick links:
<br>
[Tools Installation](#day0)
<details>
  <summary> <strong>Day_0</strong></summary>
	<a name="day0"></a>
   <details>
     <summary>iverilog</summary>
     Commands used for iverilog installation:
     
         
```
sudo apt-get update
sudo apt-get install iverilog
```
iverilog Installation
<img src="https://user-images.githubusercontent.com/140998787/257133371-5ee81e29-7172-4958-8619-d11be643f8be.png">
    
Checking for installation:
<img src="https://user-images.githubusercontent.com/140998787/257133385-23e46cdd-5286-4ac5-bfa6-ed7c014c27e5.png">
   </details>
  <details>
    <summary>gtkwave</summary>
Command used for gtkwave installation
    
```
        sudo apt-get install gtkwave
```
    
    
gtkwave installation:

<img src ="https://user-images.githubusercontent.com/140998787/257133351-15fdb0a8-0544-42ec-9728-ccc504e13f57.png">

Checking for Installation<br>
<img src =https://user-images.githubusercontent.com/140998787/257133362-8a86c9a3-9e0b-4685-882f-7aa89ffb799a.png>
    
  </details>
  <details>
    <summary>Yosys</summary>
    Commands for Yosys Installation:
		
```
sudo apt-get update
sudo apt-get install yosys
```

Yosys Installation:
<br>
    <img src="https://user-images.githubusercontent.com/140998787/257133314-abdfaa0a-5801-477e-8d73-dc2c873db915.png">
    <br>
Checking Installation of Yosys
    <br>
    <img src="https://user-images.githubusercontent.com/140998787/257133324-7888d6c3-e880-4da1-b751-961ec80847b8.png">
  </details>
</details>



<details>
	<summary><strong>Day 1</strong></summary>
	<h3>Overview</h3>
        <p>Today I performed Logic synthesis of a simple 2X1 MUX based on <a>sky130_fd_sc_hd__tt_025C_1v80</a> library  as my LAB 1 work. Tools used in this process included : 
	<ul>
		<li>Iverilog</li>
		<li>GTKwave</li>
		<li>Yosys</li>
	</ul>
<hr>
  <h4>Step 1</h4> Below is the verilog code for a 2x1 MUX:<br>
	
```
   module good_mux (input i0 , input i1 , input sel , output reg y);
   always @ (*)
   begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
   end
   endmodule
   
```
<br>
Testbench

```
`timescale 1ns / 1ps
module tb_good_mux;
	// Inputs
	reg i0,i1,sel;
	// Outputs
	wire y;

        // Instantiate the Unit Under Test (UUT)
	good_mux uut (
		.sel(sel),
		.i0(i0),
		.i1(i1),
		.y(y)
	);

	initial begin
	$dumpfile("tb_good_mux.vcd");
	$dumpvars(0,tb_good_mux);
	// Initialize Inputs
	sel = 0;
	i0 = 0;
	i1 = 0;
	#300 $finish;
	end

always #75 sel = ~sel;
always #10 i0 = ~i0;
always #55 i1 = ~i1;
endmodule
```
<br>
Command used to simulate code and corresponding Testbench:

```
iverilog good_mux.v tb_good_mux.v

```
<br>
Command for viewing output waveforms:

```
gtkwave
```
<br>
Image for execution of above commands
<img src = "https://user-images.githubusercontent.com/140998787/259437514-7856401e-6790-4f8a-95c0-bec59d68a400.png">
<br>


</p>
	
</details>

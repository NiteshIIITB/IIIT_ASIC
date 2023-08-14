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
	<h4>Step 0</h4> Accesing necessary resources from <a href ="https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop">github.</a><br>
	<img src = "https://user-images.githubusercontent.com/140998787/259470853-fb49dc3d-fd55-4d65-b31c-8b8fd15e8b4d.png">
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

<img src ="https://user-images.githubusercontent.com/140998787/259467729-00235466-2269-4be8-a22e-83b48714f58f.png">
<br>
<h4>Step 2:</h4> <br>
Once it is verified that code produces the same output as expected . Now we add libraries related to technology node and generate schematic and netlist using yosys.
Following commands are used for doing so:

```
yosys
```

```
read_liberty -lib file_address
```

```
read_verilog Verilog_Filename.v
```

```
synth -top Design_name
```

```
abc -liberty library_location
```

<br>
<img src = "https://user-images.githubusercontent.com/140998787/259470836-dd472df9-c279-445a-bf5f-d429a8363118.png">
<img src =" https://user-images.githubusercontent.com/140998787/259481064-7ad5f69c-1a2a-4e71-b9db-2006e45d6ae4.png">
<br>
Generated Schematic using Technology Cells: <br>
<img src="https://user-images.githubusercontent.com/140998787/259481001-a89d0297-30c1-4dc2-bb4e-b8231d8f99af.png">


<br>
<img src ="https://user-images.githubusercontent.com/140998787/259470856-2f8403a1-427e-4b63-98d5-b24ad5da08c1.png">
<br><br>
Generated Netlist
<br><br>

```
/* Generated by Yosys 0.23 (git sha1 7ce5011c24b) */

module good_mux(i0, i1, sel, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  input i0;
  wire i0;
  input i1;
  wire i1;
  input sel;
  wire sel;
  output y;
  wire y;
  sky130_fd_sc_hd__mux2_1 _4_ (
    .A0(_0_),
    .A1(_1_),
    .S(_2_),
    .X(_3_)
  );
  assign _0_ = i0;
  assign _1_ = i1;
  assign _2_ = sel;
  assign y = _3_;
endmodule
```



</p>
	
</details>
<details>
	<summary><strong>Day 3</strong></summary>
	<h2>Theory</h2>
	<h3>Overview</h3>
	<p>This section highlights various logic optimisations performed by the synthesis tools.The major advantage of doing optimisations is that design becomes efficient and occupies less area and consumes lesser power. </p>
	<h3>Combinational logic optimisations</h3>
	<p>Common ways of doing combinational logic optimisations:
	 <ul>
		 <li><b>Constant Propogation :</b>Constant propagation is a specific technique used in logic optimization to simplify combinational logic circuits by replacing variables with                            their constant values wherever possible. This optimization helps reduce the complexity of the circuit and can lead to improvements in both performance and area          utilization.</li>
		 <li><b>Boolean Logic Optimisation :</b>
		 <ul>
			 <li>Karnaugh Maps (K-Maps): A graphical method used to identify and group minterms or maxterms in a truth table, leading to simplified Boolean expressions.</li>
			 <li>Quine-McCluskey Method: An algorithmic approach to find the minimal sum-of-products (or product-of-sums) expressions for a Boolean function. </li>
		 </ul>There are various other ways of Boolean logic Optimisations.</li>
	 </ul><br>Demonstrations of these optimisations are included in the following lab work.</p>
	<h3>Sequential Logic Optimisations</h3>
	<p>
		<ul>
			<li><b>Basic</b>
			<ul><li>Sequential Constant Propogation: Similar to combinational logic optimization, the goal is to identify signals that are guaranteed to be constant at specific points                                  in time and then propagate these constants through the sequential elements.</li></ul>
			Demonstrations of these are included in the following</li>
                       <li><b>Advanced</b>
		           <ul>
			    <li>State Optimisation : State optimization minimizes the number of states in a finite state machine by identifying and collapsing equivalent or unreachable states,                                      reducing complexity and improving efficiency.</li> 
			      <li>Sequential Logic Cloning</li> 
			       <li>Retiming : It refers to adjusting delays of intermediate combinational circuits to enhance the operating speed of the digital circuit.</li>
		             </ul>
		       </li>	
		</ul>
	</p>
	<h2>Demonstrations</h2>
	<details><summary><strong>Combinational logic optimisation</strong></summary>
	<h3>Combinational logic optimisation</h3>
 <br>
 <h3>Design 1</h3>

 ```
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule
```

<br>
<b>Unoptimised Implementation:</b>
   <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260305003-db18752e-aca7-4411-b132-f7dbf4b4305c.jpeg" width="400" height="400">
  </div>
  <br>
  <b>Synthesis Tool Output:</b>
   <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260244399-92d0fe15-cdef-42db-a350-86de660efed7.png">
  </div>
  <br>
  
    
<p>
	<b>Equivalence of both</b><br>
	
```
         From Unoptimised  Circuit:
	 Y = a.b + a`.0
         Y = a.b
	 Both circuits are equivalent
	
```

</p>
<p>
	<h4>Steps Involved</h4>
	 <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260244396-12282dc5-4d76-4a0f-a63e-4a9d4ffa91ff.png">
  </div>
<br>
 <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260244395-1bcb75d8-8211-49a4-b5f9-2ef8657736d9.png">
  </div>
  <br>
   <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260244394-6f73b3ea-42e9-4892-ad7d-a3f398997ee0.png">
  </div>
  <br>
   <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260244391-e56d3b0a-c62f-4aa8-8ee8-e3639413283a.png">
  </div>
  
</p>
<br>

<h3>Design 2</h3><br>

```
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```

<br>
<b>Unoptimised Implementation:</b>
   <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260305542-53f343c9-6fe2-4aea-95a8-7bdfcc7cb85e.jpeg" width="400" height="400">
  </div>
  <br>
  <b>Synthesis Tool Output:</b>
   <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260247178-138e3d4e-fb3e-4783-8c55-428f247ecade.png">
  </div>
  <br>
  
    
<p>
	<b>Equivalence of both</b><br>
	
```
         From Unoptimised  Circuit:
	 Y = a.1 + a`.b
         Y = a + a`.b   
         Y = a + b  (by distributive law)
	 Both circuits are equivalent
	
```

</p>
<p>
	<h4>Steps Involved</h4>
	 <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260247175-01ceb27e-313c-4db7-b9f7-6baa33196c8d.png">
  </div>
<br>
 <div align="center">
    <img src="https://github.com/NiteshIIITB/IIIT_ASIC/assets/140998787/9411cbe6-219d-4a7f-a608-041144a9b641">
  </div>
  <br>
   <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260247172-99d653b5-d7a8-4b20-8a71-58bfb12f3779.png">
  </div>
  
  
</p>

<br>

<h3>Design 3</h3><br>

```

module opt_check3 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```
<br>
<b>Unoptimised Implementation:</b>
   <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260306384-db70ba57-c14e-43c2-8a18-7d337f026bcf.jpeg" width="400" height="400">
  </div>
  <br>
  <b>Synthesis Tool Output:</b>
   <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260247998-e99d5ca1-ef36-433f-911a-b2946ffe1ad1.png">
  </div>
  <br>
  
    
<p>
	<b>Equivalence of both</b><br>
	
```
         From Unoptimised  Circuit:
	 Y = a.(c.b + c`.0) + a`.0
         Y = a(b.c) + 0  
         Y = a.b.c 
	 Both circuits are equivalent
	
```

</p>
<p>
	<h4>Steps Involved</h4>
	 <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260247997-f1d5c12a-7256-4d0f-b182-af6f720f7760.png">
  </div>
<br>
 <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260247996-0b0c6209-eb08-4c99-a629-5769e2915510.png">
  </div>
  <br>
   <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260247995-e7a02aa7-4375-4cdb-804f-5dcf59e4f884.png">
  </div>
  <br>
  
</p>

<br>

<h3>Design 4</h3><br>

```

module opt_check4 (input a , input b , input c , output y);
 assign y = a?(b?(a & c ):c):(!c);
 endmodule
```

<br>
<b>Unoptimised Implementation:</b>
   <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260306363-211a2c04-860c-4c46-b032-fad1516d714f.jpeg" width="400" height="400">
  </div>
  <br>
  <b>Synthesis Tool Output:</b>
   <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260249845-62f9ad6c-d55e-4640-92ec-dfc092f3091e.png">
  </div>
  <br>
  
    
<p>
	<b>Equivalence of both</b><br>
	
```
         From Unoptimised  Circuit:
	 Y = a.(b(a.c)+ b`c) + a`.c`
         Y = a(b.c.a + b`c) + a`.c`
         Y = ac(a+b`) + a`.c`
         Y = ac + ab`c + a`.c`
         Y = ac + a`.c`

	 Both circuits are equivalent
	
```

</p>
<p>
	<h4>Steps Involved</h4>
	 <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260249843-b64da2ae-d83c-4745-81d8-b15d4844257d.png">
  </div>
<br>
 <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260249842-89532985-6682-4ac8-82e6-0bf3b44cf340.png">
  </div>
  <br>
   <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260249838-1bb6a678-a2bd-4e41-a6ec-b80a85e643f9.png">
  </div>
 
    
</p>


<br>

<h3>Design 5</h3><br>

```

module sub_module1(input a , input b , output y);
 assign y = a & b;
endmodule


module sub_module2(input a , input b , output y);
 assign y = a^b;
endmodule


module multiple_module_opt(input a , input b , input c , input d , output y);
wire n1,n2,n3;

sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
sub_module2 U3 (.a(b), .b(d) , .y(n3));

assign y = c | (b & n1); 


endmodule
```

<br>
<b>Unoptimised Implementation:</b>
   <div align="center">
    <img src="path-to-your-image.jpg">
  </div>
  <br>
  <b>Synthesis Tool Output:</b>
   <div align="center">
    <img src="path-to-your-image.jpg">
  </div>
  <br>
  
    
<p>
	<b>Equivalence of both</b><br>
	
```
         From Unoptimised  Circuit:
	 Y = a.(b(a.c)+ b`c) + a`.c`
         Y = a(b.c.a + b`c) + a`.c`
         Y = ac(a+b`) + a`.c`
         Y = ac +
         Y = a.b.c 
	 Both circuits are equivalent
	
```

</p>
<p>
	<h4>Steps Involved</h4>
	 <div align="center">
    <img src="path-to-your-image.jpg">
  </div>
<br>
 <div align="center">
    <img src="path-to-your-image.jpg">
  </div>
  <br>
   <div align="center">
    <img src="path-to-your-image.jpg">
  </div>
  <br>
   <div align="center">
    <img src="path-to-your-image.jpg">
  </div>
  
</p>

<br>

<b>Design 6</b><br>

```


module sub_module(input a , input b , output y);
 assign y = a & b;
endmodule



module multiple_module_opt2(input a , input b , input c , input d , output y);
wire n1,n2,n3;

sub_module U1 (.a(a) , .b(1'b0) , .y(n1));
sub_module U2 (.a(b), .b(c) , .y(n2));
sub_module U3 (.a(n2), .b(d) , .y(n3));
sub_module U4 (.a(n3), .b(n1) , .y(y));


endmodule
```
<br>
<b>Unoptimised Implementation:</b>
   <div align="center">
    <img src="path-to-your-image.jpg">
  </div>
  <br>
  <b>Synthesis Tool Output:</b>
   <div align="center">
    <img src="path-to-your-image.jpg">
  </div>
  <br>
  
    
<p>
	<b>Equivalence of both</b><br>
	
```
         From Unoptimised  Circuit:
	 Y = a.(b(a.c)+ b`c) + a`.c`
         Y = a(b.c.a + b`c) + a`.c`
         Y = ac(a+b`) + a`.c`
         Y = ac +
         Y = a.b.c 
	 Both circuits are equivalent
	
```

</p>
<p>
	<h4>Steps Involved</h4>
	 <div align="center">
    <img src="path-to-your-image.jpg">
  </div>
<br>
 <div align="center">
    <img src="path-to-your-image.jpg">
  </div>
  <br>
   <div align="center">
    <img src="path-to-your-image.jpg">
  </div>
  <br>
   <div align="center">
    <img src="path-to-your-image.jpg">
  </div>
  
</p>
</details>
<details><summary><strong>Sequential Logic Optimisations</strong></summary>
<h3>Sequential Logic Optimisations</h3><br>
<p>In this section various cases of constant propogation in Sequential circuit are being demonstrated. Through logic optimisations via Sequential constant propogation it is seen that the cases in which Unoptimised implementation is seen as a combination of flip-flops can be optimised to a circuit without flip-flops. Though constant propogation does not simply guarantee the reduction of flip-flops as is observed in the following examples. </p><hr>


<p>
	<h3><u>Design 1</u></h3>
	
```
         module dff_const1(input clk, input reset, output reg q);
         always @(posedge clk, posedge reset)
         begin
	  if(reset)
		q <= 1'b0;
	  else
		q <= 1'b1;
         end

         endmodule

```

 <br><p>Here Code represents a D-flipflop input of which is fixed at logic 1 and reset makes output logic 0. Though the input is constant but it does not simplify the circuit as seen in the below figure depicting synthesis output.</p><br>

 <h4>Synthesis Tool Output:</h4>
 <div align ="center">
	 <img src = "https://user-images.githubusercontent.com/140998787/260280385-8940d1d3-6c27-4857-8745-9ecc7e3695fe.png">
 </div>
 
<br><h4>Explanation</h4><br>
<b>Waveform for above circuit:</b><br>
<div align ="center">
	 <img src = "https://user-images.githubusercontent.com/140998787/260283302-59f55bf4-6c76-4b3a-9d13-06a99ec810ff.png">
 </div>

 <p>From above waveforms it can be seen that the output depends on clock so presence of flip-flop is required.</p>

</p>
<p>
	<h4>Steps Involved</h4>
	 <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260280384-50caf946-cc97-481e-ab55-e827f1828589.png">
  </div>
<br>
 <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260280383-dfd9d2c6-832a-4304-ba8e-13c4576d142f.png">
  </div>
  <br>
   <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260280339-03bf3231-0c3f-4bee-a0eb-f305b0b7c360.png">
  </div>
  <br>
 
</p>


<p>
	<h3><u>Design 2</u></h3>
	
```
        module dff_const2(input clk, input reset, output reg q);
        always @(posedge clk, posedge reset)
        begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
        end
        //here q remains 1 always
        endmodule
```

<br><p> Here input D-flipflop is fixed at logic 1 and reset also makes output 1. So, q always remains 1 hence circuit gets optimised as a buffer. </p><br>
 <h4>Synthesis Tool Output:</h4>
 <div align ="center">
	 <img src = "https://user-images.githubusercontent.com/140998787/260280378-0ae20c21-027e-4898-89a1-0775f25ee37a.png">
 </div>
 
<br><h4>Explanation</h4><br>
<b>Waveform for above circuit:</b><br>
<div align ="center">
	 <img src = "https://user-images.githubusercontent.com/140998787/260280380-2d1cc7d5-6e84-4544-9b49-f84d82951852.png">
 </div>

 <p>From above waveforms it can be seen that the output  remains 1 and does not depends on clock flip-flop is not required and circuit gets optimised as a buffer.</p>

</p>
<p>
	<h4>Steps Involved</h4>
	 <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260280374-fd49b7ce-a8f0-4b4d-9ec3-0433a1048c8f.png">
  </div>
<br>
 <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260280370-1913366d-3a71-466f-90ef-811b718e08f2.png">
  </div>
  <br>
   <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260280362-5a0b8868-501d-4a51-ac99-62bc55394426.png">
  </div>
  <br>
  
</p>



<p>
	<h3><u>Design 3</u></h3>
 
	
```
       module dff_const3(input clk, input reset, output reg q);
       reg q1;

       always @(posedge clk, posedge reset)
        begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
        end

        endmodule

```

<br><p>Here code represents two cascaded D flipflops and input of 1st D-flipflop is fixed at 1. </p> <br>
 <h4>Synthesis Tool Output:</h4>
 <div align ="center">
	 <img src = "https://user-images.githubusercontent.com/140998787/260280351-4cc14a53-3fd9-4d7f-ab75-b6e9fc8c4ee6.png">
 </div>
 
<br><h4>Explanation</h4><br>
<b>Waveform for above circuit:</b><br>
<div align ="center">
	 <img src = "https://user-images.githubusercontent.com/140998787/260280354-7ec7056d-9de6-41ff-874f-4b5eb031c742.png">
 </div>

 <p>From above waveforms it can be seen that the output and the intermediate signal value depends on clock so flip-flop is required.</p>

</p>

<p>
	<h4>Steps Involved</h4>
	 <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260280346-1928aaa8-362d-4d18-84ab-44e82e9e710c.png">
  </div>
<br>
 <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260280341-7ec479b8-72e0-4e59-ae6f-5b804ca1f82d.png">
  </div>
  <br>
   <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260280339-03bf3231-0c3f-4bee-a0eb-f305b0b7c360.png">
  </div>
  <br>
 
</p>

<h3><u>Design 4</u></h3>
 
	
```
      module dff_const4(input clk, input reset, output reg q);
      reg q1;

      always @(posedge clk, posedge reset)
      begin
       if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b1;
       end
       else
       begin
		q1 <= 1'b1;
		q <= q1;
      end
      end

     endmodule

```

<br><p>Here code represents two cascaded D flipflops and input of 1st D-flipflop is fixed at 1.But unlike the previous case circuit gets optimised into buffers.</p> <br>

 <h4>Synthesis Tool Output:</h4>
 <div align ="center">
	 <img src = "https://user-images.githubusercontent.com/140998787/260280338-bdb8da9f-772c-47db-b3c6-4158ace0fbde.png">
 </div>
 
<br><h4>Explanation</h4><br>
<b>Waveform for above circuit:</b><br>
<div align ="center">
	 <img src = "https://user-images.githubusercontent.com/140998787/260280333-802853f2-a23b-49cc-8b11-95e30e8aaea5.png">
 </div>

 <p>From above waveforms it can be seen that the output and the intermediate signal value does not depends on clock so flip-flop is not required.</p>

</p>
<p>
	<h4>Steps Involved</h4>
	 <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260280336-44e68247-7336-4e8d-8c0b-1d7c127b27b8.png">
  </div>
<br>
 <div align="center">
    <img src="https://github.com/NiteshIIITB/IIIT_ASIC/assets/140998787/d141eecd-b298-437a-8ec3-cbfb9323aaa3">
  </div>
  <br>
  
</p>

<h3><u>Design 5</u></h3>
 
	
```
      
module dff_const5(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b0;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end

endmodule

```
<br><p>Here code represents two cascaded D flipflops and input of 1st D-flipflop is fixed at 1. </p> <br>

 <h4>Synthesis Tool Output:</h4>
 <div align ="center">
	 <img src = "https://user-images.githubusercontent.com/140998787/260285591-2c24121a-95d8-4f89-8ecd-ce499c96c425.png">
 </div>
 
<br><h4>Explanation</h4><br>
<b>Waveform for above circuit:</b><br>
<div align ="center">
	 <img src = "https://user-images.githubusercontent.com/140998787/260285595-5de50eb7-9a50-4671-9d60-3a93941854a5.png">
 </div>
<br>
 <p>From above waveforms it can be seen that the output and the intermediate signal value depends on clock so flip-flop is required.</p>

</p>
<p>
	<h4>Steps Involved</h4>
	 <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260285584-e1592296-a2ce-419e-9568-818a8540d888.png">
  </div>
<br>
 <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260285587-242511f8-c569-4188-9927-ed6af5636ccb.png">
  </div>
  <br>

   <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260285549-e507e91a-51d6-435b-ab76-94708f5e3a6b.png">
  </div>
  <br>

  <div align="center">
    <img src="https://user-images.githubusercontent.com/140998787/260285590-c82807db-af80-4091-92b0-80b71c6c231c.png">
  </div>
  <br>
  
</p>

<p>
	<h2>Sequential Optimisations of unused outputs</h2>
	<p>The logic elements which does not have any impact on primary outputs of the module gets optimised such that we have a circuit that drives the output in desired way and portion of circuit driving unneccessary logic elements gets removed.We understand it more clearly through following examples.</p>
        <h3>Design 1</h3>
	

```
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];

always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end

endmodule

```

<br>
In the above circuit code is defining an up counter and output depends on count[0] and not on other two bits of count. So, after synthesis we get a single D-flip flop(as shown below) rather than three flipflops. This is because output does not depends on other two bits of counter so it gets optimised to produce a circuit that is neccessary to drive the output in desired manner.

<br>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260304146-83d69d2c-cdf2-4a3e-94b9-e830f1170722.png">
</div>
<br>


<p><h4>Steps Involved</h4>
	<br>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260304141-96f4f751-b26e-4f0e-b41a-b97b333d710e.png">
</div>

<br>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260304144-2b444ba8-e5ab-4c1d-848e-054009d68b4f.png">
</div>

<br>

<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260304145-fb2e7535-1b72-4ba4-8dfb-db3fa4388316.png">
</div>

<br>

</p>
</p>
 <h3>Design 2</h3>
 
```
module counter_opt2 (input clk , input reset , output q);
reg [2:0] count;
assign q = (count[2:0] == 3'b100);

always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end

endmodule
```

<br>
In the above circuit code is defining an up counter and output depends on all the three bits of count. So, after synthesis we get three D-flip flops(as shown below) . Here circuit is not reduced because output depends all the bits of counter.

<br>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260304139-9a5a9d34-eb83-44c4-ac72-be6e16d6870e.png">
</div>
<br>


<p><h4>Steps Involved</h4>
	<br>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260304137-0ff9dd97-5f3a-4c21-ae2c-e7a0d541328d.png">
</div>

<br>
<div align = "center">
	<img src = "https://user-images.githubusercontent.com/140998787/260304138-054cf173-a2a3-4ea2-9d98-e260678ee1d2.png">
</div>
<br>


</p>
</p>


</details>

</details>

<details>
	<summary><strong>Day 4</strong></summary>
	<h2>Gate Level Simulation</h2>
	<p>Gate Level Simulation(GLS) is as step in ASIC design flow through we verify the timing constraints and the Functionality of the netlist obtained through RTL synthesis. This ensures Logical equivalence between the RTL design and Netlist obtained. The image below highlights the process of GLS :<br>
		<div align = "center">
			<img src = "https://user-images.githubusercontent.com/140998787/260448595-3e2ace43-f7af-49ca-9f2b-3d7be70d2f5c.png"> <br>
		</div>
	<br>The Design in above image refers to the netlist obtained through synthesis. The main reason for doing GLS is synthesis simulation mismatch which may occur due to a variety of reasons.
	Though GLS can be used to analyse timing aspect of the netlist obtained the focus of the following work remains on verifying Functionality.
	</p>
	<h3>Examples</h3>
	<details>
		<summary><strong>GLS for a 2x1 MUX</strong></summary>
		<h2>MUX Through Ternary operator</h2>
                <h3>Synthesis Part</h3>
		<br>
                 <h2>Verilog Code</h2>

```
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
	assign y = sel?i1:i0;
	endmodule

```
<br>
<h3>Steps Involved</h3>

<div align ="center">
	<img src = "https://user-images.githubusercontent.com/140998787/260454765-bef773de-ef9a-4ee0-9f6d-57c233252d4a.png">
</div>
<br>		
<div align ="center">
	<img src = "https://user-images.githubusercontent.com/140998787/260454763-cc74a6dd-33e7-4b7a-9759-fd508deecac1.png">
</div>
<br>	
<div align ="center">
	<img src = "https://user-images.githubusercontent.com/140998787/260457957-7c686de9-f315-4602-ac41-acd6b667d01f.png">
</div>
<br>	
<div align ="center">
	<img src = "https://user-images.githubusercontent.com/140998787/260457952-b273d22b-812e-44c7-8738-85c2f724b28e.png">
</div>
<br>	
<div align ="center">
	<img src = "https://user-images.githubusercontent.com/140998787/260457944-5c80b013-eb52-48d0-bc30-bcf8cfa73407.png">
</div>
<br>	
<div align ="center">
	<img src = "https://user-images.githubusercontent.com/140998787/260457963-07f37682-fcf0-4a54-af82-ba435884e486.png">
</div>
<br>	
<h4>Netlist Obtained</h4>
<br>

```
/* Generated by Yosys 0.23 (git sha1 7ce5011c24b) */

module ternary_operator_mux(i0, i1, sel, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  input i0;
  wire i0;
  input i1;
  wire i1;
  input sel;
  wire sel;
  output y;
  wire y;
  sky130_fd_sc_hd__mux2_1 _4_ (
    .A0(_0_),
    .A1(_1_),
    .S(_2_),
    .X(_3_)
  );
  assign _0_ = i0;
  assign _1_ = i1;
  assign _2_ = sel;
  assign y = _3_;
endmodule

```

<h3>GLS</h3>

<h4>Steps Involved</h4>
<div align = "center">
 
	
<img src="https://user-images.githubusercontent.com/140998787/260464088-0b392370-3d62-408e-be6d-5f571bbd9407.png">
</div>

<br>

<p> Waveform obtained from Netlist</p><br>
<div align = "center">
 
<img src= "https://user-images.githubusercontent.com/140998787/260464093-70b64b4f-36d3-4553-acc3-010d232bfadf.png">
</div>

<br>
<p> Here we can observe that the waveforms obtained for both RTL design and Netlist obtained is the same hence netlist obtained in this case is correct.</p>
</details>
 
</details>

<h2>References</h2>
 <ul>
<li><a href ="https://github.com/kunalg123/">Kunal Ghosh Github(Mentor)</a></li>
	
 </ul>

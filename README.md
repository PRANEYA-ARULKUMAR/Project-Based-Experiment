### Project-Based-Experiment

**AIM:**

To design and simulate the traffic light controller.

**SOFTWATE USED:**

Quartus II

**THEORY:**
	
     	Consider a controller for traffic at the intersection of three main roads.  

  ![image](https://github.com/naavaneetha/Project-Based-Experiment/assets/154305477/e3af03dd-a4de-4b21-af0a-a5a332a3e4b6)


 The traffic signal for all the three main roads have equal priority and they remain red by default.

 In state 00,the traffic signals remains red for first five counts and yellow of road1 turns on for the next four counts.

 In state 01, the green of road1 turns on for first five counts and yellow of road1 and road2 turns on for next four4 counts of this state.
 
 In state 10, the traffic signal of road1 comes back to the red and that of road2 goes to green for tee first five counts.For the next four counts the traffic signal of road2 and road3 remains yellow.


 In state 11, the traffic signal of road2 comes back to the red and that of road3 goes to green for the first five counts.For the next four counts the traffic signal of road3 turns to yellow

 At the end of four states,the traffic signal of all the three roads come back to red.

**Task Assigned**

From the HDL code given formulate the correct code  to divert the traffic to path 1 direction and disable the control in other directions (Assume user is at MR3 spot)

**Procedure**

1.	Type the program in Quartus software.
2.	Compile and run the program.
3.	Generate the RTL schematic and save the logic diagram.
4.	Create nodes for inputs and outputs to generate the timing diagram.
5.	For different input combinations generate the timing diagram
   
**Program:**
```Developed by: A PRANEYA
 RegisterNumber:24900343
```

```
module traffic_light_controller (
    input wire clk,         // Clock signal
    input wire reset,       // Reset signal
    output reg [2:0] MR1,   // Traffic lights for MR1: {Red, Yellow, Green}
    output reg [2:0] MR2,   // Traffic lights for MR2: {Red, Yellow, Green}
    output reg [2:0] MR3    // Traffic lights for MR3: {Red, Yellow, Green}
);

    // State Encoding
    parameter S00 = 2'b00, S01 = 2'b01, S10 = 2'b10, S11 = 2'b11;

    reg [1:0] state;        // Current state
    reg [3:0] count;        // Timer counter for each state

    always @(posedge clk or posedge reset) begin
        if (reset) begin
            state <= S00;    // Initialize to state 00
            count <= 0;      // Reset counter
        end else begin
            // State timer management
            if (count < 9)
                count <= count + 1;
            else begin
                count <= 0;
                state <= state + 1; // Move to the next state
            end
        end
    end

    always @(state or count) begin
        // Default signals: All red
        MR1 = 3'b100;  // Red
        MR2 = 3'b100;  // Red
        MR3 = 3'b100;  // Red

        case (state)
            S00: begin
                if (count < 5)
                    MR1 = 3'b100; // Red
                else
                    MR1 = 3'b010; // Yellow
            end

            S01: begin
                if (count < 5)
                    MR1 = 3'b001; // Green
                else begin
                    MR1 = 3'b010; // Yellow
                    MR2 = 3'b010; // Yellow
                end
            end

            S10: begin
                if (count < 5)
                    MR2 = 3'b001; // Green
                else begin
                    MR2 = 3'b010; // Yellow
                    MR3 = 3'b010; // Yellow
                end
            end

            S11: begin
                if (count < 5)
                    MR3 = 3'b001; // Green
                else
                    MR3 = 3'b010; // Yellow
            end
        endcase
    end
endmodule
```

**RTL Schematic**

![Screenshot 2024-12-23 220534](https://github.com/user-attachments/assets/d4f4ff25-00cc-49b8-833e-7674b7442265)


**Output Timing Waveform**

![Screenshot 2024-12-23 220729](https://github.com/user-attachments/assets/830cc4fa-b673-405f-881d-46851555e1a5)

**Result:**

Thus the Verilog code has been implemented successfully.



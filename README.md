# Exp-6b-Traffic-Light-Controller
# Aim
To design and simulate a Verilog HDL for Traffic Light Controler

# Apparatus Required
Vivado 2023.1
Spartan 7 FPGA
# Procedure
1. Launch Vivado 2023.1
Open Vivado 2023.1 and create a new project.
Select RTL Project and enable Do not specify sources at this time.
2. Create Verilog Design File
Create a new Verilog design file.
Type the Verilog code for the Traffic Light Controler

3. Observe the Output
Observe the Traffic Signal output.

# Block Diagram(Schematic)

<img width="1324" height="622" alt="image" src="https://github.com/user-attachments/assets/30f7dda7-edea-4f95-8c75-7207383ca65d" />


# Code

```
module traffic_light_controller(
    input clk, rst,
    output reg [2:0] light
);
    parameter [1:0] RED = 2'b00, GREEN = 2'b01, YELLOW = 2'b10;
    reg [1:0] state;
    reg [3:0] count;

    always @(posedge clk or posedge rst) begin
        if (rst) begin
            state <= RED;
            count <= 0;
        end
        else begin
            count <= count + 1;
            case (state)
                RED: begin
                    if (count == 3) begin
                        state <= GREEN;
                        count <= 0;
                    end
                end
                GREEN: begin
                    if (count == 5) begin
                        state <= YELLOW;
                        count <= 0;
                    end
                end
                YELLOW: begin
                    if (count == 1) begin
                        state <= RED;
                        count <= 0;
                    end
                end
            endcase
        end
    end

    always @(*) begin
        case (state)
            RED: light = 3'b100;
            YELLOW: light = 3'b010;
            GREEN: light = 3'b001;
            default: light = 3'b000;
        endcase
    end
endmodule
```

# Test Bench

```
module tb_traffic_light;
    reg clk, rst;
    wire [2:0] light;

    traffic_light_controller uut (.clk(clk), .rst(rst), .light(light));

    initial begin
        clk = 0;
        forever #5 clk = ~clk;
    end

    initial begin
        rst = 1;
        #10 rst = 0;
        #300 $finish;
    end

    initial begin
        $monitor("Time=%0t | Light={Red,Yellow,Green}=%b", $time, light);
    end
endmodule
```

# Output

<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/09aeaf4f-f956-4633-bef6-5a8be9f3a4fa" />


# Result

The Verilog code for the Traffic Light Controller was designed and simulated successfully.
The output verified correct traffic light operation.


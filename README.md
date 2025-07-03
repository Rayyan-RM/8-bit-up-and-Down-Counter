# 8-bit-up-and-Down-Counter
This is a project for VLSI subject


Schemetric -

![image](https://github.com/user-attachments/assets/22166a21-a7ea-40a3-844c-aac178936771)


Verilog Code -

    // File: up_down_counter.v
    module up_down_counter (
        input wire clk,         // Clock signal
        input wire rst_n,       // Asynchronous active-low reset
        input wire enable,      // Enable signal
        input wire up_down,     // Direction control: 1 = up, 0 = down
        output reg [7:0] count  // 8-bit counter output
    );

              // Asynchronous Reset, Synchronous Operation
              always @(posedge clk or negedge rst_n) begin
                  if (!rst_n)
                      count <= 8'b0;  // Reset to 0
                  else if (enable) begin
                      if (up_down)
                          count <= count + 1;  // Count up
                      else
                          count <= count - 1;  // Count down
                  end
              end

    endmodule


Test Bench File Here -
          
    module tb_up_down_counter;
    
        reg clk;
        reg rst_n;
        reg enable;
        reg up_down;
        wire [7:0] count;
    
        up_down_counter uut (
            .clk(clk),
            .rst_n(rst_n),
            .enable(enable),
            .up_down(up_down),
            .count(count)
        );
    
        // Clock generation
        initial clk = 0;
        always #5 clk = ~clk;
    
        // Stimulus
        initial begin
            // Initialize
            rst_n = 0; enable = 0; up_down = 1;
    
            #12 rst_n = 1;
            #10 enable = 1;
            #60 up_down = 0;
            #40 enable = 0;
            #20 enable = 1;
            #30 rst_n = 0;
            #10 rst_n = 1;
            #50 $finish;
        end
    
        // Monitor
        initial begin
            $vcdpluson;  // Enable waveform for Verdi
            $monitor("Time = %0t | rst_n = %b | enable = %b | up_down = %b | count = %d",
                     $time, rst_n, enable, up_down, count);
        end
    
    endmodule
    
    
        

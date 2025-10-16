# 21-21-keyboard-controller
module keyboard_controller (
    input clk,
    output reg [20:0] row,   // 21 rows
    input  [20:0] col,    // 21 columns
    output reg [8:0] key_code,
    // output key code (0–440)
    output reg key_pressed
);
    reg [4:0] current_row = 0;   // row counter 0–20
    integer i;

  always @(posedge clk) begin
        // Activate one row at a time
   row=21'b111111111111111111111; 
   row[current_row] = 0; // drive current row low

  // Read all column inputs
        key_pressed = 0;
        for (i = 0; i < 21; i = i + 1) begin
            if (col[i] == 0) begin
                key_pressed = 1;
                key_code = current_row * 21 + i; // unique key code
            end
        end

   // Move to next row
  current_row = (current_row == 20) ? 0 : current_row + 1;
    end
endmodule

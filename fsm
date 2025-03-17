module FSM_pv(
    input KEY0, SW0, SW1, SW2, SW3, SW4, 
    output logic [6:0] SEG0, SEG1, SEG2, SEG3, 
    output logic [6:0] LED_SW
);

    // Add param to define whether the button is active-low or active-high...
    parameter ACTIVE_LOW_RESET = 1; // Set to 1 if KEY0 is active-low... 0 if active-high

    // Adjusts reset signal based on the param instead of blindly inverting with ~
    wire RESET;
    assign RESET = (ACTIVE_LOW_RESET) ? ~KEY0 : KEY0;

    reg [7:0] Message[3:0];
    logic [2:0] state;
    logic [1:0] Z;

    
    FSM FSMachine(RESET, SW0, SW1, SW2, SW3, SW4, state, Z);

    // ASCII to 7-Segment display conversion
    ASCII27Seg SevH3(Message[3], SEG3);
    ASCII27Seg SevH2(Message[2], SEG2);
    ASCII27Seg SevH1(Message[1], SEG1);
    ASCII27Seg SevH0(Message[0], SEG0);

    always_comb begin
        // Default message
        Message[3] = "C";
        Message[2] = "l";
        Message[1] = "a";
        Message[0] = "r";

        // LED control obviosuly
        LED_SW[6] = Z[1];
        LED_SW[5] = Z[0];
        LED_SW[4] = SW4;
        LED_SW[3] = SW3;
        LED_SW[2] = SW2;
        LED_SW[1] = SW1;
        LED_SW[0] = SW0;

        // State-dependent message
        case(state)
            3'b000: begin
                Message[3] = "C";
                Message[2] = "l";
                Message[1] = "a";
                Message[0] = "r";
            end
            3'b001: begin
                Message[3] = "S";
                Message[2] = "_";
                Message[1] = "0";
                Message[0] = "1";
            end
            3'b010: begin
                Message[3] = "S";
                Message[2] = "_";
                Message[1] = "0";
                Message[0] = "2";
            end
            3'b011: begin
                Message[3] = "S";
                Message[2] = "_";
                Message[1] = "0";
                Message[0] = "3";
            end
            3'b100: begin
                Message[3] = "S";
                Message[2] = "_";
                Message[1] = "0";
                Message[0] = "4";
            end
            default: begin
                Message[3] = "C";
                Message[2] = "l";
                Message[1] = "a";
                Message[0] = "r";
            end
        endcase  
    end

endmodule // FSM_pv





// Adding test bench below. Copy and paste elsewhere...

`timescale 1ns/1ps

module FSM_pv_tb;
    // Testbench signals
    reg KEY0, SW0, SW1, SW2, SW3, SW4;
    wire [6:0] SEG0, SEG1, SEG2, SEG3;
    wire [6:0] LED_SW;

    // Instantiate the FSM_pv module
    FSM_pv uut (
        .KEY0(KEY0), 
        .SW0(SW0), 
        .SW1(SW1), 
        .SW2(SW2), 
        .SW3(SW3), 
        .SW4(SW4),
        .SEG0(SEG0), 
        .SEG1(SEG1), 
        .SEG2(SEG2), 
        .SEG3(SEG3),
        .LED_SW(LED_SW)
    );

    // Test sequence
    initial begin
        // Initialize inputs
        KEY0 = 1; SW0 = 0; SW1 = 0; SW2 = 0; SW3 = 0; SW4 = 0;
        #10; // Wait for stabilization

        // Reset the FSM
        KEY0 = 0; #10; // 
        KEY0 = 1; #10; // 

        // Then test
        SW1 = 1; #10; SW1 = 0; #10;
        SW2 = 1; #10; SW2 = 0; #10;
        SW3 = 1; #10; SW3 = 0; #10;
        SW1 = 1; #10; SW1 = 0; #10;
        SW0 = 1; #10; SW0 = 0; #10; // Reset here at the end

        // All done wrap things up
        $finish;
    end

    
    initial begin
        $monitor("Time: %0t | KEY0: %b | SW: %b%b%b%b%b | State: %b | LED_SW: %b", 
            $time, KEY0, SW4, SW3, SW2, SW1, SW0, uut.state, LED_SW);
    end
endmodule

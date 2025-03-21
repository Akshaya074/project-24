8 BIT ALU
module ALU_8bit(
    input [7:0] A,       // 8-bit input A
    input [7:0] B,       // 8-bit input B
    input [2:0] ALUOp,   // 3-bit control signal for operation selection
    output reg [7:0] Result,  // 8-bit ALU result
    output reg Zero,      // Zero flag (indicates if result is zero)
    output reg CarryOut,  // Carry-out flag (for addition)
    output reg Overflow   // Overflow flag (for signed operations)
);

// ALU operation control signals (for ALUOp)
parameter ADD  = 3'b000;
parameter SUB  = 3'b001;
parameter AND  = 3'b010;
parameter OR   = 3'b011;
parameter XOR  = 3'b100;
parameter SLT  = 3'b101;  // Set Less Than (signed comparison)

always @(*) begin
    case (ALUOp)
        ADD: begin
            {CarryOut, Result} = A + B;  // Addition with carry out
            Overflow = (A[7] == B[7]) && (Result[7] != A[7]);  // Signed overflow detection
            Zero = (Result == 8'b0);  // Zero flag
        end
        SUB: begin
            {CarryOut, Result} = A - B;  // Subtraction with carry out (borrow)
            Overflow = (A[7] != B[7]) && (Result[7] != A[7]);  // Signed overflow detection
            Zero = (Result == 8'b0);  // Zero flag
        end
        AND: begin
            Result = A & B;  // Bitwise AND
            Zero = (Result == 8'b0);  // Zero flag
            CarryOut = 1'b0;  // No carry for AND operation
            Overflow = 1'b0;  // No overflow for AND operation
        end
        OR: begin
            Result = A | B;  // Bitwise OR
            Zero = (Result == 8'b0);  // Zero flag
            CarryOut = 1'b0;  // No carry for OR operation
            Overflow = 1'b0;  // No overflow for OR operation
        end
        XOR: begin
            Result = A ^ B;  // Bitwise XOR
            Zero = (Result == 8'b0);  // Zero flag
            CarryOut = 1'b0;  // No carry for XOR operation
            Overflow = 1'b0;  // No overflow for XOR operation
        end
        SLT: begin
            Result = (A < B) ? 8'b1 : 8'b0;  // Set Less Than (signed comparison)
            Zero = (Result == 8'b0);  // Zero flag
            CarryOut = 1'b0;  // No carry for SLT operation
            Overflow = 1'b0;  // No overflow for SLT operation
        end
        default: begin
            Result = 8'b0;  // Default case
            Zero = 1'b0;    // Zero flag
            CarryOut = 1'b0;  // No carry
            Overflow = 1'b0;  // No overflow
        end
    endcase
end

endmodule

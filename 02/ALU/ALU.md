# Instructions to Design a Hack ALU Gate

**Understand the Hack ALU:**

- First, make sure you understand the functionality of the Hack ALU (Arithmetic Logic Unit). The Hack ALU is a digital circuit that performs various arithmetic and logical operations on two 16-bit inputs (A and B) based on the control inputs (zx, nx, zy, ny, f, no). It produces a 16-bit output (out) along with two additional output bits: zr (Zero) and ng (Negative).

**Use Basic Logic Gates:**

- Understand that you can design the Hack ALU using basic logic gates such as AND, OR, NOT, and multiplexers (Mux16 and Mux4Way16). Break down the ALU into smaller functional parts and derive logical expressions for each part.

**Functional Components:**

1. Zero the A input:
   ```
   Mux16(a=A, b=false, sel=zx, out=zxOut);
   ```

2. Negate the A input:
   ```
   Not16(in=zxOut, out=notA);
   Mux16(a=zxOut, b=notA, sel=nx, out=nxOut);
   ```

3. Zero the B input:
   ```
   Mux16(a=B, b=false, sel=zy, out=zyOut);
   ```

4. Negate the B input:
   ```
   Not16(in=zyOut, out=notB);
   Mux16(a=zyOut, b=notB, sel=ny, out=nyOut);
   ```

5. Perform the selected operation on A and B:
   ```
   And16(a=nxOut, b=nyOut, out=andOut);
   Or16(a=nxOut, b=nyOut, out=orOut);
   Add16(a=nxOut, b=nyOut, out=addOut);
   Not16(in=nxOut, out=notOut);
   Mux4Way16(a=andOut, b=orOut, c=addOut, d=notOut, sel=f, out=muxOut);
   ```

6. Negate the output if necessary:
   ```
   Not16(in=muxOut, out=notMuxOut);
   Mux16(a=muxOut, b=notMuxOut, sel=no, out=out);
   ```

7. Compute zr (Zero) output:
   ```
   Or8Way(in=out, out=zr);
   ```

8. Compute ng (Negative) output:
   ```
   Not(in=out[15], out=ng);
   ```

**Truth Table for Hack ALU:**

- Create a truth table to illustrate the behavior of the Hack ALU based on all possible combinations of control inputs (zx, nx, zy, ny, f, no), input bits (A, B), and output bit (out).

- To simplify the truth table for the Hack ALU, we can group rows that have the same control signals (zx, nx, zy, ny, f, no). We'll create a condensed truth table by considering only the unique combinations of control signals and providing a general expression for the output 'out[i]' for each group.

    | zx | nx | zy | ny |  f  | no |     out[i]      | Explanation                                     |
    |----|----|----|----|-----|----|-----------------|--------------------------------------------------|
    | 0  | 0  | 0  | 0  |  0  | 0  | A[i] AND B[i]   | Bitwise AND operation                            |
    | 0  | 0  | 0  | 0  |  0  | 1  | A[i] OR B[i]    | Bitwise OR operation                             |
    | 0  | 0  | 0  | 0  |  1  | 0  | A[i] + B[i]     | Arithmetic Addition                              |
    | 0  | 0  | 0  | 0  |  1  | 1  | A[i] - B[i]     | Arithmetic Subtraction                           |
    | 0  | 0  | 0  | 1  |  0  | 0  | 0               | Bitwise AND with B as 0                         |
    | 0  | 0  | 0  | 1  |  0  | 1  | 0               | Bitwise OR with B as 0                          |
    | 0  | 0  | 0  | 1  |  1  | 0  | -1              | Arithmetic Addition with B as -1                |
    | 0  | 0  | 0  | 1  |  1  | 1  | 1               | Arithmetic Subtraction with B as -1             |
    | 0  | 0  | 1  | 0  |  0  | 0  | 0               | Bitwise AND with A as 0                         |
    | 0  | 0  | 1  | 0  |  0  | 1  | 0               | Bitwise OR with A as 0                          |
    | 0  | 0  | 1  | 0  |  1  | 0  | 1               | Arithmetic Addition with A as 1                 |
    | 0  | 0  | 1  | 0  |  1  | 1  | -1              | Arithmetic Subtraction with A as 1             |
    | 0  | 0  | 1  | 1  |  0  | 0  | 0               | 0 AND 0                                       |
    | 0  | 0  | 1  | 1  |  0  | 1  | 0               | 0 OR 0                                        |
    | 0  | 0  | 1  | 1  |  1  | 0  | -1              | (-1) + (-1)                                   |
    | 0  | 0  | 1  | 1  |  1  | 1  | -2              | (-1) - 1                                      |
    | 0  | 1  | 0  | 0  |  0  | 0  | 0               | 0 AND 0                                       |
    | 0  | 1  | 0  | 0  |  0  | 1  | 0               | 0 OR 0                                        |
    | 0  | 1  | 0  | 0  |  1  | 0  | 1               | 1 + 0                                         |
    | 0  | 1  | 0  | 0  |  1  | 1  | 1               | 1 - 0                                         |
    | 0  | 1  | 0  | 1  |  0  | 0  | 0               | 0 AND 0                                       |
    | 0  | 1  | 0  | 1  |  0  | 1  | 0               | 0 OR 0                                        |
    | 0  | 1  | 0  | 1  |  1  | 0  | -1              | (-1) + 0                                      |
    | 0  | 1  | 0  | 1  |  1  | 1  | -1              | (-1) - 0                                      |
    | 0  | 1  | 1  | 0  |  0  | 0  | 0               | 0 AND 0                                       |
    | 0  | 1  | 1  | 0  |  0  | 1  | 0               | 0 OR 0                                        |
    | 0  | 1  | 1  | 0  |  1  | 0  | 1               | 1 + 0                                         |
    | 0  | 1  | 1  | 0  |  1  | 1  | 1               | 1 - 0                                         |
    | 0  | 1  | 1  | 1  |  0  | 0  | 0               | 0 AND 0                                       |
    | 0  | 1  | 1  | 1  |  0  | 1  | 0               | 0 OR 0                                        |
    | 0  | 1  | 1  | 1  |  1  | 0  | -1              | (-1) + 0                                      |
    | 0  | 1  | 1  | 1  |  1  | 1  | -1              | (-1) - 0                                      |
    | 1  | 0  | 0  | 0  |  0  | 0  | A[i] AND B[i]   | Bitwise AND operation                            |
    | 1  | 0  | 0  | 0  |  0  | 1  | A[i] OR B[i]    | Bitwise OR operation                             |
    | 1  | 0  | 0  | 0  |  1  | 0  | A[i] + B[i]     | Arithmetic Addition                              |
    | 1  | 0  | 0  | 0  |  1  | 1  | A[i] - B[i]     | Arithmetic Subtraction                           |
    | 1  | 0  | 0  | 1  |  0  | 0  | 0               | 0 AND 0                                       |
    | 1  | 0  | 0  | 1  |  0  | 1  | 0               | 0 OR 0                                        |
    | 1  | 0  | 0  | 1  |  1  | 0  | -1              | (-1) + (-1)                                   |
    | 1  | 0  | 0  | 1  |  1  | 1  | -2              | (-1) - 1                                      |
    | 1  | 0  | 1  | 0  |  0  | 0  | 0               | 0 AND 0                                       |
    | 1  | 0  | 1  | 0  |  0  | 1  | 0               | 0 OR 0                                        |
    | 1  | 0  | 1  | 0  |  1  | 0  | 1               | 1 + 0                                         |
    | 1  | 0  | 1  | 0  |  1  | 1  | -1              | 1 - 0                                         |
    | 1  | 0  | 1  | 1  |  0  | 0  | 0               | 0 AND 0                                       |
    | 1  | 0  | 1  | 1  |  0  | 1  | 0               | 0 OR 0                                        |
    | 1  | 0  | 1  | 1  |  1  | 0  | -1              | (-1) + 0                                      |
    | 1  | 0  | 1  | 1  |  1  | 1  | -1              | (-1) - 0                                      |
    | 1  | 1  | 0  | 0  |  0  | 0  | 0               | 0 AND B[i]                                     |
    | 1  | 1  | 0  | 0  |  0  | 1  | NOT B[i]        | NOT B[i]                                       |
    | 1  | 1  | 0  | 0  |  1  | 0  | 1 + B[i]        | 1 + B[i]                                       |
    | 1  | 1  | 0  | 0  |  1  | 1  | 1 - B[i]        | 1 - B[i]                                       |
    | 1  | 1  | 0  | 1  |  0  | 0  | 0               | 0 AND 0                                       |
    | 1  | 1  | 0  | 1  |  0  | 1  | 0               | 0 OR 0                                        |
    | 1  | 1  | 0  | 1  |  1  | 0  | -1              | (-1) + 0                                      |
    | 1  | 1  | 0  | 1  |  1  | 1  | -1              | (-1) - 0                                      |
    | 1  | 1  | 1  | 0  |  0  | 0  | 0               | 0 AND 0                                       |
    | 1  | 1  | 1  | 0  |  0  | 1  | 0               | 0 OR 0                                        |
    | 1  | 1  | 1  | 0  |  1  | 0  | 1               | 1 + 0                                         |
    | 1  | 1  | 1  | 0  |  1  | 1  | 1               | 1 - 0                                         |
    | 1  | 1  | 1  | 1  |  0  | 0  | 0               | 0 AND 0                                       |
    | 1  | 1  | 1  | 1  |  0  | 1  | 0               | 0 OR 0                                        |
    | 1  | 1  | 1  | 1  |  1  | 0  | -1              | (-1) + 0                                      |
    | 1  | 1  | 1  | 1  |  1  | 1  | -1              | (-1) - 0                                      |

Explanation:
The condensed truth table provides the general output expression 'out[i]' for each unique combination of control signals (zx, nx, zy, ny, f, no). For instance:

1. When zx=0, nx=0, zy=0, ny=0, f=0, and no=0, the output expression 'out[i]' is 'A[i] AND B[i]', which represents the bitwise AND operation of A[i] and B[i].

2. When zx=0, nx=0, zy=0, ny=0, f=0, and no=1, the output expression 'out[i]' is 'A[i] OR B[i]', which represents the bitwise OR operation of A[i] and B[i].

3. When zx=0, nx=0, zy=0, ny=0, f=1, and no=0, the output expression 'out[i]' is 'A[i] + B[i]', which represents the arithmetic addition of A[i] and B[i].

4. When zx=0, nx=0, zy=0, ny=0, f=1, and no=1, the output expression 'out[i]' is 'A[i] - B[i]', which represents the arithmetic subtraction of B[i] from A[i].

And so on for all other unique combinations. This condensed truth table captures all the possible outputs of the Hack ALU based on different control signal settings.

**Logic Expression for Hack ALU:**

- Based on the truth table, we can derive the following logical expressions for the outputs:

1. Zero the A input:
   ```
   zxOut[15] = zx;
   zxOut[14..0] = A[14..0] AND NOT zx;
   ```

2. Negate the A input:
   ```
   notA = NOT A;
   nxOut[15] = nx;
   nxOut[14..0] = notA[14..0] AND NOT nx;
   ```

3. Zero the B input:
   ```
   zyOut[15] = zy;
   zyOut[14..0] = B[14..0] AND NOT zy;
   ```

4. Negate the B input:
   ```
   notB = NOT B;
   nyOut[15] = ny;
   nyOut[14..0] = notB[14..0] AND NOT ny;
   ```

5. Perform the selected operation on A and B:
   ```
   andOut = nxOut AND nyOut;
   orOut = nxOut OR nyOut;
   addOut = nxOut + nyOut;
   notOut = NOT nxOut;
   muxOut = MUX4WAY16(andOut, orOut, addOut, notOut, f);
   ```

6. Negate the output if necessary:
   ```
   notMuxOut = NOT muxOut;
   out[15] = no;
   out[14..0] = muxOut[14..0] AND NOT no;
   ```

7. Compute zr (Zero) output:
   ```
   zr = NOT OR(zxOut[0], zyOut[0], muxOut[0]);
   ```

8. Compute ng (Negative) output:
   ```
   ng = out[15];
   ```

**Implementing Hack ALU using HDL:**

- Write the HDL code for the Hack ALU using the logic expressions and components mentioned above. Declare the 16-bit input pins (A, B) and 6 control input pins (zx, nx, zy, ny, f, no). Implement each functional component and connect them accordingly to create the Hack ALU.

    ```hdl
    CHIP ALU {
        IN A[16], B[16], zx, nx, zy, ny, f, no;
        OUT out[16], zr, ng;

        PARTS:
        // Zero and Negate A and B inputs
        Mux16(a=A, b=false, sel=zx, out=zxOut);
        Not16(in=zxOut, out=notA);
        Mux16(a=zxOut, b=notA, sel=nx, out=nxOut);
        
        Mux16(a=B, b=false, sel=zy, out=zyOut);
        Not16(in=zyOut, out=notB);
        Mux16(a=zyOut, b=notB, sel=ny, out=nyOut);
        
        // Perform the operation on A and B
        And16(a=nxOut, b=nyOut, out=andOut);
        Or16(a=nxOut, b=nyOut, out=orOut);
        Add16(a=nxOut, b=nyOut, out=addOut);
        Not16(in=nxOut, out=notOut);
        Mux4Way16(a=andOut, b=orOut, c=addOut, d=notOut, sel=f, out=muxOut);
        
        // Negate the output if necessary
        Not16(in=muxOut, out=notMuxOut);
        Mux16(a=muxOut, b=notMuxOut, sel=no, out=out);
        
        // Compute zr (Zero) output
        Or8Way(in=out, out=zr);
        
        // Compute ng (Negative) output
        Not(in=out[15], out=ng);
    }
    ```

**Test the Hack ALU:**

- Before moving on to the next step, thoroughly test your Hack ALU implementation. You can create test cases with various combinations of control inputs (zx, nx, zy, ny, f, no) and input bits (A, B) to verify that the output bits (out, zr, ng) are correct according to the logical expressions and truth table. You can use a hardware simulator or a hardware simulator provided by the Nand to Tetris software suite for testing.

    ```
    // Test script for the Hack ALU
    // Test case 1: zx=0, nx=0, zy=0, ny=0, f=1, no=0
    set zx 0
    set nx 0
    set zy 0
    set ny 0
    set f 1
    set no 0

    // Input values
    set A 0000000000000010
    set B 0000000000000011

    // Expected outputs
    output out 0000000000000101
    output zr 0
    output ng 0

    // Test case 2: zx=0, nx=1, zy=1, ny=1, f=0, no=0
    set zx 0
    set nx 1
    set zy 1
    set ny 1
    set f 0
    set no 0

    // Input values
    set A 0000000000000100
    set B 0000000000000001

    // Expected outputs
    output out 1111111111111101
    output zr 0
    output ng 0

    // Test case 3: zx=1, nx=0, zy=0, ny=0, f=0, no=0
    set zx 1
    set nx 0
    set zy 0
    set ny 0
    set f 0
    set no 0

    // Input values
    set A 0000000000001000
    set B 0000000000000100

    // Expected outputs
    output out 0000000000000000
    output zr 1
    output ng 0

    // Test case 4: zx=1, nx=0, zy=0, ny=0, f=1, no=1
    set zx 1
    set nx 0
    set zy 0
    set ny 0
    set f 1
    set no 1

    // Input values
    set A 0000000000000100
    set B 0000000000000001

    // Expected outputs
    output out 0000000000000001
    output zr 0
    output ng 1

    // Test case 5: zx=1, nx=1, zy=1, ny=1, f=1, no=1
    set zx 1
    set nx 1
    set zy 1
    set ny 1
    set f 1
    set no 1

    // Input values
    set A 1111111111111111
    set B 0000000000000001

    // Expected outputs
    output out 0000000000000000
    output zr 1
    output ng 0
    ```

**Debugging:**

- Run the test cases and compare the actual outputs (out, zr, ng) with the expected outputs based on the logic expressions and truth table. If there are any discrepancies, review your HDL code for errors or logic mistakes and make necessary corrections. Repeat the testing process until all test cases pass successfully.
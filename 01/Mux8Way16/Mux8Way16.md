# Instructions to Design an 8-Way 16-Bit Multiplexer (Mux8Way16)

**Understand the 8-Way 16-Bit Multiplexer (Mux8Way16):**

- First, make sure you understand the functionality of an 8-way 16-bit multiplexer (Mux8Way16). A multiplexer takes multiple inputs and selects one of them as the output based on the control inputs. In this case, the 8-way 16-bit multiplexer has eight 16-bit data inputs (a, b, c, d, e, f, g, and h) and three 1-bit control inputs (sel[2], sel[1], and sel[0]). It has one 16-bit output (out), and based on the control inputs, one of the data inputs will be directed to the output.

**Use the Mux16 Gate:**

- Understand that you can use the Mux16 gate as the building block to implement the 8-way 16-bit multiplexer. The Mux16 gate takes two 16-bit data inputs and a 1-bit control input and produces a 16-bit output.

**Logic Expression for the 8-Way Mux16:**

- The logic expression for the 8-way 16-bit multiplexer is as follows:

    out = (sel[2] AND sel[1] AND sel[0] AND h) OR (sel[2] AND sel[1] AND NOT sel[0] AND g) OR (sel[2] AND NOT sel[1] AND sel[0] AND f) OR (sel[2] AND NOT sel[1] AND NOT sel[0] AND e) OR (NOT sel[2] AND sel[1] AND sel[0] AND d) OR (NOT sel[2] AND sel[1] AND NOT sel[0] AND c) OR (NOT sel[2] AND NOT sel[1] AND sel[0] AND b) OR (NOT sel[2] AND NOT sel[1] AND NOT sel[0] AND a)

**Truth Table for the 8-Way Mux16:**

- Create a truth table for the 8-way 16-bit multiplexer, considering all possible input combinations for the data inputs (a, b, c, d, e, f, g, and h) and control inputs (sel[2], sel[1], sel[0]). Since there are three control inputs, there will be eight rows in the truth table.

    | sel[2] | sel[1] | sel[0] | out[15:0] |
    |--------|--------|--------|-----------|
    |   0    |   0    |   0    |   a[15:0] |
    |   0    |   0    |   1    |   b[15:0] |
    |   0    |   1    |   0    |   c[15:0] |
    |   0    |   1    |   1    |   d[15:0] |
    |   1    |   0    |   0    |   e[15:0] |
    |   1    |   0    |   1    |   f[15:0] |
    |   1    |   1    |   0    |   g[15:0] |
    |   1    |   1    |   1    |   h[15:0] |

**Implementing the 8-Way Mux16 using HDL:**

- Write the HDL code for the 8-way 16-bit multiplexer using the Mux16 gate as the building block. Declare the 16-bit data inputs (a, b, c, d, e, f, g, and h), 3-bit control inputs (sel[2], sel[1], sel[0]), and the 16-bit output (out) pins. Implement the Mux16 gate for each output line and connect them accordingly based on the control inputs.

    ```hdl
    CHIP Mux8Way16 {
        IN a[16], b[16], c[16], d[16], e[16], f[16], g[16], h[16], sel[3];
        OUT out[16];

        PARTS:
        // Implement two Mux4Way16 sub-circuits to select two groups of 4 data inputs each.
        Mux4Way16(a=a, b=b, c=c, d=d, sel=sel[0..1], out=group1);
        Mux4Way16(a=e, b=f, c=g, d=h, sel=sel[0..1], out=group2);
    
        // Use the 16-bit multiplexer (Mux16) to select between group1 and group2.
        Mux16(a=group1, b=group2, sel=sel[2], out=out);
    }
    ```

**Test the 8-Way Mux16:**

- Before moving on to the next step, it's essential to test your 8-way 16-bit multiplexer implementation thoroughly. You can do this by using a hardware simulator or a hardware simulator provided by the Nand to Tetris software suite.
- Create test cases by setting different data inputs (a, b, c, d, e, f, g, and h) and control inputs (sel[2], sel[1], sel[0]) combinations, and observe the corresponding output values (out). Verify that the 8-way 16-bit multiplexer produces the correct output based on the control inputs.

    ```
    // Test script for the 8-way 16-bit multiplexer (Mux8Way16)
    // Test case 1: sel=000, a=0000000000000001, b=1111111111111111, c=1010101010101010, d=0101010101010101, e=1111111100000000, f=0000000011111111, g=0101010101010101, h=1010101010101010
    set a 0000000000000001
    set b 1111111111111111
    set c 1010101010101010
    set d 0101010101010101
    set e 1111111100000000
    set f 0000000011111111
    set g 0101010101010101
    set h 1010101010101010
    set sel[2] 0
    set sel[1] 0
    set sel[0] 0
    eval
    output out 0000000000000001 // Expected output is a

    // Test case 2: sel=001, a=0000000000000000, b=1111111111111111, c=1010101010101010, d=0101010101010101, e=1111111100000000, f=0000000011111111, g=0101010101010101, h=1010101010101010
    set a 0000000000000000
    set b 1111111111111111
    set c 1010101010101010
    set d 0101010101010101
    set e 1111111100000000
    set f 0000000011111111
    set g 0101010101010101
    set h 1010101010101010
    set sel[2] 0
    set sel[1] 0
    set sel[0] 1
    eval
    output out 1111111111111111 // Expected output is b

    // Test case 3: sel=010, a=0000000000000000, b=1111111111111111, c=1010101010101010, d=0101010101010101, e=1111111100000000, f=0000000011111111, g=0101010101010101, h=1010101010101010
    set a 0000000000000000
    set b 1111111111111111
    set c 1010101010101010
    set d 0101010101010101
    set e 1111111100000000
    set f 0000000011111111
    set g 0101010101010101
    set h 1010101010101010
    set sel[2] 0
    set sel[1] 1
    set sel[0] 0
    eval
    output out 1010101010101010 // Expected output is c

    // Test case 4: sel=011, a=0000000000000000, b=1111111111111111, c=1010101010101010, d=0101010101010101, e=1111111100000000, f=0000000011111111, g=0101010101010101, h=1010101010101010
    set a 0000000000000000
    set b 1111111111111111
    set c 1010101010101010
    set d 0101010101010101
    set e 1111111100000000
    set f 0000000011111111
    set g 0101010101010101
    set h 1010101010101010
    set sel[2] 0
    set sel[1] 1
    set sel[0] 1
    eval
    output out 0101010101010101 // Expected output is d

    // Continue with the remaining test cases for sel=100, sel=101, sel=110, and sel=111
    ```

**Debugging:**

- Run the test script and compare the actual outputs (out) with the expected outputs based on the test cases. If there are any discrepancies, review your HDL code for errors or logic mistakes.
- Check the connection between the Mux16 gate and the 4-way 16-bit multiplexers (Mux4Way16) to ensure they are correct for each control input combination.

Once the 8-way 16-bit multiplexer passes all test cases, you have successfully designed and implemented the Mux8Way16 gate using a combination of a 16-bit multiplexer and two 4-way 16-bit multiplexers. Congratulations! Now, you have an 8-way 16-bit multiplexer built from smaller, modular sub-circuits.
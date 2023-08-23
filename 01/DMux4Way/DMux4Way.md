# Instructions to Design a 4-Way Demultiplexer (DMux4Way)

**Understand the 4-Way Demultiplexer (DMux4Way):**

- First, make sure you understand the functionality of a 4-way demultiplexer (DMux4Way). A demultiplexer takes a single input and selects one of the multiple output lines based on the control inputs. In this case, the 4-way demultiplexer has one 1-bit input (in) and two 2-bit control inputs (sel[1] and sel[0]). It has four output lines (a, b, c, and d), and based on the control inputs, the input value will be directed to one of the output lines.

**Use the DMux Gate:**

- Understand that you can use the basic DMux gate as the building block to implement the 4-way demultiplexer. The DMux gate has three inputs (in, sel[1], and sel[0]) and two outputs (a and b).

**Logic Expression for the 4-Way DMux:**

- The logic expression for the 4-way demultiplexer is as follows:

    a = (in AND NOT sel[1] AND NOT sel[0])

    b = (in AND NOT sel[1] AND sel[0])

    c = (in AND sel[1] AND NOT sel[0])
    
    d = (in AND sel[1] AND sel[0])

**Truth Table for the 4-Way DMux:**

- Create a truth table for the 4-way demultiplexer, considering all possible input combinations for the input (in) and control inputs (sel[1], sel[0]). Since there are two control inputs, there will be four rows in the truth table.

    | in | sel[1] | sel[0] | a | b | c | d |
    |----|--------|--------|---|---|---|---|
    | 1  |   0    |   0    | 1 | 0 | 0 | 0 |
    | 1  |   0    |   1    | 0 | 1 | 0 | 0 |
    | 1  |   1    |   0    | 0 | 0 | 1 | 0 |
    | 1  |   1    |   1    | 0 | 0 | 0 | 1 |

**Implementing the 4-Way DMux using HDL:**

- Write the HDL code for the 4-way demultiplexer using the DMux gate as the building block. Declare the 1-bit input (in), 2-bit control inputs (sel[1], sel[0]), and four 1-bit outputs (a, b, c, and d) pins. Implement the DMux gate for each output line and connect them accordingly based on the control inputs.

    ```hdl
    CHIP DMux4Way {
        IN in, sel[2];
        OUT a, b, c, d;

        PARTS:
        DMux(in=in, sel=sel[1], a=firstLevelA, b=firstLevelB);
        DMux(in=firstLevelA, sel=sel[0], a=a, b=b);
        DMux(in=firstLevelB, sel=sel[0], a=c, b=d);
    }
    ```

**Test the 4-Way DMux:**

- Before moving on to the next step, it's essential to test your 4-way demultiplexer implementation thoroughly. You can do this by using a hardware simulator or a hardware simulator provided by the Nand to Tetris software suite.
- Create test cases by setting different input (in) and control input (sel[1], sel[0]) combinations, and observe the corresponding output values (a, b, c, and d). Verify that the 4-way demultiplexer produces the correct output based on the control inputs.

    ```
    // Test script for the 4-way demultiplexer (DMux4Way)
    // Test case 1: sel=00, in=0
    set in 0
    set sel[1] 0
    set sel[0] 0
    eval
    output a 0 // Expected output is 0
    output b 0 // Expected output is 0
    output c 0 // Expected output is 0
    output d 0 // Expected output is 0

    // Test case 2: sel=01, in=1
    set in 1
    set sel[1] 0
    set sel[0] 1
    eval
    output a 1 // Expected output is 1
    output b 0 // Expected output is 0
    output c 0 // Expected output is 0
    output d 0 // Expected output is 0

    // Test case 3: sel=11, in=0
    set in 0
    set sel[1] 1
    set sel[0] 1
    eval
    output a 0 // Expected output is 0
    output b 0 // Expected output is 0
    output c 0 // Expected output is 0
    output d 0 // Expected output is 0

    // Test case 4: sel=11, in=1
    set in 1
    set sel[1] 1
    set sel[0] 1
    eval
    output a 0 // Expected output is 0
    output b 0 // Expected output is 0
    output c 0 // Expected output is 0
    output d 1 // Expected output is 1
    ```

**Debugging:**

- Run the test script and compare the actual outputs (a, b, c, and d) with the expected outputs based on the test cases. If there are any discrepancies, review your HDL code for errors or logic mistakes.
- Check the connection between the DMux gate inputs (in and sel) and the output lines (a, b, c, and d) to ensure they are correct for each control
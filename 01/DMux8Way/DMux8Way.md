# Instructions to Design a 8-Way Demultiplexer (DMux8Way)

**Understand the 8-Way Demultiplexer (DMux8Way):**

- First, make sure you understand the functionality of an 8-way demultiplexer (DMux8Way). A demultiplexer takes a single input and selects one of the multiple output lines based on the control inputs. In this case, the DMux8Way has one 1-bit input (in) and three 3-bit control inputs (sel[2], sel[1], and sel[0]). It has eight output lines (a, b, c, d, e, f, g, and h), and based on the control inputs, the input value will be directed to one of the output lines.

**Use the DMux and DMux4Way Gates:**

- Understand that you can use the DMux and DMux4Way gates provided in the HDL code to implement the 8-way demultiplexer. The DMux gate has two inputs (in, and sel) and two outputs (a and b), while The DMux4Way gate has three inputs (in, sel[1], and sel[0]) and four outputs (a, b, c, and d).

**Logic Expression for the 8-Way DMux:**

- The logic expression for the 8-way demultiplexer is as follows:

    a = (in AND NOT sel[2] AND NOT sel[1] AND NOT sel[0])

    b = (in AND NOT sel[2] AND NOT sel[1] AND sel[0])

    c = (in AND NOT sel[2] AND sel[1] AND NOT sel[0])

    d = (in AND NOT sel[2] AND sel[1] AND sel[0])

    e = (in AND sel[2] AND NOT sel[1] AND NOT sel[0])

    f = (in AND sel[2] AND NOT sel[1] AND sel[0])

    g = (in AND sel[2] AND sel[1] AND NOT sel[0])

    h = (in AND sel[2] AND sel[1] AND sel[0])

**Truth Table for the 8-Way DMux:**

- Create a truth table for the 8-way demultiplexer, considering all possible input combinations for the input (in) and control inputs (sel[2], sel[1], sel[0]). Since there are three control inputs, there will be eight rows in the truth table.

    | in | sel[2] | sel[1] | sel[0] | a | b | c | d | e | f | g | h |
    |----|--------|--------|--------|---|---|---|---|---|---|---|---|
    | 1  |   0    |   0    |   0    | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
    | 1  |   0    |   0    |   1    | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 |
    | 1  |   0    |   1    |   0    | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
    | 1  |   0    |   1    |   1    | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 |
    | 1  |   1    |   0    |   0    | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 |
    | 1  |   1    |   0    |   1    | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 0 |
    | 1  |   1    |   1    |   0    | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 0 |
    | 1  |   1    |   1    |   1    | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 |

**Implementing the 8-Way DMux using HDL:**

- Write the HDL code for the 8-way demultiplexer using the provided code as a basis. Declare the input (in) and control inputs (sel[2], sel[1], sel[0]), and the eight output pins (a, b, c, d, e, f, g, h).
- Use the DMux gate to split the input into two branches: one connected to dmux4way_out and the other to dmux4way_e.
- Use the DMux4Way gate twice to further split the branches from dmux4way_out and dmux4way_e based on the control inputs (sel[2], sel[1], sel[0]). This will create eight output branches (a, b, c, d, e, f, g, h) based on the control inputs.

    ```hdl
    CHIP DMux8Way {
        IN in, sel[3];
        OUT a, b, c, d, e, f, g, h;

        PARTS:
        DMux(in=in, sel=sel[2], a=dmux4way_out, b=dmux4way_e);
        DMux4Way(in=dmux4way_out, sel=sel[0..1], a=a, b=b, c=c, d=d);
        DMux4Way(in=dmux4way_e, sel=sel[0..1], a=e, b=f, c=g, d=h);
    }
    ```

**Test the 8-Way DMux:**

- Before moving on to the next step, it's essential to test your 8-way demultiplexer implementation thoroughly. You can do this by using a hardware simulator or a hardware simulator provided by the Nand to Tetris software suite.
- Create test cases by setting different input (in) and control input (sel[2], sel[1], sel[0]) combinations, and observe the corresponding output values (a, b, c, d, e, f, g, and h). Verify that the 8-way demultiplexer produces the correct output based on the control inputs.

    ```
    // Test script for the 8-way demultiplexer (DMux8Way)
    // Test case 1: sel=000, in=0
    set in 0
    set sel[2] 0
    set sel[1] 0
    set sel[0] 0
    eval
    output a 0 // Expected output is 0
    output b 0 // Expected output is 0
    output c 0 // Expected output is 0
    output d 0 // Expected output is 0
    output e 0 // Expected output is 0
    output f 0 // Expected output is 0
    output g 0 // Expected output is 0
    output h 0 // Expected output is 0

    // Test case 2: sel=001, in=1
    set in 1
    set sel[2] 0
    set sel[1] 0
    set sel[0] 1
    eval
    output a 1 // Expected output is 1
    output b 0 // Expected output is 0
    output c 0 // Expected output is 0
    output d 0 // Expected output is 0
    output e 0 // Expected output is 0
    output f 0 // Expected output is 0
    output g 0 // Expected output is 0
    output h 0 // Expected output is 0

    // Test case 3: sel=010, in=0
    set in 0
    set sel[2] 0
    set sel[1] 1
    set sel[0] 0
    eval
    output a 0 // Expected output is 0
    output b 0 // Expected output is 0
    output c 0 // Expected output is 0
    output d 0 // Expected output is 0
    output e 0 // Expected output is 0
    output f 0 // Expected output is 0
    output g 0 // Expected output is 0
    output h 0 // Expected output is 0

    // Test case 4: sel=011, in=1
    set in 1
    set sel[2] 0
    set sel[1] 1
    set sel[0] 1
    eval
    output a 0 // Expected output is 0
    output b 1 // Expected output is 1
    output c 0 // Expected output is 0
    output d 0 // Expected output is 0
    output e 0 // Expected output is 0
    output f 0 // Expected output is 0
    output g 0 // Expected output is 0
    output h 0 // Expected output is 0

    // Test case 5: sel=100, in=0
    set in 0
    set sel[2] 1
    set sel[1] 0
    set sel[0] 0
    eval
    output a 0 // Expected output is 0
    output b 0 // Expected output is 0
    output c 0 // Expected output is 0
    output d 0 // Expected output is 0
    output e 0 // Expected output is 0
    output f 0 // Expected output is 0
    output g 0 // Expected output is 0
    output h 0 // Expected output is 0

    // Test case 6: sel=101, in=1
    set in 1
    set sel[2] 1
    set sel[1] 0
    set sel[0] 1
    eval
    output a 0 // Expected output is 0
    output b 0 // Expected output is 0
    output c 1 // Expected output is 1
    output d 0 // Expected output is 0
    output e 0 // Expected output is 0
    output f 0 // Expected output is 0
    output g 0 // Expected output is 0
    output h 0 // Expected output is 0

    // Test case 7: sel=110, in=0
    set in 0
    set sel[2] 1
    set sel[1] 1
    set sel[0] 0
    eval
    output a 0 // Expected output is 0
    output b 0 // Expected output is 0
    output c 0 // Expected output is 0
    output d 0 // Expected output is 0
    output e 0 // Expected output is 0
    output f 0 // Expected output is 0
    output g 0 // Expected output is 0
    output h 0 // Expected output is 0

    // Test case 8: sel=111, in=1
    set in 1
    set sel[2] 1
    set sel[1] 1
    set sel[0] 1
    eval
    output a 0 // Expected output is 0
    output b 0 // Expected output is 0 
    ```

**Debugging:**

- Run the test cases and compare the actual outputs with the expected outputs based on the truth table. If there are any discrepancies, review your HDL code for errors or logic mistakes.
- Check the connections between the components to ensure they are correct.
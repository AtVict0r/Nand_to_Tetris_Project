# Instructions to Design the DMux Gate

**Understand the DMux Gate:**

- First, make sure you understand the functionality of a DMux (Demultiplexer) gate. The DMux gate is a digital logic gate that takes two inputs: one data input (input) and one control input (sel). It produces two outputs (a and b) based on the control input (sel). When the control input (sel) is 0 (false), the data input (input) is passed to output a, and output b is set to 0. When the control input (sel) is 1 (true), the data input (input) is passed to output b, and output a is set to 0.

**Use AND and NOT Gates:**

- Understand that you can use the AND and NOT gates as foundation building blocks to implement the DMux gate. You'll be combining these gates to create the DMux gate. The NOT gate has one input and one output, while the AND gate has two inputs (A and B) and one output.

**DMux Logic Expression:**

- The DMux gate's logic expression is as follows:

  a = input AND NOT sel

  b = input AND sel

  It means that you can implement the DMux gate using the combination of AND and NOT gates as per the above expressions.

**Truth Table for the DMux Gate:**

- Begin by creating a truth table for the DMux gate. List all possible input combinations (0 and 1) for the input (input) and the control input (sel) and their corresponding outputs (a and b). Since the DMux gate has one input (input) and one control input (sel), there will be four rows in the truth table.

    | Input (input) | Control (sel) | Output a | Output b |
    |---------------|---------------|----------|----------|
    |       0       |       0       |    0     |    0     |
    |       0       |       1       |    0     |    0     |
    |       1       |       0       |    1     |    0     |
    |       1       |       1       |    0     |    1     |

**Implementing the DMux Gate using HDL:**

- Write the HDL code for the DMux gate using the AND and NOT gates as building blocks. Declare the input and control pins and the output pins, implement the AND and NOT gates as per the DMux logic expressions, and connect them accordingly.

    ```hdl
    CHIP DMux {
        IN input, sel;
        OUT a, b;

        PARTS:
        Not(in=sel, out=notSel);
        And(a=input, b=notSel, out=a);
        And(a=input, b=sel, out=b);
    }
    ```

**Test the DMux Gate:**

- Before moving on to the next step, it's essential to test your DMux gate implementation thoroughly. You can do this by using a hardware simulator or a hardware simulator provided by the Nand to Tetris software suite.
- Create test cases for all possible input combinations (00, 01, 10, and 11) for the input (input) and the control input (sel) and observe the outputs (a and b). Verify that the DMux gate produces the correct outputs based on the truth table.

    ```
    // Test script for the DMux gate
    // Test case 1: Input=0, Control=0
    set input 0
    set sel 0
    eval
    output a 0 // Expected output is 0
    output b 0 // Expected output is 0

    // Test case 2: Input=0, Control=1
    set input 0
    set sel 1
    eval
    output a 0 // Expected output is 0
    output b 0 // Expected output is 0

    // Test case 3: Input=1, Control=0
    set input 1
    set sel 0
    eval
    output a 1 // Expected output is 1
    output b 0 // Expected output is 0

    // Test case 4: Input=1, Control=1
    set input 1
    set sel 1
    eval
    output a 0 // Expected output is 0
    output b 1 // Expected output is 1
    ```

**Debugging:**

- Run the test script and compare the actual outputs (a and b) with the expected outputs based on the truth table. If there are any discrepancies, review your HDL code for errors or logic mistakes.
- Check the connections between the gates to ensure they are correct as per the DMux logic expressions.

Once the DMux gate passes all test cases, you have successfully designed and implemented the DMux gate using only the AND and NOT gates. Congratulations! Now, you have a DMux gate built from these basic logic gates. You can continue exploring more complex circuits and logic designs in your Nand to Tetris journey. Keep up the great work!
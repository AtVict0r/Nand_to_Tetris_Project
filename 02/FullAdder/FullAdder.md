# Instructions to Design a Full Adder Gate

**Understand the Full Adder Gate:**

- First, make sure you understand the functionality of a full adder gate. A full adder is a combinational logic circuit that takes three 1-bit inputs (A, B, and Cin) and produces two outputs: the sum (S) and the carry-out (Cout). The sum output is the result of adding A, B, and Cin, while the carry-out output represents the carry that propagates to the next bit.

**Use the Half Adder and OR Gates:**

- Understand that you can use a half adder and an or gate as building blocks. The half adder takes two bits as inputs and produces two bits as outputs: the sum and the carry. The or gate takes two bits as inputs and produces one bit as output. By combining these two components, you can create a full adder that can add three bits and produce two bits: the sum and the carry out.

**Logic Expression for the Full Adder Gate:**

- The logic expressions for the full adder gate are as follows:

    Sum (S) = A XOR B XOR Cin

    Carry-out (Cout) = (A AND B) OR ((A XOR B) AND Cin)

**Truth Table for the Full Adder Gate:**

- Create a truth table for the full adder gate, considering all possible input combinations for the inputs (A, B, and Cin) and the corresponding outputs (S and Cout).

    | A | B | Cin | S | Cout |
    |---|---|-----|---|------|
    | 0 | 0 |  0  | 0 |  0   |
    | 0 | 0 |  1  | 1 |  0   |
    | 0 | 1 |  0  | 1 |  0   |
    | 0 | 1 |  1  | 0 |  1   |
    | 1 | 0 |  0  | 1 |  0   |
    | 1 | 0 |  1  | 0 |  1   |
    | 1 | 1 |  0  | 0 |  1   |
    | 1 | 1 |  1  | 1 |  1   |

**Implementing the Full Adder Gate using HDL:**

- Write the HDL code for the full adder gate using the previously built half adder gate as a building block. Declare the 1-bit inputs (A, B, and Cin) and the 1-bit outputs (S and Cout) pins. Implement the half adder for the sum (S) and carry-out (Cout) and combine them according to the logic expressions.

    ```hdl
    CHIP FullAdder {
        IN A, B, Cin;
        OUT S, Cout;

        PARTS:
        HalfAdder(a=A, b=B, sum=AB_sum, carry=AB_carry);
        HalfAdder(a=AB_sum, b=Cin, sum=S, carry=Cout_intermediate);
        Or(a=AB_carry, b=Cout_intermediate, out=Cout);
    }
    ```

**Test the Full Adder Gate:**

- Before moving on to the next step, it's essential to test your full adder gate implementation thoroughly. You can do this by using a hardware simulator or a hardware simulator provided by the Nand to Tetris software suite.
- Create test cases by setting different input combinations (A, B, and Cin), and observe the corresponding output values (S and Cout). Verify that the full adder gate produces the correct outputs based on the logic expressions.

    ```hdl
    // Test script for the full adder gate (FullAdder)
    // Test case 1: A=0, B=0, Cin=0
    set A 0
    set B 0
    set Cin 0
    eval
    output S 0 // Expected output is 0
    output Cout 0 // Expected output is 0

    // Test case 2: A=1, B=0, Cin=1
    set A 1
    set B 0
    set Cin 1
    eval
    output S 0 // Expected output is 0
    output Cout 1 // Expected output is 1

    // Test case 3: A=1, B=1, Cin=0
    set A 1
    set B 1
    set Cin 0
    eval
    output S 0 // Expected output is 0
    output Cout 1 // Expected output is 1

    // Test case 4: A=1, B=1, Cin=1
    set A 1
    set B 1
    set Cin 1
    eval
    output S 1 // Expected output is 1
    output Cout 1 // Expected output is 1
    ```

**Debugging:**

- Run the test script and compare the actual outputs (S and Cout) with the expected outputs based on the

 test cases. If there are any discrepancies, review your HDL code for errors or logic mistakes.
- Check the connections between the half adders and the OR gate to ensure they are correct for each input.

Once the full adder gate (FullAdder) passes all test cases, you have successfully designed and implemented the full adder gate using the previously built half adder gate. Congratulations! You can continue exploring more complex circuits and logic designs in your Nand to Tetris journey. Keep up the great work!
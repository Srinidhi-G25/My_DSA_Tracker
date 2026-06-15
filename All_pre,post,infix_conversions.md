Important Conversions(Stack related)             LINK=[Striver's one for all conversions!](https://www.youtube.com/watch?v=4pIc9UBHJtk)







Infix to Postfix



\->if in stack, higher precedence operator is succeeded by a lower, then pop the higher one to the result string.

follow this precedence rule in ALL stack situations.



\->Follow the og sem-2 logic!

\->tc:O(N)

\->sc:O(N)







Infix to Prefix



\->Reverse the string  (Remember to reverse the parenthesis as well)

\->Perform Infix to Postfix process

\->Reverse the resultant string

\->tc:O(N)

\->sc:O(N)





Postfix to Infix



\->While met with operator on 'forward traversal', pop the preceding two operands often separated by ',' in stack ordering.

\->place the operator between the operands in the 'very order'(second top, operator, first top) and insert the resultant string with 'enclosed parenthesis' back to the stack as one operand.

\->keep repeating this process for each operator to get the result.

\->inside while loop(i<N), if operand: push to the stack

&#x20;                    else:

&#x20;                         perform the above process and add the res str to the stack

&#x20;                    i++;

\->tc:O(N)

\->sc:O(N)







Prefix to Infix



\->While met with the operator on 'backward traversal', pop the two preceding operands, place the operator between the operands in the order: first\_top\_of\_stack   operator   second\_top\_of\_stack and insert the resultant string with 'enclosed parenthesis' back to the stack as a single operand;

\->keep repeating this process for each operator to get the result.

\->inside while loop(i>=0)given(i=N-1), if operand: push to the stack

&#x20;                    else:

&#x20;                         perform the above process and add the res str to the stack

&#x20;                    i--;

\->tc:O(N)

\->sc:O(N)





Postfix to Prefix



\->While met with operator on 'forward traversal', pop the preceding two operands often separated by ',' in stack ordering.

\->place the operator before the operands in the 'very order' and insert the resultant string "without" 'enclosed parenthesis' back to the stack as one operand.

\->keep repeating this process for each operator to get the result.

\->inside while loop(i<N), if operand: push to the stack

&#x20;                    else:

&#x20;                         perform the above process and add the res str to the stack

&#x20;                    i++;

\->tc:O(N)

\->sc:O(N)





Prefix to Postfix



\->While met with the operator on 'backward traversal', pop the two preceding operands, place the operator between the operands in the order: first\_top\_of\_stack   second\_top\_of\_stack   operator and insert the resultant string "without" 'enclosed parenthesis' back to the stack as a single operand;

\->keep repeating this process for each operator to get the result.

\->inside while loop(i>=0)given(i=N-1), if operand: push to the stack

&#x20;                    else:

&#x20;                         perform the above process and add the res str to the stack

&#x20;                    i--;

\->tc:O(N)

\->sc:O(N)
























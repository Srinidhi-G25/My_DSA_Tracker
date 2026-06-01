Add one to a number represented by LL

{P.O.V. :  If you really dont get why i make these random commits and uploads, no! its not for the green commit graph marks.I tend to store the code of the sums from my takeuforward sheet that i cant verify and run through leetcode.

Thanks for patiently reading my note!
}

class Solution {
private:
    // Helper function to reverse a linked list (You've seen this before!)
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* curr = head;
        while (curr != nullptr) {
            ListNode* nextTemp = curr->next;
            curr->next = prev;
            prev = curr;
            curr = nextTemp;
        }
        return prev;
    }

public:
    ListNode* addOne(ListNode* head) {
        // Step 1: Reverse the list to access the ones place first
        head = reverseList(head);
        
        // Step 2: Add 1 and handle the carry
        ListNode* curr = head;
        int carry = 1; // We start with a carry of 1 because we are adding exactly 1
        
        while (curr != nullptr) {
            curr->val = curr->val + carry;
            
            // If the value is less than 10, no carry is needed! We can stop doing math.
            if (curr->val < 10) {
                carry = 0;
                break; 
            } 
            // If the value is 10, change it to 0 and push the carry forward
            else {
                curr->val = 0;
                carry = 1;
            }
            
            // EDGE CASE: If we are on the last node and still have a carry, 
            // we must append a new node (e.g., 99 + 1 = 100)
            if (curr->next == nullptr && carry == 1) {
                curr->next = new ListNode(1);
                carry = 0; // We used the carry, so reset it
                break;     // The list is artificially extended, so we are done
            }
            
            curr = curr->next;
        }
        
        // Step 3: Reverse the list back to its original order
        return reverseList(head);
    }
};
/*
The Intuition: Reverse, Add, Reverse
If we need to do grade-school math from right-to-left, we can use a trick you already know: reversing the linked list.

Reverse the list: The number 123 (1 -> 2 -> 3) becomes 3 -> 2 -> 1. Now, the "ones place" is sitting right at the head!

Add 1: We just add 1 to the head.

Carry the 1: If the node becomes 10 (because it was a 9), we change it to a 0 and carry the 1 to the next node, exactly like on paper.

Reverse it back: Once the math is done, we reverse the list one more time to put it back in the correct order.



The Edge Case: 99 + 1 = 100
What happens if the original list is 9 -> 9?

We reverse it: 9 -> 9.

Add 1 to the head: It becomes 10. We change it to 0 and carry 1.

Add carry to the next node: It becomes 10. We change it to 0 and carry 1.

The Trap: We are out of nodes, but we still have a carry of 1! If we stop here, our answer will just be 00.

The Fix: If we reach the absolute end of the list and our carry is still 1, we must create a brand new node with the value 1 and attach it to the end.
*/
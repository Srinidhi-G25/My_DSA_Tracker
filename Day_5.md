Length of Loop in Singly Linked list 

{P.O.V. :  If you really dont get why i make these random commits and uploads, no! its not for the green commit graph marks.I tend to store the code of the sums from my takeuforward sheet that i cant verify and run through leetcode.

Thanks for patiently reading my note!
}

int lengthOfLoop(ListNode *head) {
        ListNode* slow = head;
        ListNode* fast = heed;
        
        // Step 1: Detect if a loop exists
        while (fast != nullptr && fast->next != nullptr) {
            slow = slow->next;          // Move 1 step
            fast = fast->next->next;    // Move 2 steps
            
            // Collision detected! We are definitely inside the loop.
            if (slow == fast) {
                int count = 1;
                fast = fast->next;      // Fast takes the first step
                
                // Step 2: Walk the track until we hit the slow pointer again
                while (fast != slow) {
                    fast = fast->next;  // Move 1 step at a time now
                    count++;            // Count the steps
                }
                
                // Once they meet, the count is the length of the loop
                return count;
            }
        }
        
        // If we exit the loop, fast reached the end. No loop exists.
        return 0;
    }
};                EXPLANATION:

Because you already understand how to detect a cycle (the Tortoise and Hare method), you actually already know 90% of the solution to this problem too.

Think about it: once the fast pointer laps the slow pointer, they collide inside the loop. We don't even need to find where the loop starts (like we did in the last problem) to find out how long it is!

The Intuition: Walk the Lap
Imagine your fast and slow runners collide on a circular track. To find out the exact length of the track, you just give them these instructions:

"Tortoise (Slow), stand perfectly still right where you are."

"Hare (Fast), walk forward exactly one step at a time, and count your steps."

"When the Hare bumps into the Tortoise again, the number of steps taken is the length of the lap!"

The Algorithm Step-by-Step
Find the collision: Use the exact same slow (1 step) and fast (2 steps) pointer logic to detect the cycle.

If no collision: If fast reaches nullptr, there is no cycle. Return 0.

Count the loop: The moment slow == fast, we freeze slow. We move fast forward by exactly 1 step and start a count at 1.

Keep moving fast forward 1 step at a time, adding to count until fast bumps into slow again.

Return the count.
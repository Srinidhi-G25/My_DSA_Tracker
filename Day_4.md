Split Array-Largest Sum

    Solved step by step with 100% beats on leetcode.
    Id:Srinidhi_G21


Painter's Partition 

#include <vector>
#include <algorithm>

class Solution {
public:
    int paint(int A, int B, vector<int>& C) {
        long long low = 0;
        long long high = 0;
        
        // Step 1: Define Boundaries using long long to prevent overflow
        for (int i = 0; i < C.size(); i++) {
            low = max(low, (long long)C[i]); // The thickest single board
            high += (long long)C[i];         // Sum of all boards
        }
        
        long long ans = high;
        
        // Step 2: Binary Search on Answer
        while (low <= high) {
            long long mid = low + (high - low) / 2;
            
            if (isPossible(C, A, mid)) {
                ans = mid;       // It works! Record it.
                high = mid - 1;  // Try to push the painters to work even faster (smaller max length)
            } else {
                low = mid + 1;   // It failed (needed too many painters). Increase the limit.
            }
        }
        
        // Step 3: Apply the multiplier B and the modulo
        // We use modulo arithmetic rules: (ans % M * B % M) % M
        long long MOD = 10000003;
        return (ans % MOD * B % MOD) % MOD;
    }

    // Helper Function: Can we assign boards so no painter exceeds 'max_len'?
    bool isPossible(const vector<int>& C, int A, long long max_len) {
        int painters_needed = 1;
        long long current_board_len = 0;
        
        for (int i = 0; i < C.size(); i++) {
            // If adding the next board exceeds the limit, bring in a new painter
            if (current_board_len + C[i] > max_len) {
                painters_needed++;
                current_board_len = C[i];
            } else {
                // Otherwise, current painter keeps painting
                current_board_len += C[i];
            }
        }
        
        // Did we do it without needing more painters than we have?
        return painters_needed <= A;
    }
};

Difference from ship, book allocation and split array sum:

• The Trick: Identical to Book Allocation and Ship Packages. Use Binary Search on Answer to divide the array into 'A' contiguous parts to minimize the maximum sum.

• Trap 1 (Multiplier 'B'): Ignore 'B' completely during the binary search. Just find the optimal max board length, then multiply the final answer by 'B' at the very end.

• Trap 2 (Modulo): Because the final time can be massive, you must return (Answer * B) % 10000003.

• Trap 3 (Integer Overflow): The sum of all boards can reach $10^{10}$, which overflows a standard 32-bit integer. You MUST use long long for your low, high, mid, and the running sum in your helper function.

• Trap 4 (Idle Painters): Unlike Book Allocation (where every student needs a book), it is perfectly fine if a painter sits idle. Do NOT return -1 if Painters (A) > Boards (C.size()).
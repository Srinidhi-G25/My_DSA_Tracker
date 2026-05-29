Aggressive Cows


#include <vector>
#include <algorithm>

class Solution {
public:
    int aggressiveCows(vector<int>& stalls, int k) {
        // STEP 1: ALWAYS sort the array first!
        sort(stalls.begin(), stalls.end());

        // STEP 2: Define boundaries for Binary Search
        int low = 1;
        int high = stalls.back() - stalls.front(); // Last element - First element
        int best_distance = 0; // To store our final answer

        // STEP 3: Binary Search on the Answer
        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (canPlaceCows(stalls, k, mid)) {
                // It works! This is a valid distance.
                best_distance = mid; 
                // But can we push them even further apart? Go right!
                low = mid + 1;
            } else {
                // It doesn't work. We are forcing them too far apart. Go left!
                high = mid - 1;
            }
        }

        return best_distance;
    }

private:
    // Helper function to check if a distance is valid
    bool canPlaceCows(const vector<int>& stalls, int k, int target_dist) {
        int cows_placed = 1;         // Always place the first cow...
        int last_pos = stalls[0];    // ...in the very first stall.

        for (int i = 1; i < stalls.size(); i++) {
            // If the current stall is far enough from the last cow...
            if (stalls[i] - last_pos >= target_dist) {
                cows_placed++;           // Place a cow!
                last_pos = stalls[i];    // Update the position of the last placed cow

                // If we've successfully placed all 'k' cows, this distance is a success.
                if (cows_placed == k) {
                    return true;
                }
            }
        }
        
        // If the loop finishes and we couldn't place enough cows
        return false;
    }
};
Interview Tip: How to explain this to an interviewerIf you are asked to state the time complexity in an interview, say exactly this:
"The time complexity is $O(N \log N + N \log M)$. The $N \log N$ comes from the initial sorting of the stalls array. The $N \log M$ comes from the binary search, where $M$ is the search space between the maximum and minimum stall distance, and for each of those $\log M$ steps, we do a linear $O(N)$ scan to check if the cows can be placed.

"As for Space Complexity, it is $O(1)$ (constant space). Aside from the small amount of memory the compiler uses for the sorting algorithm, we are only creating a few integer variables (left, right, mid, last_pos). We don't create any new arrays or heavy data structures, no matter how many stalls there are!


Book Allocation


#include <vector>
#include <numeric>
#include <algorithm>

class Solution {
public:
    int findPages(vector<int>& arr, int n, int m) {
        // Edge Case: If there are more students than books, allocation is impossible.
        if (m > n) return -1;
        
        // Step 1: Define boundaries
        int low = *max_element(arr.begin(), arr.end()); // Thickest single book
        int high = accumulate(arr.begin(), arr.end(), 0); // Sum of all books
        int ans = -1;
        
        // Step 2: Binary Search on Answer (Template A)
        while (low <= high) {
            int mid = low + (high - low) / 2;
            
            if (isValidAllocation(arr, n, m, mid)) {
                ans = mid;       // It works! Record it.
                high = mid - 1;  // Try to find a smaller, tighter limit.
            } else {
                low = mid + 1;   // It failed (needs too many students). Increase limit.
            }
        }
        
        return ans;
    }

private:
    // Helper Function: Can we allocate books with 'page_limit' to 'm' students?
    bool isValidAllocation(const vector<int>& arr, int n, int m, int page_limit) {
        int students_needed = 1;
        int current_student_pages = 0;
        
        for (int i = 0; i < n; i++) {
            // If giving this book exceeds the limit, assign it to a NEW student
            if (current_student_pages + arr[i] > page_limit) {
                students_needed++;
                current_student_pages = arr[i]; // New student starts with this book
            } else {
                // Otherwise, current student keeps reading
                current_student_pages += arr[i];
            }
        }
        
        // Did we manage to do it within the allowed number of students?
        return students_needed <= m;
    }
};
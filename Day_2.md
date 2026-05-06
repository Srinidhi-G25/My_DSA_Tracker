
//Find the repeating and missing numbers


#include <vector>

using namespace std;

class Solution {
public:
    vector<int> findTwoElement(vector<int>& arr) {
        long long n=arr.size();

        // Calculate expected sums using formulas
        long long SN=(n(n+1))/2;             //Sum of n numbers
        long long S2N=(n*(n+1)*(2*n+1))/6;   //Expected Square Sum(sum of squares)
        
        long long S=0;
        long long S2=0;

        // Calculate actual sums from the array
        for(int i=0;i<n;i++){
            S+=arr[i];                     //Calc Sum of given nums
            S2+=(long long)arr[i]*(long long)arr[i]; //Calc sum of squares
        }

        // Equation 1: X - Y
        long long val1=S-SN;

        // Equation 2: X^2 - Y^2
        long long val2=S2-S2N;

        // Find X + Y by dividing (X^2 - Y^2) / (X - Y)
        val2=val2/val1;

        // Solve for X (Repeating) and Y (Missing)
        long long x=(val1+val2)/2;
        long long y=x-val1;

        return {(int)x,(int)y};



//Count inversions in an array


#include <vector>

using namespace std;

class Solution {
private:
    long long merge(vector<int>& arr, int low, int mid, int high) {
        vector<int> temp; 
        int left = low;      // starting index of left half
        int right = mid + 1; // starting index of right half
        long long count = 0; // use long long to prevent overflow

        // Compare elements and merge into temp
        while (left <= mid && right <= high) {
            if (arr[left] <= arr[right]) {
                temp.push_back(arr[left]);
                left++;
            } else {
                // Right is smaller than Left -> INVERSION FOUND!
                // It is smaller than everything remaining in the left half
                count += (mid - left + 1);
                
                temp.push_back(arr[right]);
                right++;
            }
        }

        // Copy remaining elements from left half (if any)
        while (left <= mid) {
            temp.push_back(arr[left]);
            left++;
        }

        // Copy remaining elements from right half (if any)
        while (right <= high) {
            temp.push_back(arr[right]);
            right++;
        }

        // Transfer sorted elements back to original array
        for (int i = low; i <= high; i++) {
            arr[i] = temp[i - low];
        }

        return count;
    }

    long long mergeSort(vector<int>& arr, int low, int high) {
        long long count = 0;
        if (low >= high) return count; // Base case: 1 element

        int mid = low + (high - low) / 2;

        // Count inversions in left half
        count += mergeSort(arr, low, mid);
        
        // Count inversions in right half
        count += mergeSort(arr, mid + 1, high);
        
        // Count inversions while merging them together
        count += merge(arr, low, mid, high);

        return count;
    }

public:
    long long int inversionCount(vector<int>& arr) {
        int n = arr.size();
        return mergeSort(arr, 0, n - 1);
    }
};

//Length of the Longest Sub-array with Zero-sum

unordered_map<int,int>hash_map;
int max_len=0;
int prefix_sum=0;

for(int i=0;i<arr.size();i++){
        prefix_sum+=arr[i];
        if(prefix_sum==0){
            max_len=i+1;
        }else if(hash_map.find(prefix_sum)!=hash_map.end()){
            int current=i-hash_map[prefix_sum];
            max_len=max(current,max_len);
        }else{
            hash_map[prefix_sum]=i;
        }
}
return max_len;      TC,SC=O(N)


//Count the number of sub-arrays with given XOR k

unordered_map<int,int>freq_map;
int xr=0;
int count=0;

for(int i=0;i<arr.size();i++){
    // Step 1: Update prefix XOR
    xr=xr^arr[i];//calculates pre-fix XOR
 
    freq_map[0]=1; //prevents edge case where pre-fix XOR =k
 
    // Step 2: Calculate the 'target' prefix XOR we need (x = xr ^ k)
    int target=xr^k;//if x^k=XR(Final prefix xor) then x=XR^k where(k-xor of a subarray(given))
 
    // Step 3: If target exists in map, add its frequency to our count
    if(freq_map.find(target)!=freq_map.end()){
        count+=freq_map[target];
    }
    // Step 4: Update the frequency of the current prefix XOR in the map
    freq_map[xr]++;
}
return count;       TC,SC=O(N)
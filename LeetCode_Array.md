1 Range Addition(范围相加）  
```
Assume you have an array of length n initialized with all 0's and are given k update operations.  
Each operation is represented as a triplet: [startIndex, endIndex, inc] which increments each element of 
subarray A[startIndex ... endIndex] (startIndex and endIndex inclusive) with inc.  
Return the modified array after all k operations were executed.  

Example  
Given:  
    length = 5,  
    updates = [  
        [1,  3,  2],  
        [2,  4,  3],  
        [0,  2, -2]  
    ]  

Output:  
    [-2, 0, 3, 5, 3]  
    
Explanation:  
Initial state:  
[ 0, 0, 0, 0, 0 ]  
After applying operation [1, 3, 2]:  
[ 0, 2, 2, 2, 0 ]  
After applying operation [2, 4, 3]:  
[ 0, 2, 5, 5, 3 ]  
After applying operation [0, 2, -2]:  
[-2, 0, 3, 5, 3 ]  

Hint:
- Thinking of using advanced data structures? You are thinking it too complicated.
- For each update operation, do you really need to update all elements between i and j?
- Update only the first and end element is sufficient.
- The optimal time complexity is O(k + n) and uses O(1) extra space.

思路：
对于最优解法，不能将范围内所有元素都加上数值。实际上，只需要将开头坐标startIndex位置加上inc，而在结束位置加1的地方加上-inc。然后，再依次累加起来
就得到答案。按此种思路，上述 Explaation 应该为：
[ 0, 0, 0, 0, 0 ]
[ 0, 2, 0, 0, -2 ]
[ 0, 2, 3, 0, -2 ]
[ -2, 2, 3, 2, -2 ]
[ -2, 0, 3, 5, 3 ]
```











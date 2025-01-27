/*
Question 1 : 

LeetCode 1877. Minimize Maximum Pair Sum in Array

The pair sum of a pair (a,b) is equal to a + b. The maximum pair sum is the largest pair sum in a list of pairs.

For example, if we have pairs (1,5), (2,3), and (4,4), the maximum pair sum would be max(1+5, 2+3, 4+4) = max(6, 5, 8) = 8.
Given an array nums of even length n, pair up the elements of nums into n / 2 pairs such that:

Each element of nums is in exactly one pair, and
The maximum pair sum is minimized.
Return the minimized maximum pair sum after optimally pairing up the elements.

Example 1:

Input: nums = [3,5,2,3]
Output: 7
Explanation: The elements can be paired up into pairs (3,3) and (5,2).
The maximum pair sum is max(3+3, 5+2) = max(6, 7) = 7.

********************** Code **********************
*/

class Solution {
public:
    int minPairSum(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        int ans = 0;
        int s = 0, e = n - 1;
        while(s < e){
            ans = max(ans, nums[s++] + nums[e--]);
        }
        return ans;
    }
};


/*
Question 2 : 

LeetCode 53. Maximum Subarray

Given an integer array nums, find the 
subarray
 with the largest sum, and return its sum.

 

Example 1:

Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: The subarray [4,-1,2,1] has the largest sum 6.

********************** Code **********************
*/

class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int sum = 0, maxsum = INT_MIN;
        for(int i = 0; i < nums.size();i++){
            sum = max(sum + nums[i], nums[i]);
            maxsum = max(sum, maxsum);
        }
        return maxsum;
    }
};


/*
Question 3 : 

LeetCode 1861. Rotating the Box

You are given an m x n matrix of characters boxGrid representing a side-view of a box. Each cell of the box is one of the following:

A stone '#'
A stationary obstacle '*'
Empty '.'
The box is rotated 90 degrees clockwise, causing some of the stones to fall due to gravity. Each stone falls down until it lands on an obstacle, another stone, or the bottom of the box. Gravity does not affect the obstacles' positions, and the inertia from the box's rotation does not affect the stones' horizontal positions.

It is guaranteed that each stone in boxGrid rests on an obstacle, another stone, or the bottom of the box.

Return an n x m matrix representing the box after the rotation described above.

********************** Code **********************
*/

class Solution {
public:
    vector<vector<char>> rotateTheBox(vector<vector<char>>& box) {
        int row = box.size(), col = box[0].size();

        for(int i = 0; i < row; i++){
            int x = -1;
            for(int j = 0; j < col; j++){
                if(box[i][j] == '.' && x != -1){
                    swap(box[i][j], box[i][x]);
                    x++;
                }
                else if(x == -1 && box[i][j] == '#') x = j;
                else if(box[i][j] == '*') x = -1;
            }
        }

        swap(row, col);
        vector<vector<char>> ans(row, vector<char> (col, '.'));
        for(int i = 0; i < row; i++){
            for(int j = 0; j < col; j++){
                ans[i][j] = box[j][i];
            }
        }

        for(int i = 0; i < row; i++){
            int s = 0, e = col - 1;
            while(s < e){
                swap(ans[i][s], ans[i][e]);
                s++;
                e--;
            }
        }
        return ans;
    }
};


/*
Question 4 : 

LeetCode : 2241. Design an ATM Machine

There is an ATM machine that stores banknotes of 5 denominations: 20, 50, 100, 200, and 500 dollars. Initially the ATM is empty. The user can use the machine to deposit or withdraw any amount of money.

When withdrawing, the machine prioritizes using banknotes of larger values.

For example, if you want to withdraw $300 and there are 2 $50 banknotes, 1 $100 banknote, and 1 $200 banknote, then the machine will use the $100 and $200 banknotes.
However, if you try to withdraw $600 and there are 3 $200 banknotes and 1 $500 banknote, then the withdraw request will be rejected because the machine will first try to use the $500 banknote and then be unable to use banknotes to complete the remaining $100. Note that the machine is not allowed to use the $200 banknotes instead of the $500 banknote.
Implement the ATM class:

ATM() Initializes the ATM object.
void deposit(int[] banknotesCount) Deposits new banknotes in the order $20, $50, $100, $200, and $500.
int[] withdraw(int amount) Returns an array of length 5 of the number of banknotes that will be handed to the user in the order $20, $50, $100, $200, and $500, and update the number of banknotes in the ATM after withdrawing. Returns [-1] if it is not possible (do not withdraw any banknotes in this case).
 
Example 1:

Input
["ATM", "deposit", "withdraw", "deposit", "withdraw", "withdraw"]
[[], [[0,0,1,2,1]], [600], [[0,1,0,1,1]], [600], [550]]
Output
[null, null, [0,0,1,0,1], null, [-1], [0,1,0,0,1]]

********************** Code **********************
*/

class ATM {
private:
    int n;
    vector<int> curr;
    int amt;
public:
    ATM() : n(5), curr(n, 0) {
    }
    
    void deposit(vector<int> banknotesCount) {
        for(int i = 0; i < 5; i++){
            curr[i] += banknotesCount[i];
        }
    }
    
    vector<int> withdraw(int amount) {
        amt = amount;
        int fiveHund = 0, twoHund = 0, hund = 0, fifty = 0, twenty = 0;
        fiveHund = amt / 500;
        if(curr[4] >= fiveHund) amt %= 500;
        else{
            fiveHund = curr[4];
            amt -= (fiveHund * 500);
        }

        twoHund = amt / 200;
        if(curr[3] >= twoHund) amt %= 200;
        else{
            twoHund = curr[3];
            amt -= (twoHund * 200);
        } 

        hund = amt / 100;
        if(curr[2] >= hund) amt %= 100;
        else{
            hund = curr[2];
            amt -= (hund * 100);
        }

        fifty = amt / 50;
        if(curr[1] >= fifty) amt %= 50;
        else{
            fifty = curr[1];
            amt -= (fifty * 50);
        } 

        twenty = amt / 20;
        if(curr[0] >= twenty) amt %= 20;
        else{
            twenty = curr[0];
            amt -= (twenty * 20);
        } 

        if(amt == 0){
            curr[0] -= twenty;
            curr[1] -= fifty;
            curr[2] -= hund;
            curr[3] -= twoHund;
            curr[4] -= fiveHund;

            return {twenty, fifty, hund, twoHund, fiveHund};
        }
        return {-1};
    }
};


/*
Question 5 :

LeetCode 596. Classes More Than 5 Students

Table: Courses

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| student     | varchar |
| class       | varchar |
+-------------+---------+
(student, class) is the primary key (combination of columns with unique values) for this table.
Each row of this table indicates the name of a student and the class in which they are enrolled.
 
Write a solution to find all the classes that have at least five students.

Return the result table in any order.

The result format is in the following example.

Example 1:

Input: 
Courses table:
+---------+----------+
| student | class    |
+---------+----------+
| A       | Math     |
| B       | English  |
| C       | Math     |
| D       | Biology  |
| E       | Math     |
| F       | Computer |
| G       | Math     |
| H       | Math     |
| I       | Math     |
+---------+----------+
Output: 
+---------+
| class   |
+---------+
| Math    |
+---------+
*/

********************** Query **********************

SELECT DISTINCT class FROM Courses
GROUP BY class Having COUNT(class) >= 5;

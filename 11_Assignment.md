/*
Question 1 :

LeetCode : 1800. Maximum Ascending Subarray Sum

Given an array of positive integers nums, return the maximum possible sum of an ascending subarray in nums.

A subarray is defined as a contiguous sequence of numbers in an array.

A subarray [numsl, numsl+1, ..., numsr-1, numsr] is ascending if for all i where l <= i < r, numsi  < numsi+1. Note that a subarray of size 1 is ascending.

Example 1:

Input: nums = [10,20,30,5,10,50]
Output: 65

********************** Code **********************
*/

class Solution {
public:
    int maxAscendingSum(vector<int>& nums) {
        int n = nums.size();
        int maxi = 0, sum = 0;
        int prev = -1;
        for(int i = 0; i < n; i++){
            if(nums[i] > prev){
                sum += nums[i];
                maxi = max(maxi, sum);
            }
            else{
                sum = nums[i];
                maxi = max(maxi, sum);
            }
            prev = nums[i];
        }
        return maxi;
    }
};



/*
Question 2 : 
LeetCode : 1790. Check if One String Swap Can Make Strings Equal

You are given two strings s1 and s2 of equal length. A string swap is an operation where you choose two indices in a string (not necessarily different) and swap the characters at these indices.

Return true if it is possible to make both strings equal by performing at most one string swap on exactly one of the strings. Otherwise, return false.

Example 1:

Input: s1 = "bank", s2 = "kanb"
Output: true

********************** Code **********************
*/

class Solution {
public:
    bool areAlmostEqual(string s1, string s2) {
        if(s1 == s2) return true;

        int n = s1.length(), m = s2.length();
        if(n != m) return false;

        int x = -1, y = -1;
        for(int i = 0; i < n; i++){
            if(s1[i] != s2[i]){
                if(x == -1) x = i;
                else{
                    y = i;
                    break;
                }
            }
        }
        if(x == -1 || y == -1) return false;
        swap(s1[x], s1[y]);
        return s1 == s2;
    }
};



/*
Question 3 : 

LeetCode : 3105. Longest Strictly Increasing or Strictly Decreasing Subarray

You are given an array of integers nums. Return the length of the longest subarray of nums which is either strictly increasing or strictly decreasing.

Example 1:

Input: nums = [1,4,3,3,2]
Output: 2

********************** Code **********************
*/

class Solution {
public:
    int longestMonotonicSubarray(vector<int>& nums) {
        int n = nums.size();
        int maxi = 0, count = 0;
        int last = -1, e = 0;
        while(e < n){
            if(nums[e] > last) count++;
            else count = 1;
            last = nums[e];
            maxi = max(maxi, count);
            e++;
        }

        count = 0;
        last = INT_MAX, e = 0;
        while(e < n){
            if(nums[e] < last) count++;
            else count = 1;
            last = nums[e];
            maxi = max(maxi, count);
            e++;
        }
        return maxi;
    }
};



/*
Question 4 : 

LeetCode : 183. Customers Who Never Order

Table: Customers

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+
id is the primary key (column with unique values) for this table.
Each row of this table indicates the ID and name of a customer.
 
Table: Orders

+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| customerId  | int  |
+-------------+------+
id is the primary key (column with unique values) for this table.
customerId is a foreign key (reference columns) of the ID from the Customers table.
Each row of this table indicates the ID of an order and the ID of the customer who ordered it.
 
Write a solution to find all customers who never order anything.
Return the result table in any order.
The result format is in the following example.

********************** Query **********************
*/

SELECT c.name AS Customers FROM 
Customers AS c LEFT JOIN Orders AS o
ON c.id = o.customerId
WHERE o.customerId is NULL;
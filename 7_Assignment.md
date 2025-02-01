/*
Question 1 : 

Coding Ninja :  Bit Set

Problem statement
You are given a sequence of only digits in the form of a string 'DIGIT_PATTERN', your task is to find the first repeating digit. If no digit is repeating you should return -1.

Example:

Given string of digits is 123456325. Now starting from the left, the first digit which is repeating is 3 as till 2nd 3 every digit is encountered 1st time and thus our answer for this input will be 3.

********************** Code **********************
*/

#include <bits/stdc++.h> 
int findFirstRepeatingDigit(string digitPattern) {
    int n = digitPattern.length();
    set<char> st;
    for(int i = 0; i < n; i++){
        if(st.find(digitPattern[i]) == st.end()) st.insert(digitPattern[i]);
        else return stoi(digitPattern.substr(i, 1));
    }
    return -1;
}



/*
Question 2 : 

LeetCode : 3438. Find Valid Pair of Adjacent Digits in String

You are given a string s consisting only of digits. A valid pair is defined as two adjacent digits in s such that:

The first digit is not equal to the second.
Each digit in the pair appears in s exactly as many times as its numeric value.
Return the first valid pair found in the string s when traversing from left to right. If no valid pair exists, return an empty string.

Example 1:

Input: s = "2523533"
Output: "23"

********************** Code **********************
*/

class Solution {
public:
    string findValidPair(string s) {
        map<char, int> m;
        for(auto& mp : s){
            m[mp]++;
        }

        string ans = "";
        int n = s.length();
        for(int i = 0; i < n - 1; i++){
            if(s[i] != s[i + 1]){
                if(stoi(s.substr(i, 1)) == m[s[i]] && stoi(s.substr(i + 1, 1)) == m[s[i + 1]]){
                    ans += s[i];
                    ans += s[i + 1];
                    return ans;
                }
            }
        }
        return ans;
    }
};



/*
Question 3 : 

LeetCode : 3439. Reschedule Meetings for Maximum Free Time I

You are given an integer eventTime denoting the duration of an event, where the event occurs from time t = 0 to time t = eventTime.

You are also given two integer arrays startTime and endTime, each of length n. These represent the start and end time of n non-overlapping meetings, where the ith meeting occurs during the time [startTime[i], endTime[i]].

You can reschedule at most k meetings by moving their start time while maintaining the same duration, to maximize the longest continuous period of free time during the event.
The relative order of all the meetings should stay the same and they should remain non-overlapping.
Return the maximum amount of free time possible after rearranging the meetings.

Note that the meetings can not be rescheduled to a time outside the event.

Example 1:

Input: eventTime = 5, k = 1, startTime = [1,3], endTime = [2,5]
Output: 2

********************** Code **********************
*/

class Solution {
public:
    int maxFreeTime(int eventTime, int k, vector<int>& startTime, vector<int>& endTime) {
        int n = startTime.size();
        vector<int> free;
        int st = 0, ed = 0;
        for(int i = 0; i < n; i++){
            st = startTime[i];
            free.push_back(st - ed);
            ed = endTime[i];
        }
        free.push_back(eventTime - ed);
        int s = 0, e = 0;
        int ans = 0;
        while(e <= k && e < free.size()){
            ans += free[e];
            e++;
        }
        int maxi = ans;
        while(e < free.size()){
            ans = (ans - free[s] + free[e]);
            maxi = max(maxi, ans);
            s++;
            e++;
        }
        return maxi;
    }
};



/*
Question 4 : 

LeetCode : 303. Range Sum Query - Immutable

Given an integer array nums, handle multiple queries of the following type:

Calculate the sum of the elements of nums between indices left and right inclusive where left <= right.
Implement the NumArray class:

NumArray(int[] nums) Initializes the object with the integer array nums.
int sumRange(int left, int right) Returns the sum of the elements of nums between indices left and right inclusive (i.e. nums[left] + nums[left + 1] + ... + nums[right]).

********************** Code **********************
*/

class NumArray {
private:
    vector<int> ps;
public:
    NumArray(vector<int>& nums) {
        int sum = 0, n = nums.size();
        ps.push_back(sum);
        for(int i = 0; i < n; i++){
            sum += nums[i];
            ps.push_back(sum);
        }
    }
    
    int sumRange(int left, int right) {
        return ps[right + 1] - ps[left];
    }
};



/*
Question 5 :

LeetCode : 181. Employees Earning More Than Their Managers

Table: Employee

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| salary      | int     |
| managerId   | int     |
+-------------+---------+
id is the primary key (column with unique values) for this table.
Each row of this table indicates the ID of an employee, their name, salary, and the ID of their manager.
 

Write a solution to find the employees who earn more than their managers.
Return the result table in any order.
The result format is in the following example.

********************** Code **********************
*/

SELECT e.name AS Employee FROM
Employee AS e INNER JOIN Employee AS m
ON e.managerId = m.id
WHERE e.salary > m.salary;
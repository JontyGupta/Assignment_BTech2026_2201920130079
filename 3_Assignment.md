/*
Question 1 : 
Coding Ninja :  Best Time to Buy and Sell Stock

Problem statement
You are given an array/list 'prices' where the elements of the array represent the prices of the stock as they were yesterday and indices of the array represent minutes. Your task is to find and return the maximum profit you can make by buying and selling the stock. You can buy and sell the stock only once.

You can’t sell without buying first.
For Example:
For the given array [ 2, 100, 150, 120],
The maximum profit can be achieved by buying the stock at minute 0 when its price is Rs. 2 and selling it at minute 2 when its price is Rs. 150.
So, the output will be 148.
Detailed explanation ( Input/output format, Notes, Images )
Constraints:
1 <= T <= 10
2 <= N <= 10^4
1 <= ARR[i] <= 10^9

Time Limit: 1 sec.
Sample Input 1:
2
4
1 2 3 4
4
2 2 2 2
Sample Output 1:
3
0

********************** Code **********************
*/

#include <bits/stdc++.h> 
int maximumProfit(vector<int> &prices){
    // Write your code here.
    int n = prices.size();
    int minbuy = prices[0];
    int maxprofit = 0;
    for(int i = 1; i < n; i++){
        minbuy = min(minbuy, prices[i]);
        maxprofit = max(maxprofit, prices[i] - minbuy);
    }
    return maxprofit;
}



/*
Question 2 : 
Coding Ninja : Binary Pattern

Problem statement
You have been given an input integer 'N'. Your task is to print the following binary pattern for it.

Example

Pattern for 'N' = 4

1111
000
11
0
The first line contains 'N' 1s. The next line contains 'N' - 1 0s. Then the next line contains 'N' - 2 1s and so on.

Detailed explanation ( Input/output format, Notes, Images )
Constraints :
1 <= T <= 10
1 <= N <= 10^3

Time Limit: 1 sec
Sample Input 1:
2
1
2
Sample Output 1:
1
11
0

********************** Code **********************
*/

#include <bits/stdc++.h> 
void printPatt(int n) {
  // Write your code here.
  int val = 1;
  for(int i = n; i >= 1; i--){
    for(int j = 1; j <= i; j++){
      cout << val;
    }
    cout << endl;
    val = !val;
  }
}


/*
Question 3 : 
Coding Ninja :  Tree Traversals

Problem statement
You have been given a Binary Tree of 'N'

nodes, where the nodes have integer values.

Your task is to return the ln-Order, Pre-Order, and Post-Order traversals of the given binary tree.

For example :

Sample Input 1 :
1 2 3 -1 -1 -1  6 -1 -1
Sample Output 1 :
2 1 3 6 
1 2 3 6 
2 6 3 1

********************** Code **********************
*/

vector<vector<int>> getTreeTraversal(TreeNode *root){
    // Write your code here.
    TreeNode* t = root;
    vector<vector<int>> v;
    vector<int> ds;
    inorder(t, ds);
    v.push_back(ds);
    ds.clear();
    t = root;
    preorder(t, ds);
    v.push_back(ds);
    t = root;
    ds.clear();
    postorder(t, ds);
    v.push_back(ds);
    return v;
}

void inorder(TreeNode* t, vector<int> &ds){
    if(t == NULL){
        return;
    }
    inorder(t -> left, ds);
    ds.push_back(t -> data);
    inorder(t -> right, ds);
}
void preorder(TreeNode* t, vector<int> &ds){
    if(t == NULL){
        return;
    }
    ds.push_back(t -> data);
    preorder(t -> left, ds);
    preorder(t -> right, ds);
}
void postorder(TreeNode* t, vector<int> &ds){
    if(t == NULL){
        return;
    }
    postorder(t -> left, ds);
    postorder(t -> right, ds);
    ds.push_back(t -> data);
}



/*
Question 4 : 

LeetCode 1845. Seat Reservation Manager

Design a system that manages the reservation state of n seats that are numbered from 1 to n.

Implement the SeatManager class:

SeatManager(int n) Initializes a SeatManager object that will manage n seats numbered from 1 to n. All seats are initially available.
int reserve() Fetches the smallest-numbered unreserved seat, reserves it, and returns its number.
void unreserve(int seatNumber) Unreserves the seat with the given seatNumber.
 
Example 1:

Input
["SeatManager", "reserve", "reserve", "unreserve", "reserve", "reserve", "reserve", "reserve", "unreserve"]
[[5], [], [], [2], [], [], [], [], [5]]
Output
[null, 1, 2, null, 2, 3, 4, 5, null]

********************** Code **********************
*/

class SeatManager {
    set<int> reserved;
    set<int> unreserved;
public:
    SeatManager(int n) {
        for(int i = 1; i <= n; i++){
            unreserved.insert(i);
        }
    }
    
    int reserve() {
        int val;
        for(auto& mp : unreserved){
            val = mp;
            break;
        }
        unreserved.erase(val);
        reserved.insert(val);
        return val;
    }
    
    void unreserve(int seatNumber) {
        reserved.erase(seatNumber);
        unreserved.insert(seatNumber);
    }
};


/*
Question 5 : 

LeetCode 586. Customer Placing the Largest Number of Orders

Table: Orders

+-----------------+----------+
| Column Name     | Type     |
+-----------------+----------+
| order_number    | int      |
| customer_number | int      |
+-----------------+----------+
order_number is the primary key (column with unique values) for this table.
This table contains information about the order ID and the customer ID.
 
Write a solution to find the customer_number for the customer who has placed the largest number of orders.

The test cases are generated so that exactly one customer will have placed more orders than any other customer.

The result format is in the following example.

Example 1:

Input: 
Orders table:
+--------------+-----------------+
| order_number | customer_number |
+--------------+-----------------+
| 1            | 1               |
| 2            | 2               |
| 3            | 3               |
| 4            | 3               |
+--------------+-----------------+
Output: 
+-----------------+
| customer_number |
+-----------------+
| 3               |
+-----------------+

********************** Query **********************
*/

SELECT customer_number FROM Orders 
GROUP BY customer_number 
ORDER BY COUNT(customer_number) 
DESC 
LIMIT 1
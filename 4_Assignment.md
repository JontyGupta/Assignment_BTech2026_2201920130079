/*
Question 1 : 

Coding Ninja : Search In BST

Problem statement
There is a Binary Search Tree (BST) consisting of ‘N’ nodes. Each node of this BST has some integer data.
You are given the root node of this BST, and an integer ‘X’. Return true if there is a node in BST having data equal to ‘X’, otherwise return false.

A binary search tree (BST) is a binary tree data structure that has the following properties:

1. The left subtree of a node contains only nodes with data less than the node’s data.
2. The right subtree of a node contains only nodes with data greater than the node’s data.
3. The left and right subtrees must also be binary search trees.
Note:
It is guaranteed that all nodes have distinct data.
Detailed explanation ( Input/output format, Notes, Images )
Sample Input 1:
7 8
4 2 6 1 3 5 7 -1 -1 -1 -1 -1 -1 -1 -1
Sample Output 1:
False

********************** Code **********************
*/

bool searchInBST(BinaryTreeNode<int> *root, int x) {
    if(root == NULL) return false;

    if(root -> data == x) return true;
    else if(root -> data > x) return searchInBST(root -> left, x);
    else return searchInBST(root -> right, x);
}



/*
Question 2 : 

Coding Ninja : Ceil from BST

Problem statement
Ninja is given a binary search tree and an integer. Now he is given a particular key in the tree and returns its ceil value. Can you help Ninja solve the problem?

Note:
Ceil of an integer is the closest integer greater than or equal to a given number.
For example:
arr[] = {1, 2, 5, 7, 8, 9}, key = 3.
The closest integer greater than 3 in the given array is 5. So, its ceil value in the given array is 5.

********************** Code **********************
*/

int findCeil(BinaryTreeNode<int> *node, int x){
    if(node == NULL) return -1;
    int ans = -1;
    while(node){
        if(node -> data == x){
            ans = node -> data;
            return ans;
        }
        if(node -> data > x){
            ans = node -> data;
            node = node -> left;
        }
        else{
            node = node -> right;
        }
    }
    return ans;
}



/*
Question 3 : 

Coding Ninja : Floor in BST

Problem statement
You are given a BST (Binary search tree) with’ N’ number of nodes and a value ‘X’. Your task is to find the greatest value node of the BST which is smaller than or equal to ‘X’.

Note :‘X’ is not smaller than the smallest node of BST .

For example:

In the above example, For the given BST  and X = 7, the greatest value node of the BST  which is smaller than or equal to  7 is 6.

********************** Code **********************
*/

int floorInBST(TreeNode<int> * root, int X){
    if(root == NULL) return -1;

    int ans = INT_MAX;
    while(root){
        if(root -> val == X){
            ans = root -> val;
            return ans;
        }
        if(root -> val < X){
            ans = root -> val;
            root = root -> right;
        }
        else{
            root = root -> left;
        }
    }
    return ans;
}



/*
Question 4 : 

LeetCode : 225. Implement Stack using Queues

Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (push, top, pop, and empty).

Implement the MyStack class:

void push(int x) Pushes element x to the top of the stack.
int pop() Removes the element on the top of the stack and returns it.
int top() Returns the element on the top of the stack.
boolean empty() Returns true if the stack is empty, false otherwise.
Notes:

You must use only standard operations of a queue, which means that only push to back, peek/pop from front, size and is empty operations are valid.
Depending on your language, the queue may not be supported natively. You may simulate a queue using a list or deque (double-ended queue) as long as you use only a queue's standard operations.
 
Example 1:

Input
["MyStack", "push", "push", "top", "pop", "empty"]
[[], [1], [2], [], [], []]
Output
[null, null, null, 2, 2, false]

********************** Code **********************
*/

class MyStack {
private:
    queue<int> q;
public:
    MyStack() { }
    
    void push(int x) {
        q.push(x);
    }
    
    int pop() {
        int n = q.size();
        for(int i = 0; i < n - 1; i++){
            q.push(q.front());
            q.pop();
        }
        int val = q.front();
        q.pop();
        return val;
    }
    
    int top() {
        int n = q.size();
        int ans;
        for(int i = 0; i < n; i++){
            ans = q.front();
            q.push(q.front());
            q.pop();
        }
        return ans;
    }
    
    bool empty() {
        return q.size() == 0;
    }
};

/*
Question 5 : 

Coding Ninja : Combine Two Tables

Table: Person

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
PersonId is the primary key column for this table.
Table: Address

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
AddressId is the primary key column for this table.
Write a SQL query for a report that provides the following 
information for each person in the Person table, regardless if there is an address for each of those people:

FirstName, LastName, City, State

********************** Query **********************
*/

SELECT p.FirstName, p.LastName, a.City, a.State 
FROM Person AS p LEFT JOIN Address AS a 
ON p.PersonId = a.PersonId;
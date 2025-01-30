/*
Question 1 :

CodeChef : Swish Game

Toofan is playing an exciting game where there are N rings, and for each ring, Toofan can either SWISH or PASS. The goal of the game is to achieve at least K swishes.

The game will end immediately when at least one of the following two conditions is met:

Toofan has finished all the N rings; or
It it impossible to win the game any more.
This means it is impossible for Toofan to get at least K swishes, regardless of what he does with the remaining rings.
Toofan knows the value of K (the number of swishes he must achieve), and remembers the sequence of moves he made. However, he no longer remembers the value of N (the total number of rings).
You are given a string S of length M, denoting the moves Toofan made during the game. Each character of S is either S (denoting a swish) or P (denoting a pass).

Given that Toofan ended the game after exactly this sequence of M moves, what could the value of N be?

********************** Code **********************
*/

#include <bits/stdc++.h>
using namespace std;

int main() {
    int t;
    cin >> t;
    while(t--){
        int m, k;
        cin >> m >> k;
        string s;
        cin >> s;
        int swish = 0;
        for(int i = 0; i < m; i++){
            if(s[i] == 'S') swish++;
        }
        
        if(swish >= k){
            cout << m << endl;
        } 
        else{
            int rs = k - swish;
            cout << m + rs - 1 << endl;
        }
    }
}



/*
Question 2 : 

CodeChef : White Wall

Toofan loves white walls! He recently painted a wall with some colors: Red (R), Green (G), and Blue (B). However, he realized that the wall doesn't look as bright and white as he imagined.

Toofan's wall has length N, meaning it has N positions placed in a line, and each position can be painted in one color — either red, blue, or green.
You are given a string S of length N, denoting the initial coloring of the wall.

A white wall is defined as a wall where every consecutive set of positions with length divisible by 3 contains equal numbers of R, G, and B.
Formally, for every pair of indices (L,R) such that 1≤L≤R≤N and (R−L+1) is divisible by 3, there must be an equal amount of R, G, and B in the substring 

For instance,
S= RGBR is an example of a white wall - the only substrings with length divisible by 3 are RGB (with L=1 and R=3) L=2 and R=4), which both have an equal number of each color.

S= RGBRGG is an example of a wall that's not white: choosing L=1 and R=6 gives us a segment with length 6 (which is a multiple of 3), which has three positions painted green but only one painted blue, which aren't equal.
Toofan can repaint a single position on the wall to any other color (R, G, or B) in one operation.
Your task is to determine the minimum number of operations required to transform the given wall into a white wall.

********************** Code **********************
*/

#include <bits/stdc++.h>
using namespace std;

int main() {
    int t;
    cin >> t;
    while(t--){
        int n;
        cin >> n;
        string s;
        cin >> s;
    
        int ans = INT_MAX;
        vector<string> p = {"BGR", "BRG", "GBR", "GRB", "RGB", "RBG"};
        for(int i = 0; i < p.size(); i++){
            int count = 0;
            for(int j = 0; j < n; j++){
                if(s[j] != p[i][j % 3]) count++;
            }
            ans = min(ans, count);
        }
        cout << ans << endl;
        
    }
}


/*
Question 3 : 

Coding Ninja :  Find K-th smallest Element in BST

Problem statement
Given a binary search tree and an integer ‘K’. Your task is to find the ‘K-th’ smallest element in the given BST( binary search tree).

BST ( binary search tree) -

If all the smallest nodes on the left side and all the greater nodes on the right side of the node current node.

********************** Code **********************
*/

int kthSmallest(BinaryTreeNode<int>* root, int k) {
    int count = 0, ans = -1;
    inorder(root, count, k, ans);
    return ans;
}

void inorder(BinaryTreeNode<int>* root, int &count, int &k, int &ans){
    if(root == NULL) return;
    inorder(root -> left, count, k, ans);
    count++;
    if(count == k){
        ans = root -> data;
        return;
    }
    inorder(root -> right, count, k, ans);
}


/*
Question 4 :

LeetCode : 232. Implement Queue using Stacks

Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (push, peek, pop, and empty).

Implement the MyQueue class:

void push(int x) Pushes element x to the back of the queue.
int pop() Removes the element from the front of the queue and returns it.
int peek() Returns the element at the front of the queue.
boolean empty() Returns true if the queue is empty, false otherwise.
Notes:

You must use only standard operations of a stack, which means only push to top, peek/pop from top, size, and is empty operations are valid.
Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.

********************** Code **********************
*/

class MyQueue {
private:
    stack<int> stk, temp;
public:
    MyQueue() { }
    
    void push(int x) {
        stk.push(x);
    }
    
    int pop() {
        int n = stk.size();
        for(int i = 0; i < n - 1; i++){
            temp.push(stk.top());
            stk.pop();
        }
        int val = stk.top();
        stk.pop();
        for(int i = 0; i < n - 1; i++){
            stk.push(temp.top());
            temp.pop();
        }
        return val;
    }
    
    int peek() {
        int n = stk.size(), ans;
        for(int i = 0; i < n; i++){
            temp.push(stk.top());
            ans = temp.top();
            stk.pop();
        }
        for(int i = 0; i < n; i++){
            stk.push(temp.top());
            temp.pop();
        }
        return ans;
    }
    
    bool empty() {
        return (stk.size() == 0);
    }
};



/*
Question 5 : 

Coding Ninja : Students DB

Insert below student details in students table and print all data of table.

+---------+--------+-------+
| ID  |  Name       | Gender|
+---------+--------+-------+
|   3     |  Kim    |   F   |
|   4     | Molina  |   F   |
|   5     | Dev     |   M   |
+---------+--------+-------+

********************** Query **********************
*/

INSERT INTO students(id, name, gender)
VALUES
(3, 'Kim', 'F'),
(4, 'Molina', 'F'),
(5, 'Dev', 'M');

SELECT * FROM students;
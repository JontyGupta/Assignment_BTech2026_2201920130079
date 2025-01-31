/*
Question  1 : 

Coding ninja :  BFS in Graph

Problem statement
Given an adjacency list representation of a directed graph with ‘n’ vertices and ‘m’ edges. Your task is to return a list consisting of Breadth-First Traversal (BFS) starting from vertex 0.

In this traversal, one can move from vertex 'u' to vertex 'v' only if there is an edge from 'u' to 'v'. The BFS traversal should include all nodes directly or indirectly connected to vertex 0.

Note:
The traversal should proceed from left to right according to the input adjacency list.

********************** Code **********************
*/

vector<int> bfsTraversal(int n, vector<vector<int>> &adj){
    int start = 0;
    queue<int> q;
    q.push(start);
    vector<int> vis(n, 0);
    vis[start] = 1;
    vector<int> ans;
    while(!q.empty()){
        int node = q.front();
        q.pop();
        ans.push_back(node);
        for(auto& it : adj[node]){
            if(!vis[it]){
                vis[it] = 1;
                q.push(it);
            }
        }
    }
    return ans;
}



/*
Question 2 : 

Coding Ninja :  Level Order Traversal

Problem statement
You have been given a Binary Tree of integers. You are supposed to return the level order traversal of the given tree.

For example:
For the given binary tree
The level order traversal will be {1,2,3,4,5,6,7}.

********************** Code **********************
*/

vector<int> getLevelOrder(BinaryTreeNode<int> *root){
    vector<int> ans;
    if(root == NULL) return ans;
    queue<BinaryTreeNode<int>*> q;
    q.push(root);
    while(!q.empty()){
        int n = q.size();
        for(int i = 0; i < n; i++){
            auto node = q.front();
            q.pop();
            ans.push_back(node -> val);
            if(node -> left) q.push(node -> left);
            if(node -> right) q.push(node -> right);
        }
    }
    return ans;
}


/*
Question 3 : 

Coding Ninja : Number of Islands

Problem statement
You have been given a non-empty grid consisting of only 0s and 1s. You have to find the number of islands in the given grid.

An island is a group of 1s (representing land) connected horizontally, vertically or diagonally. You can assume that all four edges of the grid are surrounded by 0s (representing water).

********************** Code **********************
*/

#include <bits/stdc++.h> 
void bfs(int i, int j, int n, int m, vector<vector<int>> grid, vector<vector<int>> &vis){
    vis[i][j] = 1;
    queue<pair<int, int>> q;
    q.push({i, j});
    while(!q.empty()){
        int r = q.front().first;
        int c = q.front().second;
        q.pop();

        for(int dr = -1; dr <= 1; dr++){
            for(int dc = -1; dc <= 1; dc++){
                int nr = r + dr;
                int nc = c + dc;
                if(nr >= 0 && nr < n && nc >= 0 && nr < m && grid[nr][nc] == 1 && vis[nr][nc] == 0){
                    vis[nr][nc] = 1;
                    q.push({nr, nc});
                }
            }
        }
    }
}
int numberOfIslands(vector<vector<int>>& grid, int n, int m){
    int count = 0;
    vector<vector<int>> vis(n, vector<int> (m, 0));
    for(int i = 0; i < n; i++){
        for(int j = 0; j < m; j++){
            if(!vis[i][j] && grid[i][j] == 1){
                count++;
                bfs(i, j, n, m, grid, vis);
            }
        }
    }
    return count;
}





/*
Question 4 : 

Coding Ninja : Area of a Rectangle

Problem statement
Design a class called Rectangle. It contains two data members as length(L) and breadth(B); and a member function getArea(). The member function computes the area of the given rectangle and returns it to the caller.

Note:

Area of a rectangle = length x breadth
Data Members:
1. length: Length of the rectangle
2. breadth: Breadth of the rectangle 
Member Functions:
1. getArea(): that calculates the area of the rectangle and returns the value.
Note:

You do not have to take input, just create the Class and set the name of the Data Members and function as given in the above discription.

********************** Code **********************
*/

#include <bits/stdc++.h> 
class Rectangle {
    public : 
    int length;
    int breadth;
    int getArea(){
        int area = (length * breadth);
        return area;
    }
};



/*
Question 5 :

Coding Ninja : Duplicate Emails

Write a SQL query to find all duplicate emails in a table named Person.

+----+---------+
| Id | Email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
For example, your query should return the following for the above table:

+---------+
| Email   |
+---------+
| a@b.com |
+---------+

********************** Query **********************
*/

SELECT Email FROM Person 
GROUP BY Email 
HAVING COUNT(Email) > 1;
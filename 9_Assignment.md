/*
Question 1: 

LeetCode : 547. Number of Provinces
 
There are n cities. Some of them are connected, while some are not. If city a is connected directly with city b, and city b is connected directly with city c, then city a is connected indirectly with city c.
A province is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an n x n matrix isConnected where isConnected[i][j] = 1 if the ith city and the jth city are directly connected, and isConnected[i][j] = 0 otherwise.

Return the total number of provinces.

Example 1:

Input: isConnected = [[1,1,0],[1,1,0],[0,0,1]]
Output: 2

********************** Code **********************
*/

class Solution {
private:
    void dfs(int i, vector<int> &vis, vector<int> adj[]){
        vis[i] = 1;
        for(auto& it : adj[i]){
            if(!vis[it]){
                dfs(it, vis, adj);
            }
        }
    }
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        int n = isConnected.size();

        vector<int> adj[n];
        for(int i = 0; i < n; i++){
            for(int j = 0; j < n; j++){
                if(isConnected[i][j] == 1 && i != j) adj[i].push_back(j);
            }
        }

        int cnt = 0;
        vector<int> vis(n, 0);
        for(int i = 0; i < n; i++){
            if(!vis[i]){
                cnt++;
                dfs(i, vis, adj);
            }
        }

        return cnt;
    }
};



/*
Question 2 : 

LeetCode : 419. Battleships in a Board

Given an m x n matrix board where each cell is a battleship 'X' or empty '.', return the number of the battleships on board.

Battleships can only be placed horizontally or vertically on board. In other words, they can only be made of the shape 1 x k (1 row, k columns) or k x 1 (k rows, 1 column), where k can be of any size. At least one horizontal or vertical cell separates between two battleships (i.e., there are no adjacent battleships).

Example 1:

Input: board = [["X",".",".","X"],[".",".",".","X"],[".",".",".","X"]]
Output: 2

********************** Code **********************
*/

class Solution {
private:
    void bfs(int i, int j, int n, int m, vector<vector<int>> &vis, vector<vector<char>> grid, int drow[], int dcol[]){
        vis[i][j] = 1;
        queue<pair<int, int>> q;
        q.push({i, j});
        while(!q.empty()){
            int row = q.front().first;
            int col = q.front().second;
            q.pop();
            for(int i = 0; i < 4; i++){
                int nr = row + drow[i];
                int nc = col + dcol[i];
                if(nr >= 0 && nr < n && nc >= 0 && nc < m && !vis[nr][nc] && grid[nr][nc] == 'X'){
                    vis[nr][nc] = 1;
                    q.push({nr, nc});
                }
            }
        }
    }
public:
    int countBattleships(vector<vector<char>>& board) {
        int n = board.size();
        int m = board[0].size();

        vector<vector<int>> vis(n, vector<int> (m ,0));
        int drow[] = {-1, 0, 1, 0};
        int dcol[] = {0, 1, 0, -1};
        int cnt = 0;
        for(int i = 0; i < n; i++){
            for(int j = 0; j < m; j++){
                if(!vis[i][j] && board[i][j] == 'X'){
                    cnt++;
                    bfs(i, j, n, m, vis, board, drow, dcol);
                }
            }
        }
        return cnt;
    }
};



/*
Question 3 : 

LeetCode : 542. 01 Matrix

Given an m x n binary matrix mat, return the distance of the nearest 0 for each cell.
The distance between two cells sharing a common edge is 1.

Example 1:

Input: mat = [[0,0,0],[0,1,0],[0,0,0]]
Output: [[0,0,0],[0,1,0],[0,0,0]]

********************** Code **********************
*/

class Solution {
public:
    vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
        int n = mat.size();
        int m = mat[0].size();

        queue<pair<pair<int, int>, int>> q;
        vector<vector<int>> vis(n, vector<int> (m, 0));
        vector<vector<int>> dis(n, vector<int> (m, 0));
        int drow[] = {-1, 0, 1, 0};
        int dcol[] = {0, 1, 0, -1};
        for(int i = 0; i < n; i++){
            for(int j = 0; j < m; j++){
                if(mat[i][j] == 0){
                    q.push({{i, j}, 0});
                    vis[i][j] = 1;
                }
            }
        }

        while(!q.empty()){
            int row = q.front().first.first;
            int col = q.front().first.second;
            int step = q.front().second;
            dis[row][col] = step;
            q.pop(); 
            for(int i = 0; i < 4; i++){
                int nr = row + drow[i];
                int nc = col + dcol[i];
                if(nr >= 0 && nr < n && nc >= 0 && nc < m && !vis[nr][nc]){
                    vis[nr][nc] = 1;
                    q.push({{nr, nc}, step + 1});
                }
            }
        }
        return dis;
    }
};



/*
Question 4 : 

LeetCode : 449. Serialize and Deserialize BST

Serialization is converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary search tree. There is no restriction on how your serialization/deserialization algorithm should work. You need to ensure that a binary search tree can be serialized to a string, and this string can be deserialized to the original tree structure.

The encoded string should be as compact as possible.

Example 1:

Input: root = [2,1,3]
Output: [2,1,3]

********************** Code **********************
*/

class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string s = "";
        if(root == NULL) return s;

        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            TreeNode* node = q.front();
            q.pop();
            if(node == NULL) s += "#,";
            else s += (to_string(node -> val) + ',');

            if(node){
                q.push(node -> left);
                q.push(node -> right);
            }
        }
        return s;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if(data.size() == 0) return NULL;
        vector<string> s;
        string str = "";
        for(int i = 0; i < data.length(); i++){
            if(data[i] != ',') str += data[i];
            else if(data[i] == ',' || (i + 1) == data.length()) {
                s.push_back(str);
                str = "";
            }
        }
        int i = 0;
        TreeNode* root = new TreeNode(stoi(s[i++]));
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            TreeNode* node = q.front();
            q.pop();

            if(s[i] == "#") node -> left = NULL;
            else {
                TreeNode* leftnode = new TreeNode(stoi(s[i]));
                node -> left = leftnode;
                q.push(leftnode);
            }
            i++;

            if(s[i] == "#") node -> right = NULL;
            else {
                TreeNode* rightnode = new TreeNode(stoi(s[i]));
                node -> right = rightnode;
                q.push(rightnode);
            }
            i++;
        }
        return root;
    }
};



/*
Question 5 : 

Coding Ninja : Big Countries

Problem statement
There is a table World

+-----------------+------------+------------+--------------+---------------+
| name            | continent  | area       | population   | gdp           |
+-----------------------+------------------+-----------------+-------------+

| Afghanistan     | Asia       | 652230     | 25500100    |20343000  | 
| Albania         | Europe     | 28748      | 2831741     | 12960000  |
| Algeria         | Africa     | 2381741    | 37100000    | 188681000 |    
| Andorra         | Europe     | 468        | 78115       | 3712000   |
| Angola          | Africa     | 1246700    | 20609294    | 100990000 |    
+-----------------+------------+------------+--------------+---------------+

A country is big if it has an area of bigger than 3 million square km or a population of more than 25 million.

Write a SQL solution to output big countries' name, population and area.

********************** Query **********************
*/

SELECT name, population, area FROM World 
WHERE area > 3000000 OR population > 25000000;
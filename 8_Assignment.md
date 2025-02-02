/*
Question 1 : 

LeetCode : 1752. Check if Array Is Sorted and Rotated

Given an array nums, return true if the array was originally sorted in non-decreasing order, then rotated some number of positions (including zero). Otherwise, return false.

There may be duplicates in the original array.

Note: An array A rotated by x positions results in an array B of the same length such that A[i] == B[(i+x) % A.length], where % is the modulo operation.

********************** Code **********************
*/

class Solution {
public:
    bool check(vector<int>& nums) {
        int count = 0;
        int n = nums.size();
        for(int i = 1; i < n; i++){
            if(nums[i - 1] > nums[i]){
                count++;
            }
        }
        if(nums[n - 1] > nums[0]){
            count++;
        }
        return count <= 1;
    }
};



/*
Question 2 : 

LeetCode : 3442. Maximum Difference Between Even and Odd Frequency I

You are given a string s consisting of lowercase English letters. Your task is to find the maximum difference between the frequency of two characters in the string such that:

One of the characters has an even frequency in the string.
The other character has an odd frequency in the string.
Return the maximum difference, calculated as the frequency of the character with an odd frequency minus the frequency of the character with an even frequency.

********************** Code **********************
*/

class Solution {
public:
    int maxDifference(string s) {
        map<int, int> m;
        for(auto& mp : s){
            m[mp]++;
        }

        int mineven = INT_MAX;
        for(auto& [key, value] : m){
            if(value % 2 == 0) mineven = min(mineven, value);
        }
        int maxodd = 0;
        for(auto& [key, value] : m){
            if(value % 2 != 0) maxodd = max(maxodd, value);
        }

        return maxodd - mineven;
    }
};



/*
Question 3 : 

LeetCode : 130. Surrounded Regions

You are given an m x n matrix board containing letters 'X' and 'O', capture regions that are surrounded:

Connect: A cell is connected to adjacent cells horizontally or vertically.
Region: To form a region connect every 'O' cell.
Surround: The region is surrounded with 'X' cells if you can connect the region with 'X' cells and none of the region cells are on the edge of the board.
To capture a surrounded region, replace all 'O's with 'X's in-place within the original board. You do not need to return anything.

Example 1:

Input: board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]
Output: [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]

********************** Code **********************
*/

class Solution {
private:
    void dfs(int row, int col, int n, int m, vector<vector<int>> &vis, vector<vector<char>> board, int drow[], int dcol[]){
        vis[row][col] = 1;
        for(int i = 0; i < 4; i++){
            int nr = row + drow[i];
            int nc = col + dcol[i];
            if(nr >= 0 && nr < n && nc >= 0 && nc < m && !vis[nr][nc] && board[nr][nc] == 'O'){
                dfs(nr, nc, n, m, vis, board, drow, dcol);
            }
        }
    }

public:
    void solve(vector<vector<char>>& board) {
        int n = board.size();
        int m = board[0].size();

        vector<vector<int>> vis(n, vector<int> (m, 0));
        int drow[] = {-1, 0, 1, 0};
        int dcol[] = {0, 1, 0, -1};
        for(int i = 0; i < n; i++){
            for(int j = 0; j < m; j++){
                if(i == 0 || j == 0 || i == n - 1 || j == m - 1){
                    if(!vis[i][j] && board[i][j] == 'O'){
                        dfs(i, j, n, m, vis, board, drow, dcol);
                    }
                }
            }
        }

        for(int i = 0; i < n; i++){
            for(int j = 0; j < m; j++){
                if(!vis[i][j] && board[i][j] == 'O') board[i][j] = 'X';
            }
        }
    }
};


/*
Question 4 : 

LeetCode : 297. Serialize and Deserialize Binary Tree

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

Clarification: The input/output format is the same as how LeetCode serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

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
            if(node != NULL){
                q.push(node -> left);
                q.push(node -> right);
            }
        }
        return s;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if(data.size() == 0) return NULL;
        stringstream s(data);
        string str;
        getline(s, str, ',');
        TreeNode* root = new TreeNode(stoi(str));
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            TreeNode* node = q.front();
            q.pop();
            getline(s, str, ',');
            if(str == "#") node -> left = NULL;
            else {
                TreeNode* leftnode = new TreeNode(stoi(str));
                node -> left = leftnode;
                q.push(leftnode);
            }

            getline(s, str, ',');
            if(str == "#") node -> right = NULL;
            else {
                TreeNode* rightnode = new TreeNode(stoi(str));
                node -> right = rightnode;
                q.push(rightnode);
            }
        }
        return root;
    }
};



/*
Question 5 : 

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

********************** Query **********************
*/

SELECT c.name AS Customers FROM 
Customers AS c LEFT JOIN Orders AS o
ON c.id = o.customerId
WHERE o.customerId is NULL;
/*
Question 1 :

LeetCode : 1254. Number of Closed Islands

Given a 2D grid consists of 0s (land) and 1s (water).  An island is a maximal 4-directionally connected group of 0s and a closed island is an island totally (all left, top, right, bottom) surrounded by 1s.

Return the number of closed islands.

Example 1:

Input: grid = [[1,1,1,1,1,1,1,0],[1,0,0,0,0,1,1,0],[1,0,1,0,1,1,1,0],[1,0,0,0,0,1,0,1],[1,1,1,1,1,1,1,0]]
Output: 2
*/

class Solution {
private:
    void bfs(int r, int c, int n, int m, vector<vector<int>> &vis, vector<vector<int>> grid, int drow[], int dcol[]){
        queue<pair<int, int>> q;
        q.push({r, c});
        vis[r][c] = 1;
        while(!q.empty()){
            int row = q.front().first;
            int col = q.front().second;
            q.pop();
            for(int i = 0; i < 4; i++){
                int nr = row + drow[i];
                int nc = col + dcol[i];
                if(nr >= 0 && nr < n && nc >= 0 && nc < m && !vis[nr][nc] && grid[nr][nc] == 0){
                    vis[nr][nc] = 1;
                    q.push({nr, nc});
                }
            }
        }
    }
public:
    int closedIsland(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();

        
        int drow[] = {-1, 0, 1, 0};
        int dcol[] = {0, 1, 0, -1};
        vector<vector<int>> vis(n, vector<int> (m, 0));
        for(int i = 0; i < n; i++){
            for(int j = 0; j < m; j++){
                if(i == 0 || j == 0 || i == n - 1 || j == m - 1){
                    if(grid[i][j] == 0){
                        bfs(i, j, n, m, vis, grid, drow, dcol);
                    }
                }
            }
        }
        
        
        int cnt = 0;
        for(int i = 0; i < n; i++){
            for(int j = 0; j < m; j++){
                if(grid[i][j] == 0 && !vis[i][j]){
                    cnt++;
                    bfs(i, j, n, m, vis, grid, drow, dcol);
                }
            }
        }
        return cnt;
    }
};


/*
Question 2 : 

LeetCode : 994. Rotting Oranges

You are given an m x n grid where each cell can have one of three values:

0 representing an empty cell,
1 representing a fresh orange, or
2 representing a rotten orange.
Every minute, any fresh orange that is 4-directionally adjacent to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return -1.

Example 1:

Input: grid = [[2,1,1],[1,1,0],[0,1,1]]
Output: 4
*/

class Solution {
public:
    int orangesRotting(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();

        vector<vector<int>> vis(n, vector<int> (m, 0));
        queue<pair<pair<int, int>, int>> q;
        for(int i = 0; i < n; i++){
            for(int j = 0; j < m; j++){
                if(grid[i][j] == 2){
                    vis[i][j] = 1;
                    q.push({{i, j}, 0});
                }
            }
        }

        int drow[] = {-1, 0, 1, 0};
        int dcol[] = {0, 1, 0, -1};
        int tm = 0;
        while(!q.empty()){
            int row = q.front().first.first;
            int col = q.front().first.second;
            int time = q.front().second;
            q.pop();
            tm = max(tm, time);
            for(int i = 0; i < 4; i++){
                int nr = row + drow[i];
                int nc = col + dcol[i];
                if(nr >= 0 && nr < n && nc >= 0 && nc < m && grid[nr][nc] == 1 && !vis[nr][nc]){
                    vis[nr][nc] = 1;
                    q.push({{nr, nc}, time + 1});
                }
            }
        }

        for(int i = 0; i < n; i++){
            for(int j = 0; j < m; j++){
                if(!vis[i][j] && grid[i][j] == 1) return -1;
            }
        }

        return tm;
    }
};



/*
Question 3 : 

LeetCode : 1020. Number of Enclaves

You are given an m x n binary matrix grid, where 0 represents a sea cell and 1 represents a land cell.

A move consists of walking from one land cell to another adjacent (4-directionally) land cell or walking off the boundary of the grid.

Return the number of land cells in grid for which we cannot walk off the boundary of the grid in any number of moves.

Example 1:

Input: grid = [[0,0,0,0],[1,0,1,0],[0,1,1,0],[0,0,0,0]]
Output: 3
*/

class Solution {
private:
    void dfs(int row, int col, int n, int m, vector<vector<int>> &vis, vector<vector<int>> &grid, int drow[], int dcol[]){
        vis[row][col] = 1;
        for(int i = 0; i < 4; i++){
            int nr = row + drow[i];
            int nc = col + dcol[i];
            if(nr >= 0 && nr < n && nc >= 0 && nc < m && grid[nr][nc] == 1 && !vis[nr][nc]){
                dfs(nr, nc, n, m, vis, grid, drow, dcol);
            }
        }
    }
public:
    int numEnclaves(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();

        vector<vector<int>> vis(n, vector<int> (m, 0));
        int drow[] = {-1, 0, 1, 0};
        int dcol[] = {0, 1, 0, -1};
        for(int i = 0; i < n; i++){
            for(int j = 0; j < m; j++){
                if(i == 0 || j == 0 || i == n - 1 || j == m - 1){
                    if(!vis[i][j] && grid[i][j] == 1){
                        dfs(i, j, n, m, vis, grid, drow, dcol);
                    }
                }
            }
        }

        int cnt = 0;
        for(int i = 0; i < n; i++){
            for(int j = 0; j < m; j++){
                if(grid[i][j] == 1 && !vis[i][j]) cnt++;
            }
        }

        return cnt;
    }
};



/*
Question 4 :

Coding Ninja : Marvel Cities

Problem statement
Query all columns for all Marvel cities in the CITY table with populations larger than 100000. The CountryCode for Marvel is Marv.

The CITY table is described as follows:

+---------+--------+
| Field  |  Type    |
+---------+--------+
|   ID    |  Number  | 
|   Name  | Varchar  |
|   CountryCode | Varchar  |
|   Population |   Number  | 
+---------+--------+-------+
*/

SELECT * FROM CITY 
WHERE CountryCode = 'Marv' AND Population > 100000;
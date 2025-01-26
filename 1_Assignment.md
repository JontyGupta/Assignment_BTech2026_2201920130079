/*
Question 1 : 

3432. Count Partitions with Even Sum Difference

You are given an integer array nums of length n.

A partition is defined as an index i where 0 <= i < n - 1, splitting the array into two non-empty subarrays such that:

Left subarray contains indices [0, i].
Right subarray contains indices [i + 1, n - 1].
Return the number of partitions where the difference between the sum of the left and right subarrays is even.
*/

********************** Code **********************

class Solution {
public:
    int countPartitions(vector<int>& nums) {
        int n = nums.size();
        int  sum = 0;
        for(int i = 0; i < n; i++){
            sum += nums[i];
        }

        int count = 0;
        int s = 0;
        for(int i = 0; i < n - 1; i++){
            sum -= nums[i];
            s += nums[i];
            if((sum - s) % 2 == 0) count++;
        }
        return count;
    }
};


/*
Question 2 : 

590. N-ary Tree Postorder Traversal

Given the root of an n-ary tree, return the postorder traversal of its nodes' values.

Nary-Tree input serialization is represented in their level order traversal. Each group of children is separated by the null value (See examples)
*/

********************** Code ********************** 

class Solution {
private:
    void postOrder(Node* root, vector<int> &ans){
        if(root == NULL) return;
        vector<Node*> nodes = root -> children;
        int n = nodes.size();
        for(int i = 0; i < n; i++){
            postOrder(nodes[i], ans);
        }
        ans.push_back(root -> val);
    }

public:
    vector<int> postorder(Node* root) {
        vector<int> ans;
        postOrder(root, ans);
        return ans;
    }
};


/*
Question 3 : 

235. Lowest Common Ancestor of a Binary Search Tree

Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”
*/

********************** Code **********************

class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == NULL) return root;

        int curr = root -> val;
        if(curr < p -> val && curr < q -> val){
            return lowestCommonAncestor(root -> right, p, q);
        }

        if(curr > p -> val && curr > q -> val){
            return lowestCommonAncestor(root -> left, p, q);
        }

        return root;
    }
};


/*
Question 4 : System Design

3408. Design Task Manager

There is a task management system that allows users to manage their tasks, each associated with a priority. The system should efficiently handle adding, modifying, executing, and removing tasks.

Implement the TaskManager class:

TaskManager(vector<vector<int>>& tasks) initializes the task manager with a list of user-task-priority triples. Each element in the input list is of the form [userId, taskId, priority], which adds a task to the specified user with the given priority.

void add(int userId, int taskId, int priority) adds a task with the specified taskId and priority to the user with userId. It is guaranteed that taskId does not exist in the system.

void edit(int taskId, int newPriority) updates the priority of the existing taskId to newPriority. It is guaranteed that taskId exists in the system.

void rmv(int taskId) removes the task identified by taskId from the system. It is guaranteed that taskId exists in the system.

int execTop() executes the task with the highest priority across all users. If there are multiple tasks with the same highest priority, execute the one with the highest taskId. After executing, the taskId is removed from the system. Return the userId associated with the executed task. If no tasks are available, return -1.

Note that a user may be assigned multiple tasks.
*/

********************** Code **********************

class TaskManager {
private:
    priority_queue<pair<int, int>> pq;
    map<int, vector<int>> m;
    
public:
    TaskManager(vector<vector<int>>& tasks) {
        int n = tasks.size();
        for(int i = 0; i < n; i++){
            m[tasks[i][1]] = tasks[i];
        }

        for(int i = 0; i < n; i++){
            pq.push({tasks[i][2], tasks[i][1]});
        }
    }
    
    void add(int userId, int taskId, int priority) {
        pq.push({priority, taskId});
        m.insert({taskId, {userId, taskId, priority}});
    }
    
    void edit(int taskId, int newPriority) {
        m[taskId][2] = newPriority;
        pq.push({newPriority, taskId});
    }
    
    void rmv(int taskId) {
        m.erase(taskId);
    }
    
    int execTop() {
        if(pq.empty()) return -1;
        pair<int, int> val;
        while(!pq.empty()){
            val = pq.top();
            pq.pop();
            if(m.find(val.second) != m.end() && (m[val.second][2] == val.first)){
                int ans = m[val.second][0];
                m.erase(val.second);
                return ans;
            }
        }
        return -1;
    }
};
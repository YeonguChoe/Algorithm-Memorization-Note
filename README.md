# 알고리즘 암기 노트

### DFS
- Type: Tree, 2D Matrix, Graph
- 방문 노드 표시 필요

### Iterative DFS
```cpp
struct Node
{
    int value;
    vector<Node *> adjacent_nodes;
};

void dfs(Node *start, set<Node *> &visited)
{

    stack<Node *> S;
    S.push(start);

    while (!S.empty())
    {
        Node *v = S.top();
        S.pop();

        if (find(visited.begin(), visited.end(), v) == visited.end())
        {
            visited.insert(v);
            for (Node *w : v->adjacent_nodes)
            {
                S.push(w);
            }
        }
    }
}
```


- Implementation type: Stack DFS, Recursive DFS

### Stack DFS




### Backtracking
- 내용: 노드를 붙였다 뗏다가를 반복함.
- Usage: Permutation, Combination

#### From LC 39

```cpp
void backtrack(int current_sum, int goal, int current_start_of_candidate,
               vector<int>& combination, vector<int>& candidate,
               vector<vector<int>>& result) {
    // 합이 정답에 도달하면, 해당 조합을 답에 추가
    if (current_sum == goal) {
        result.push_back(combination);
        return;
    }
    // 합이 정답을 넘어서면, 더이상 들어가지 않음.
    if (current_sum > goal) {
        return;
    }
    // candidate을 combination에 붙였다가 뗏다가 하면서, 함수 실행
    for (int i = current_start_of_candidate; i < candidate.size(); ++i) {
        combination.push_back(candidate[i]);
        backtrack(current_sum + candidate[i], goal, i, combination, candidate,
                  result);
        combination.pop_back();
    }
}
vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
    vector<vector<int>> result;
    vector<int> combination;
    backtrack(0, target, 0, combination, candidates, result);
    return result;
}
```

#### From LC 2028
```cpp
// Time complexity: O(6^n)
// Space complexity: O(n)

bool backtracking(int current_sum, int goal, int remaining_n,
                  vector<int>& combination, vector<int>& answer) {
    // 나머지 횟수가 0이고 합이 goal에 도달한 경우.
    if (remaining_n == 0 and current_sum == goal) {
        answer = combination;
        return true;
    }
    // 나머지 횟수만 다 소진한 경우.
    if (remaining_n == 0) {
        return false;
    }

    // 나머지 횟수가 남았지만, 이미 합이 초과한 경우
    if (current_sum > goal) {
        return false;
    }

    // 값을 붙였다가 뗏다가 하면서 백트래킹
    for (int i = 6; i > 0; --i) {
        combination.push_back(i);
        if (backtracking(current_sum + i, goal, remaining_n - 1,
                         combination, answer)) {
            return true;
        }
        combination.pop_back();
    }

    return false;
}
```

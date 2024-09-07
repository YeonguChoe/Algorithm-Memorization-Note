# 알고리즘 암기 노트

- Recursion == Stack + Iteration ???
- Graph Algorithm 분류

### DFS
- Type: Tree, 2D Matrix, Graph
- 방문 노드 표시 필요

### DFS type based on Implementation
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

    // 스택에서 한개씩 빼서 관찰
    while (!S.empty())
    {
        Node *v = S.top();
        S.pop();

        // 만약에 방문한적이 있는 노드가 아니라면, 해당 노드를 방문했다고 표시하고, 인접한 노드들을 스택에 추가
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

### Recursive DFS

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

#### From LC 79 Word Search

```cpp
// Time complexity: O(row*column*(4^길이))
// Space complexity: O(길이)

bool backtrack(vector<vector<char>> &board, const string &word,
               int current_row, int current_column, int index)
{
    // 이전 iteration까지 backtrack을 통과한 경우
    // index가 단어의 길이와 같아지는 경우
    if (index == word.size())
    {
        return true;
    }

    // 탐색중인 cell이 board를 넘어선 경우
    if (current_row < 0 || current_row >= board.size() ||
        current_column < 0 || current_column >= board[0].size())
    {
        return false;
    }

    // 탐색중인 cell에 있는 문자가 단어의 문자와 다른 경우
    if (board[current_row][current_column] != word[index])
    {
        return false;
    }

    // 탐색중인 cell의 문자를 글자가 아닌것으로 지정하여, 방문 표시
    char temp = board[current_row][current_column];
    board[current_row][current_column] = '#';

    // 탐색중인 cell의 주변을 backtrack으로 탐색
    bool found =
        backtrack(board, word, current_row - 1, current_column,
                  index + 1) or
        backtrack(board, word, current_row, current_column - 1,
                  index + 1) or
        backtrack(board, word, current_row + 1, current_column,
                  index + 1) or
        backtrack(board, word, current_row, current_column + 1, index + 1);

    // 탐색중인 cell의 값을 원래대로 돌려 놓음
    board[current_row][current_column] = temp;

    // 만약에 찾아진 경우, true를 위로 올림
    // 만약에 탐색 도중에 2가지 false를 반환하는 케이스에 걸렸으면 false를 위로 올림
    return found;
}

bool exist(vector<vector<char>> &board, string word)
{

    // 시작점을 설정
    for (int row = 0; row < board.size(); ++row)
    {
        for (int col = 0; col < board[0].size(); ++col)
        {
            // 만약에 backtrack 함수가 true인 경우를 찾았으면, true 반환
            if (backtrack(board, word, row, col, 0))
            {
                return true;
            }
        }
    }
    // true인 경우를 한번도 찾지 못했다면, false 반환
    return false;
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

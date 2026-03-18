Design a program to find the shortest path in an unweighted graph from a given
source node to a destination node using Breadth First Search.
#include <iostream>
#include <algorithm>
#include <vector>
#include <queue>
using namespace std;
void bfs(int src,int dest,vector<vector<int>> &adj,vector<int> &dist,vector<int> &parents){
    
    queue<int> q;
    dist[src]=0;
    q.push(src);
    while(!q.empty()){
        int u=q.front();
        q.pop();
        for(int adjacent:adj[u]){
            if(dist[adjacent]==-1){
                dist[adjacent]=dist[u]+1;
                q.push(adjacent);
                parents[adjacent]=u;
            }
        }
    }
    if(dist[dest]==-1){
        cout<<"Path not Exist"<<endl;
    }
    cout<<"Shortes Distance "<<dist[dest]<<endl;

    vector<int> path;
    for(int i=dest ; i!=-1 ; i=parents[i]){
        path.push_back(i);
    }
    reverse(path.begin(),path.end());
    cout<<"Path Is ";
    for(auto k :path){
        cout<<k <<" ";
    }

}



int main(){
    int V,E;
    cout<<"Entre the number of vertices and Edges :->"<<endl;
    cin>>V>>E;
    vector<vector<int>> adj(V);
    for(int i=0;i<E;i++){
        int u,v;
        cout<<"Entre the Edge (u,v) ";
        cin>>u>>v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    for(int i=0;i<V;i++){
        cout<<i<<"-> ";
        for(int j:adj[i]){
            cout<<j<<" ";
        }
        cout<<endl;
    }
    vector<int>parents(V,-1);
    vector<int>dist(V,-1);
    int src,dest;
    cout<<"Entre the source and destination ";
    cin>>src>>dest;
    bfs(src,dest,adj,dist,parents);

}

Implement Depth First Search to explore all reachable nodes in a graph starting
from a given node and detect cycles.
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
void dfs(int node,int parent,vector<vector<int>> &adj,vector<bool> &visited,bool &hascycles){
    visited[node]=true;
    for(int adjacent:adj[node]){
        if(!visited[adjacent]){
            dfs(adjacent,parent,adj,visited,hascycles);
        }
        else if(adjacent!=parent){
            hascycles=true;
        }
    }

}

int main(){
    int V,E;
    cout<<"Entre the number of vertices and Edges :->"<<endl;
    cin>>V>>E;
    vector<vector<int>> adj(V);
    for(int i=0;i<E;i++){
        int u,v;
        cout<<"Entre the Edge (u,v) ";
        cin>>u>>v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    for(int i=0;i<V;i++){
        cout<<i<<"-> ";
        for(int j:adj[i]){
            cout<<j<<" ";
        }
        cout<<endl;
    }
    bool hascycles=false;
    vector<bool> visited(V,false);
    int node;
    int parent=-1;
    cout<<"Entre Starting Node"<<endl;
    cin>>node;

    dfs(node,parent,adj,visited,hascycles);
    if(hascycles){
        cout<<"Cycles Exist :)"<<endl;
    }
    else {
        cout<<"Not Exist :("<<endl;
    }


}

Develop a program to determine the least-cost path between two nodes in a
weighted graph where edge costs vary, using Uniform Cost Search.
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>
#include <climits>
using namespace std;
void bfs(int src,int dest,vector<vector<pair<int,int>>> &adj,int V){
    vector<int>parents(V,-1);
    vector<int>cost(V,INT_MAX);
    priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>>pq;
    pq.push({0,src});
    cost[src]=0;
    while(!pq.empty()){
        auto top=pq.top();
        pq.pop();
        int currweight=top.first;
        int u=top.second;
        if(u==dest){
            break;
        }
        for(auto edge:adj[u]){
            int weight=edge.first;
            int v=edge.second;
            if(cost[v]>currweight+weight){
                cost[v]=currweight+weight;
                pq.push({cost[v],v});
                parents[v]=u;
            }
        }
    }
    vector<int> path;
    for(int i=dest;i!=-1;i=parents[i]){
        path.push_back(i);
    }
    reverse(path.begin(),path.end());
    cout<<"Shortest Distance is "<<cost[dest]<<endl;
    cout<<"Path "<<endl;
    for(int p:path){
        cout<<p<<" ";
    }
}
int main(){
    int V,E;
    cout<<"Entre the number of Vertices and Edges"<<endl;
    cin>>V>>E;
    vector<vector<pair<int,int>>> adj(V);
    for(int i=0 ;i<E ;i++){
        cout<<"Entre the edge (u,v,weight)"<<endl;
        int u,v,w;
        cin>>u>>v>>w;
        adj[u].push_back({w,v});
    }
    for(int i=0;i<V;i++){
        cout<<i<<"-> ";
        for(auto j:adj[i]){
            cout<<j.first<<" "<<j.second<<" ";
        }
        cout<<endl;
    }
    cout<<"Entre Source and destination"<<endl;
    int src,dest;
    cin>>src>>dest;
    bfs(src,dest,adj,V);
    
}


Write a program to search for a target node in a tree structure using Depth Limited
Search, with a user-defined depth limit.

#include <iostream>
#include <vector>
int max_depth=1000;
using namespace std;

bool dls(int node,int target,vector<vector<int>> &adj,int depthlimit){
    if(node==target){
        return true;
    }
    if(depthlimit<=0){
        return false;
    }
    for(int j:adj[node]){
        if(dls(j,target,adj,depthlimit-1)){
            return true;
        }
    }
    return false;
}
bool iddfs(int node,int target,vector<vector<int>>&adj)
{
    for(int depth=0;depth<max_depth;depth++){
            if(dls(node,target,adj,depth)){
                return true;
            }
    }
    return false;
}
int main(){
    int V,E;
    cout<<"Entre the number of vertices and edges:-> "<<endl;
    cin>>V>>E;
    vector<vector<int> >adj(V);
    for(int i=0;i<E;i++){
        cout<<"Entre the edge(u,v) "<<endl;
        int u,v;
        cin>>u>>v;
        adj[u].push_back(v);
    }
    for(int i=0;i<V;i++){
        cout<<i<<"-> ";
        for(int j:adj[i]){
            cout<<j<<" ";
        }
        cout<<endl;
    }
    cout<<"Entre the target element"<<endl;
    int target;
    cin>>target;
    if(iddfs(0,target,adj)){
        cout<<"Found :)"<<endl;
    }
    else {
        cout<<"Not Found";
    }
}

Implement the A* search algorithm to find the optimal path between two locations
in a graph using a heuristic function.

#include <iostream>
#include <climits>
#include <queue>
#include <vector>
#include <algorithm>
using namespace std;
void astar(int start,int target,vector<vector<pair<int,int> >>&adj,vector<int>&heuristic,int  V){
    priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>>pq;
    vector<int>parent(V,-1);
    vector<int> g(V,INT_MAX);
    g[start]=0;
    pq.push({heuristic[start],start});
    while(!pq.empty()){
        auto top=pq.top();
        pq.pop();
        int node=top.second;
    
        if(node==target){
            break;
        }
        for(auto neighbour:adj[node]){
            int next=neighbour.first;
            int weight=neighbour.second;
            int current_weight=neighbour.second;
            if(g[next]>weight+current_weight){
                g[next]=weight+current_weight;
                int fn=heuristic[next]+g[next];
                pq.push({fn,next});
                parent[next]=node;
            }
        }
    }
    if(g[target]==INT_MAX){
        cout<<"Not found"<<endl;
    }
    vector<int> path;
    for(int i=target ;i!=-1;i=parent[i]){
        path.push_back(i);
    }
    reverse(path.begin(),path.end());
    for(int j: path){
        cout<<j<<" ";
    }

}
int main(){
    int V,E;
    cout<<"Entre the number of Vertices and Edges "<<endl;
    cin>>V>>E;
    vector<vector<pair<int,int>>> adj(V);
    for(int i=0 ;i<E ;i++){
        cout<<"Entre Edge and weight (u,v,w) "<<endl;
        int u,v,w;
        cin>>u>>v>>w;
        adj[u].push_back({v,w});
    }
    vector<int> heuristic(V);
    for(int i=0;i<V;i++){
        int h;
        cout<<"Entre h("<<i<<") ";
        cin>>h;
        heuristic[i]=h;
    }
    astar(0,3,adj,heuristic,V);
}

Implement a solution for the N-Queens problem using Hill Climbing techniques
#include <iostream>
#include <vector>
#include <cstdlib>
#include <ctime>
using namespace std;

int calculateConflicts(vector<int>& board, int N) {
    int conflicts = 0;

    for (int i = 0; i < N; i++) {
        for (int j = i + 1; j < N; j++) {

            if (board[i] == board[j])
                conflicts++;

            if (abs(board[i] - board[j]) == abs(i - j))
                conflicts++;
        }
    }

    return conflicts;
}

void printBoard(vector<int>& board, int N) {
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            if (board[i] == j)
                cout << "Q ";
            else
                cout << ". ";
        }
        cout << endl;
    }
}

bool hillClimbing(int N) {

    vector<int> board(N);

    // Random initial state
    for (int i = 0; i < N; i++)
        board[i] = rand() % N;

    int currentConflicts = calculateConflicts(board, N);

    while (true) {

        if (currentConflicts == 0) {
            cout << "Solution Found:\n";
            printBoard(board, N);
            return true;
        }

        vector<int> bestBoard = board;
        int bestConflicts = currentConflicts;

        // Generate neighbours
        for (int row = 0; row < N; row++) {

            int originalCol = board[row];

            for (int col = 0; col < N; col++) {

                if (col == originalCol)
                    continue;

                board[row] = col;

                int newConflicts = calculateConflicts(board, N);

                if (newConflicts < bestConflicts) {
                    bestConflicts = newConflicts;
                    bestBoard = board;
                }
            }

            board[row] = originalCol;
        }

        if (bestConflicts >= currentConflicts) {
            cout << "Stuck in Local Minimum.\n";
            return false;
        }

        board = bestBoard;
        currentConflicts = bestConflicts;
    }
}

int main() {
    srand(time(0));

    int N;
    cout << "Enter value of N: ";
    cin >> N;

    if (!hillClimbing(N)) {
        cout << "Try running again (random restart).\n";
    }

    return 0;
}

Develop a program to play Tic-Tac-Toe using the Min–Max algorithm.
#include <iostream>
using namespace std;

char board[3][3];

// Print Board
void printBoard() {
    cout << "\n";
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++)
            cout << board[i][j] << " ";
        cout << "\n";
    }
    cout << "\n";
}

// Evaluate Board
int evaluate() {

    // Check rows and columns
    for (int i = 0; i < 3; i++) {

        if (board[i][0] == board[i][1] &&
            board[i][1] == board[i][2] &&
            board[i][0] != '_') {

            if (board[i][0] == 'O') return 10;
            else return -10;
        }

        if (board[0][i] == board[1][i] &&
            board[1][i] == board[2][i] &&
            board[0][i] != '_') {

            if (board[0][i] == 'O') return 10;
            else return -10;
        }
    }

    // Diagonals
    if (board[0][0] == board[1][1] &&
        board[1][1] == board[2][2] &&
        board[0][0] != '_') {

        if (board[0][0] == 'O') return 10;
        else return -10;
    }

    if (board[0][2] == board[1][1] &&
        board[1][1] == board[2][0] &&
        board[0][2] != '_') {

        if (board[0][2] == 'O') return 10;
        else return -10;
    }

    return 0;
}

// Check if moves left
bool isMovesLeft() {
    for (int i = 0; i < 3; i++)
        for (int j = 0; j < 3; j++)
            if (board[i][j] == '_')
                return true;
    return false;
}

// Minimax Function
int minimax(bool isMax) {

    int score = evaluate();

    if (score == 10 || score == -10)
        return score;

    if (!isMovesLeft())
        return 0;

    if (isMax) {
        int best = -1000;

        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {

                if (board[i][j] == '_') {

                    board[i][j] = 'O';
                    best = max(best, minimax(false));
                    board[i][j] = '_';
                }
            }
        }
        return best;
    }
    else {
        int best = 1000;

        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {

                if (board[i][j] == '_') {

                    board[i][j] = 'X';
                    best = min(best, minimax(true));
                    board[i][j] = '_';
                }
            }
        }
        return best;
    }
}

// Find Best Move for AI
void findBestMove() {

    int bestVal = -1000;
    int bestRow = -1, bestCol = -1;

    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {

            if (board[i][j] == '_') {

                board[i][j] = 'O';
                int moveVal = minimax(false);
                board[i][j] = '_';

                if (moveVal > bestVal) {
                    bestRow = i;
                    bestCol = j;
                    bestVal = moveVal;
                }
            }
        }
    }

    board[bestRow][bestCol] = 'O';
}

// Main Function
int main() {

    // Initialize board
    for (int i = 0; i < 3; i++)
        for (int j = 0; j < 3; j++)
            board[i][j] = '_';

    cout << "You are X, AI is O\n";

    while (true) {

        printBoard();

        int row, col;
        cout << "Enter your move (row col): ";
        cin >> row >> col;

        if (board[row][col] != '_') {
            cout << "Invalid move!\n";
            continue;
        }

        board[row][col] = 'X';

        if (evaluate() == -10) {
            printBoard();
            cout << "You Win!\n";
            break;
        }

        if (!isMovesLeft()) {
            printBoard();
            cout << "Draw!\n";
            break;
        }

        findBestMove();

        if (evaluate() == 10) {
            printBoard();
            cout << "AI Wins!\n";
            break;
        }

        if (!isMovesLeft()) {
            printBoard();
            cout << "Draw!\n";
            break;
        }
    }

    return 0;
}

Enhance the Min–Max implementation by incorporating Alpha–Beta Pruning.
(Implement Tic-Tac-Toe)

#include <iostream>
#include <vector>
#include <limits>
using namespace std;

#define AI 'X'
#define HUMAN 'O'
#define EMPTY '_'

// Print Board
void printBoard(vector<vector<char>>& board) {
    cout << "\n";
    for(int i = 0; i < 3; i++) {
        for(int j = 0; j < 3; j++) {
            cout << board[i][j] << " ";
        }
        cout << endl;
    }
    cout << endl;
}

// Check Winner
int evaluate(vector<vector<char>>& board) {

    // Rows
    for(int i = 0; i < 3; i++) {
        if(board[i][0] == board[i][1] &&
           board[i][1] == board[i][2]) {

            if(board[i][0] == AI) return 10;
            if(board[i][0] == HUMAN) return -10;
        }
    }

    // Columns
    for(int j = 0; j < 3; j++) {
        if(board[0][j] == board[1][j] &&
           board[1][j] == board[2][j]) {

            if(board[0][j] == AI) return 10;
            if(board[0][j] == HUMAN) return -10;
        }
    }

    // Diagonals
    if(board[0][0] == board[1][1] &&
       board[1][1] == board[2][2]) {

        if(board[0][0] == AI) return 10;
        if(board[0][0] == HUMAN) return -10;
    }

    if(board[0][2] == board[1][1] &&
       board[1][1] == board[2][0]) {

        if(board[0][2] == AI) return 10;
        if(board[0][2] == HUMAN) return -10;
    }

    return 0;
}

// Check if moves left
bool isMovesLeft(vector<vector<char>>& board) {
    for(int i = 0; i < 3; i++)
        for(int j = 0; j < 3; j++)
            if(board[i][j] == EMPTY)
                return true;
    return false;
}

// Minimax with Alpha-Beta
int minimax(vector<vector<char>>& board, int depth,
            bool isMax, int alpha, int beta) {

    int score = evaluate(board);

    // Terminal states
    if(score == 10) return score - depth;
    if(score == -10) return score + depth;
    if(!isMovesLeft(board)) return 0;

    if(isMax) {
        int best = numeric_limits<int>::min();

        for(int i = 0; i < 3; i++) {
            for(int j = 0; j < 3; j++) {

                if(board[i][j] == EMPTY) {

                    board[i][j] = AI;

                    best = max(best,
                        minimax(board, depth + 1, false, alpha, beta));

                    board[i][j] = EMPTY;

                    alpha = max(alpha, best);

                    // Prune
                    if(beta <= alpha)
                        return best;
                }
            }
        }
        return best;
    }
    else {
        int best = numeric_limits<int>::max();

        for(int i = 0; i < 3; i++) {
            for(int j = 0; j < 3; j++) {

                if(board[i][j] == EMPTY) {

                    board[i][j] = HUMAN;

                    best = min(best,
                        minimax(board, depth + 1, true, alpha, beta));

                    board[i][j] = EMPTY;

                    beta = min(beta, best);

                    // Prune
                    if(beta <= alpha)
                        return best;
                }
            }
        }
        return best;
    }
}

// Find Best Move
pair<int,int> findBestMove(vector<vector<char>>& board) {

    int bestVal = numeric_limits<int>::min();
    pair<int,int> bestMove = {-1, -1};

    for(int i = 0; i < 3; i++) {
        for(int j = 0; j < 3; j++) {

            if(board[i][j] == EMPTY) {

                board[i][j] = AI;

                int moveVal = minimax(board, 0, false,
                        numeric_limits<int>::min(),
                        numeric_limits<int>::max());

                board[i][j] = EMPTY;

                if(moveVal > bestVal) {
                    bestMove = {i, j};
                    bestVal = moveVal;
                }
            }
        }
    }

    return bestMove;
}

int main() {

    vector<vector<char>> board = {
        {EMPTY, EMPTY, EMPTY},
        {EMPTY, EMPTY, EMPTY},
        {EMPTY, EMPTY, EMPTY}
    };

    cout << "Tic-Tac-Toe (You = O, AI = X)\n";

    while(true) {

        printBoard(board);

        // Human move
        int r, c;
        cout << "Enter row and column (0-2): ";
        cin >> r >> c;
        board[r][c] = HUMAN;

        if(evaluate(board) == -10) {
            printBoard(board);
            cout << "You win!\n";
            break;
        }

        if(!isMovesLeft(board)) {
            printBoard(board);
            cout << "Draw!\n";
            break;
        }

        // AI move
        pair<int,int> bestMove = findBestMove(board);
        board[bestMove.first][bestMove.second] = AI;

        if(evaluate(board) == 10) {
            printBoard(board);
            cout << "AI wins!\n";
            break;
        }

        if(!isMovesLeft(board)) {
            printBoard(board);
            cout << "Draw!\n";
            break;
        }
    }

    return 0;
}

Design a Sudoku solver using Constraint Satisfaction techniques

#include <iostream>
#include <vector>
using namespace std;

#define N 9

// Check if safe
bool isSafe(vector<vector<int>>& board, int row, int col, int num) {

    // Row
    for(int x = 0; x < N; x++)
        if(board[row][x] == num)
            return false;

    // Column
    for(int x = 0; x < N; x++)
        if(board[x][col] == num)
            return false;

    // 3x3 Box
    int startRow = row - row % 3;
    int startCol = col - col % 3;

    for(int i = 0; i < 3; i++)
        for(int j = 0; j < 3; j++)
            if(board[i + startRow][j + startCol] == num)
                return false;

    return true;
}

// Find unassigned cell using MRV
bool findUnassigned(vector<vector<int>>& board, int &row, int &col) {

    int minDomain = 10;
    bool found = false;

    for(int i = 0; i < N; i++) {
        for(int j = 0; j < N; j++) {

            if(board[i][j] == 0) {

                int count = 0;
                for(int num = 1; num <= 9; num++)
                    if(isSafe(board, i, j, num))
      # Smoke-

//Simpler
// ================= 1_connected_components.cpp =================
#include <iostream>
using namespace std;

bool adj[1005][1005];
bool visited[1005];
int n, m;

void dfs(int u) {
    visited[u] = true;
    for (int v = 1; v <= n; v++) {
        if (adj[u][v] && !visited[v]) {
            dfs(v);
        }
    }
}

int main() {
    cin >> n >> m;
    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        adj[u][v] = true;
        adj[v][u] = true;
    }

    int components = 0;
    for (int i = 1; i <= n; i++) {
        if (!visited[i]) {
            components++;
            dfs(i);
        }
    }
    cout << components << "\n";
    return 0;
}


// ================= 2_bfs_shortest_path.cpp =================
#include <iostream>
#include <queue>
using namespace std;

bool adj[1005][1005];
int dist_[1005];
int n, m;

int main() {
    cin >> n >> m;
    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        adj[u][v] = true;
        adj[v][u] = true;
    }
    int s, t;
    cin >> s >> t;

    for (int i = 1; i <= n; i++) dist_[i] = -1;
    dist_[s] = 0;
    
    queue<int> q;
    q.push(s);

    while (!q.empty()) {
        int u = q.front(); 
        q.pop();
        for (int v = 1; v <= n; v++) {
            if (adj[u][v] && dist_[v] == -1) {
                dist_[v] = dist_[u] + 1;
                q.push(v);
            }
        }
    }
    cout << dist_[t] << "\n";
    return 0;
}


// ================= 3_dijkstra.cpp =================
#include <iostream>
using namespace std;

const int INF = 1e9;
int adj[1005][1005];
int dist_[1005];
bool visited[1005];
int n, m;

int main() {
    cin >> n >> m;
    
    // Initialize adjacency matrix and distances
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            adj[i][j] = INF;
        }
        dist_[i] = INF;
    }

    for (int i = 0; i < m; i++) {
        int u, v, w;
        cin >> u >> v >> w;
        if (w < adj[u][v]) { // Keep minimum weight for multiple edges
            adj[u][v] = w;
            adj[v][u] = w;
        }
    }
    int s;
    cin >> s;

    dist_[s] = 0;

    // O(V^2) Dijkstra (Much simpler, no priority_queue or vector needed)
    for (int i = 1; i <= n; i++) {
        int u = -1;
        for (int j = 1; j <= n; j++) {
            if (!visited[j] && (u == -1 || dist_[j] < dist_[u])) {
                u = j;
            }
        }

        if (dist_[u] == INF) break; // Remaining vertices are unreachable
        visited[u] = true;

        for (int v = 1; v <= n; v++) {
            if (adj[u][v] != INF && dist_[u] + adj[u][v] < dist_[v]) {
                dist_[v] = dist_[u] + adj[u][v];
            }
        }
    }

    cout << "Vertex : Distance\n";
    for (int i = 1; i <= n; i++) {
        cout << i << " : " << dist_[i] << "\n";
    }
    return 0;
}


// ================= 4_bellman_ford.cpp =================
#include <iostream>
using namespace std;

const int INF = 1e9;

struct Edge {
    int u, v, w;
};

Edge edges[100005]; // Fixed-size array instead of vector<array>
int dist_[1005];
int n, m;

int main() {
    cin >> n >> m;
    for (int i = 0; i < m; i++) {
        cin >> edges[i].u >> edges[i].v >> edges[i].w;
    }
    int s;
    cin >> s;

    for (int i = 1; i <= n; i++) dist_[i] = INF;
    dist_[s] = 0;

    for (int i = 1; i <= n - 1; i++) {
        for (int j = 0; j < m; j++) {
            int u = edges[j].u;
            int v = edges[j].v;
            int w = edges[j].w;
            if (dist_[u] != INF && dist_[u] + w < dist_[v]) {
                dist_[v] = dist_[u] + w;
            }
        }
    }

    bool negativeCycle = false;
    for (int j = 0; j < m; j++) {
        int u = edges[j].u;
        int v = edges[j].v;
        int w = edges[j].w;
        if (dist_[u] != INF && dist_[u] + w < dist_[v]) {
            negativeCycle = true;
            break;
        }
    }

    if (negativeCycle) {
        cout << "Negative Cycle Detected\n";
    } else {
        for (int i = 1; i <= n; i++) {
            cout << i << " : " << dist_[i] << "\n";
        }
    }
    return 0;
}


// ================= 5_floyd_warshall.cpp =================
#include <iostream>
using namespace std;

const int INF = 1e9;
int dist_[1005][1005];
int n, m;

int main() {
    cin >> n >> m;
    
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            dist_[i][j] = INF;
        }
        dist_[i][i] = 0;
    }

    for (int i = 0; i < m; i++) {
        int u, v, w;
        cin >> u >> v >> w;
        if (w < dist_[u][v]) {
            dist_[u][v] = w;
        }
    }

    for (int k = 1; k <= n; k++) {
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                if (dist_[i][k] != INF && dist_[k][j] != INF) {
                    if (dist_[i][k] + dist_[k][j] < dist_[i][j]) {
                        dist_[i][j] = dist_[i][k] + dist_[k][j];
                    }
                }
            }
        }
    }

    for (int

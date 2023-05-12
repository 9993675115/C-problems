# C-problems
#include <iostream>
#include <vector>
#include <queue>
#include <climits>

using namespace std;

int networkDelayTime(vector<vector<int>>& times, int n, int k) {
    vector<vector<pair<int, int>>> graph(n + 1);
    for (const auto& time : times) {
        int u = time[0];
        int v = time[1];
        int w = time[2];
        graph[u].push_back({v, w});
    }

    vector<int> dist(n + 1, INT_MAX);
    dist[k] = 0;

    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    pq.push({0, k});

    while (!pq.empty()) {
        int u = pq.top().second;
        int u_dist = pq.top().first;
        pq.pop();

        if (u_dist > dist[u]) {
            continue;
        }

        for (const auto& edge : graph[u]) {
            int v = edge.first;
            int w = edge.second;
            if (u_dist + w < dist[v]) {
                dist[v] = u_dist + w;
                pq.push({dist[v], v});
            }
        }
    }

    int max_time = 0;
    for (int i = 1; i <= n; i++) {
        if (dist[i] == INT_MAX) {
            return -1; // Not all nodes are reachable
        }
        max_time = max(max_time, dist[i]);
    }

    return max_time;
}

int main() {
    vector<vector<int>> times = {{2, 1, 2}, {2, 3, 1}, {3, 4, 1}};
    int n = 4;
    int k = 2;

    int result = networkDelayTime(times, n, k);
    cout << "Minimum time for all nodes to receive the signal: " << result << endl;

    return 0;
}

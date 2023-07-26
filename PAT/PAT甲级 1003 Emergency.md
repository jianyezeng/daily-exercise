# PAT甲级 1003 Emergency

[PTA | 程序设计类实验辅助教学平台 (pintia.cn)](https://pintia.cn/problem-sets/994805342720868352/exam/problems/994805523835109376?type=7&page=0)

## 题目

As an emergency rescue team leader of a city, you are given a special map of your country. The map shows several scattered cities connected by some roads. Amount of rescue teams in each city and the length of each road between any pair of cities are marked on the map. When there is an emergency call to you from some other city, your job is to lead your men to the place as quickly as possible, and at the mean time, call up as many hands on the way as possible.

**Input Specification:**

Each input file contains one test case. For each test case, the first line contains 4 positive integers: *N* (≤500) - the number of cities (and the cities are numbered from 0 to *N*−1), *M* - the number of roads, *C*1 and *C*2 - the cities that you are currently in and that you must save, respectively. The next line contains *N* integers, where the *i*-th integer is the number of rescue teams in the *i*-th city. Then *M* lines follow, each describes a road with three integers *c*1, *c*2 and *L*, which are the pair of cities connected by a road and the length of that road, respectively. It is guaranteed that there exists at least one path from *C*1 to *C*2.

**Output Specification:**

For each test case, print in one line two numbers: the number of different shortest paths between *C*1 and *C*2, and the maximum amount of rescue teams you can possibly gather. All the numbers in a line must be separated by exactly one space, and there is no extra space allowed at the end of a line.

**Sample Input：**

```in
5 6 0 2
1 2 1 5 3
0 1 1
0 2 2
0 3 1
1 2 1
2 4 1
3 4 1
```

**Sample Output:**

```out
2 4
```

## 代码

```c++
#include<iostream>
#include<list>
#include <climits>
using namespace std;
typedef struct{
    int weight;
    int dest;
}Edge;
struct PathInfo {
    int distance;
    int pointSum;
    int pathCount;
};
class Graph {
private:
    int num_nodes;
    int* num_eme;
    list<Edge> *adjlist;
public:
    Graph(int n){
        num_nodes = n;
        adjlist = new list<Edge>[num_nodes];
        num_eme = new int[num_nodes];
    }
    int get_numnode(){
        return num_nodes;
    }
    void add_path(int u, int v, int weight){
        Edge edge1;
        edge1.weight = weight;
        edge1.dest = v;
        adjlist[u].emplace_back(edge1);
        Edge edge2;
        edge2.weight = weight;
        edge2.dest = u;
        adjlist[v].emplace_back(edge2);
    }
    void print_graph(){
        for(int i=0; i<num_nodes; i++){
            list<Edge>::iterator it = adjlist[i].begin();
            while(it!=adjlist[i].end()){
                cout << i << " " << it->dest << " " << it->weight <<endl;
                it++;
            }
        }
    }
    void add_numeme(int i,int num) {
        num_eme[i] = num;
    }
    int get_numeme(int i){
        return num_eme[i];
    }
    list<Edge> get_edges(int i){
        return adjlist[i];
    }
};
void dijkstra(Graph g, int u, int v);
int main(){
    int num_nodes,num_paths,ori,dect;
    int temp = 0;
    cin >> num_nodes >> num_paths >> ori >> dect;
    Graph g(num_nodes);
    for(int i=0; i<num_nodes; i++) {
        cin >> temp;
        g.add_numeme(i,temp);
    }
    int u,v,w;
    for(int i=0; i<num_paths; i++){
        cin >> u >> v >> w;
        g.add_path(u,v,w);
    }
    dijkstra(g,ori,dect);
    return 0;
}
void dijkstra(Graph g, int u, int v) {
    int num_nodes = g.get_numnode();
    PathInfo* pathInfo = new PathInfo[num_nodes];

    // 创建并初始化距离数组和访问状态数组
    int* distance = new int[num_nodes];
    bool* visited = new bool[num_nodes];

    for (int i = 0; i < num_nodes; i++) {
        distance[i] = INT_MAX;
        visited[i] = false;
        pathInfo[i].distance = INT_MAX;
        pathInfo[i].pointSum = 0;
        pathInfo[i].pathCount = 0;
    }

    // 设置起始顶点的距离为0
    distance[u] = 0;
    pathInfo[u].distance = 0;
    pathInfo[u].pointSum = g.get_numeme(u);
    pathInfo[u].pathCount = 1;

    for (int count = 0; count < num_nodes - 1; count++) {
        // 在未访问顶点中找到距离最小的顶点
        int min_distance = INT_MAX;
        int min_index;
        for (int i = 0; i < num_nodes; i++) {
            if (!visited[i] && distance[i] <= min_distance) {
                min_distance = distance[i];
                min_index = i;
            }
        }

        // 标记该顶点为已访问
        visited[min_index] = true;

        // 更新与当前顶点相邻顶点的最短距离
        list<Edge> edges = g.get_edges(min_index);
        for (auto edge : edges) {
            int v = edge.dest;
            int weight = edge.weight;
            if (!visited[v] && distance[min_index] + weight < distance[v]) {
                distance[v] = distance[min_index] + weight;
                pathInfo[v].distance = distance[v];
                pathInfo[v].pointSum = pathInfo[min_index].pointSum + g.get_numeme(v);
                pathInfo[v].pathCount = pathInfo[min_index].pathCount;
            } else if (distance[min_index] + weight == distance[v]) {
                if (pathInfo[min_index].pointSum + g.get_numeme(v) > pathInfo[v].pointSum) {
                    pathInfo[v].pointSum = pathInfo[min_index].pointSum + g.get_numeme(v);
                }
                pathInfo[v].pathCount += pathInfo[min_index].pathCount;
            }
        }
    }

    // 输出最短距离、最短路径上点权和以及最短路径的数量
    //cout << pathInfo[v].distance << " " << pathInfo[v].pointSum << " " << pathInfo[v].pathCount << endl;
    cout << pathInfo[v].pathCount <<" "<< pathInfo[v].pointSum;
    // 释放动态分配的内存
    delete[] distance;
    delete[] visited;
    delete[] pathInfo;
}
```

## 笔记

这道题可以使用`dijkstra算法`进行求解。（`dijkstra算法`详见博客`算法`分类）
#【codeforces 764C】Timofey and a tree 题解

## 题目
题目链接：http://codeforces.com/problemset/problem/764/C

> <div class="problem-statement"><div class="header"><div class="title">C. Timofey and a tree</div><div class="time-limit"><div class="property-title">time limit per test</div>2 seconds</div><div class="memory-limit"><div class="property-title">memory limit per test</div>256 megabytes</div><div class="input-file"><div class="property-title">input</div>standard input</div><div class="output-file"><div class="property-title">output</div>standard output</div></div><div><p>Each New Year Timofey and his friends cut down a tree of <span class="tex-span"><i>n</i></span> vertices and bring it home. After that they paint all the <span class="tex-span"><i>n</i></span> its vertices, so that the <span class="tex-span"><i>i</i></span>-th vertex gets color <span class="tex-span"><i>c</i><sub class="lower-index"><i>i</i></sub></span>.</p><p>Now it's time for Timofey birthday, and his mother asked him to remove the tree. Timofey removes the tree in the following way: he takes some vertex in hands, while all the other vertices move down so that the tree becomes rooted at the chosen vertex. After that Timofey brings the tree to a trash can.</p><p>Timofey doesn't like it when many colors are mixing together. A subtree annoys him if there are vertices of different color in it. Timofey wants to find a vertex which he should take in hands so that there are no subtrees that annoy him. He doesn't consider the whole tree as a subtree since he can't see the color of the root vertex.</p><p>A subtree of some vertex is a subgraph containing that vertex and all its descendants.</p><p>Your task is to determine if there is a vertex, taking which in hands Timofey wouldn't be annoyed.</p></div><div class="input-specification"><div class="section-title">Input</div><p>The first line contains single integer <span class="tex-span"><i>n</i></span> (<span class="tex-span">2 ≤ <i>n</i> ≤ 10<sup class="upper-index">5</sup></span>)&nbsp;— the number of vertices in the tree.</p><p>Each of the next <span class="tex-span"><i>n</i> - 1</span> lines contains two integers <span class="tex-span"><i>u</i></span> and <span class="tex-span"><i>v</i></span> (<span class="tex-span">1 ≤ <i>u</i>, <i>v</i> ≤ <i>n</i></span>, <span class="tex-span"><i>u</i> ≠ <i>v</i></span>), denoting there is an edge between vertices <span class="tex-span"><i>u</i></span> and <span class="tex-span"><i>v</i></span>. It is guaranteed that the given graph is a tree.</p><p>The next line contains <span class="tex-span"><i>n</i></span> integers <span class="tex-span"><i>c</i><sub class="lower-index">1</sub>, <i>c</i><sub class="lower-index">2</sub>, ..., <i>c</i><sub class="lower-index"><i>n</i></sub></span> (<span class="tex-span">1 ≤ <i>c</i><sub class="lower-index"><i>i</i></sub> ≤ 10<sup class="upper-index">5</sup></span>), denoting the colors of the vertices.</p></div><div class="output-specification"><div class="section-title">Output</div><p>Print "<span class="tex-font-style-tt">NO</span>" in a single line, if Timofey can't take the tree in such a way that it doesn't annoy him.</p><p>Otherwise print "<span class="tex-font-style-tt">YES</span>" in the first line. In the second line print the index of the vertex which Timofey should take in hands. If there are multiple answers, print any of them.</p></div><div class="sample-tests"><div class="section-title">Examples</div><div class="sample-test"><div class="input"><div class="title">Input</div><pre>4<br>1 2<br>2 3<br>3 4<br>1 2 1 1<br></pre></div><div class="output"><div class="title">Output</div><pre>YES<br>2</pre></div><div class="input"><div class="title">Input</div><pre>3<br>1 2<br>2 3<br>1 2 3<br></pre></div><div class="output"><div class="title">Output</div><pre>YES<br>2</pre></div><div class="input"><div class="title">Input</div><pre>4<br>1 2<br>2 3<br>3 4<br>1 2 1 2<br></pre></div><div class="output"><div class="title">Output</div><pre>NO</pre></div></div></div></div>

## 题目大意
给一颗带颜色的树，看能不能找一个顶点，使其每一个子树的顶点的颜色都相同。

## 题解

bfs搜索。

顺序尝试顶点，看看这个顶点是否符合题目要求。

第一次写的时候没剪枝，结果毫无疑问TLE了。加了个记忆化搜索，记录下已经搜索过的点，就过了。详细看代码。

## 代码

```
//2017-02-02-22.30
//C

#include <bits/stdc++.h>
using namespace std;

vector<int> d[100000 + 100];
int c[100000 + 100];
int rt, n;
map<pair<int, int>, bool> m;
bool bfs(int i, int fa)
{
    if (m.find({i, fa}) != m.end()) return m[{i, fa}];
    vector<int>::iterator it;
    //printf(" %d %d\n", i, fa);
    for (it = d[i].begin(); it != d[i].end(); it++)
    {
        if (*it == fa) continue;
        if ((fa == -1 || c[i] == c[*it]) && bfs(*it, i)) {continue;}
        else {m.insert({{i, fa}, 0}); return 0;}
    }
    m.insert({{i, fa}, 1});
    return 1;
}

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n - 1; i++)
    {
        int u, v;
        scanf("%d%d", &u, &v);
        d[u].push_back(v);
        d[v].push_back(u);
    }
    for (int i = 1; i <= n; i++)
    {
        scanf("%d", &c[i]);
    }
    for (int i = 1; i <= n; i++)
    {
        if (bfs(i, -1))
        {
            printf("YES\n%d\n", i);
            return 0;
        }
    }
    puts("NO");
}
```


# 【codeforces 776C】Molly's Chemicals 题解

## 题目
题目链接：http://codeforces.com/problemset/problem/776/C
><div class="problem-statement"><div class="header"><div class="title">C. Molly's Chemicals</div><div class="time-limit"><div class="property-title">time limit per test</div>2.5 seconds</div><div class="memory-limit"><div class="property-title">memory limit per test</div>512 megabytes</div><div class="input-file"><div class="property-title">input</div>standard input</div><div class="output-file"><div class="property-title">output</div>standard output</div></div><div><p>Molly Hooper has <span class="tex-span"><i>n</i></span> different kinds of chemicals arranged in a line. Each of the chemicals has an affection value, The <span class="tex-span"><i>i</i></span>-th of them has affection value <span class="tex-span"><i>a</i><sub class="lower-index"><i>i</i></sub></span>.</p><p>Molly wants Sherlock to fall in love with her. She intends to do this by mixing a contiguous segment of chemicals together to make a love potion with total affection value as a non-negative <span class="tex-font-style-bf">integer</span> power of <span class="tex-span"><i>k</i></span>. Total affection value of a continuous segment of chemicals is the sum of affection values of each chemical in that segment.</p><p>Help her to do so in finding the total number of such segments.</p></div><div class="input-specification"><div class="section-title">Input</div><p>The first line of input contains two integers, <span class="tex-span"><i>n</i></span> and <span class="tex-span"><i>k</i></span>, the number of chemicals and the number, such that the total affection value is a non-negative power of this number <span class="tex-span"><i>k</i></span>. (<span class="tex-span">1 ≤ <i>n</i> ≤ 10<sup class="upper-index">5</sup></span>, <span class="tex-span">1 ≤ |<i>k</i>| ≤ 10</span>).</p><p>Next line contains <span class="tex-span"><i>n</i></span> integers <span class="tex-span"><i>a</i><sub class="lower-index">1</sub>, <i>a</i><sub class="lower-index">2</sub>, ..., <i>a</i><sub class="lower-index"><i>n</i></sub></span> (<span class="tex-span"> - 10<sup class="upper-index">9</sup> ≤ <i>a</i><sub class="lower-index"><i>i</i></sub> ≤ 10<sup class="upper-index">9</sup></span>)&nbsp;— affection values of chemicals.</p></div><div class="output-specification"><div class="section-title">Output</div><p>Output a single integer&nbsp;— the number of valid segments.</p></div><div class="sample-tests"><div class="section-title">Examples</div><div class="sample-test"><div class="input"><div class="title">Input</div><pre>4 2<br>2 2 2 2<br></pre></div><div class="output"><div class="title">Output</div><pre>8<br></pre></div><div class="input"><div class="title">Input</div><pre>4 -3<br>3 -6 -3 12<br></pre></div><div class="output"><div class="title">Output</div><pre>3<br></pre></div></div></div><div class="note"><div class="section-title">Note</div><p>Do keep in mind that <span class="tex-span"><i>k</i><sup class="upper-index">0</sup> = 1</span>.</p><p>In the first sample, Molly can get following different affection values: </p><ul> <li><span class="tex-span">2</span>: segments <span class="tex-span">[1, 1]</span>, <span class="tex-span">[2, 2]</span>, <span class="tex-span">[3, 3]</span>, <span class="tex-span">[4, 4]</span>;<p> </p></li><li><span class="tex-span">4</span>: segments <span class="tex-span">[1, 2]</span>, <span class="tex-span">[2, 3]</span>, <span class="tex-span">[3, 4]</span>;<p> </p></li><li><span class="tex-span">6</span>: segments <span class="tex-span">[1, 3]</span>, <span class="tex-span">[2, 4]</span>;<p> </p></li><li><span class="tex-span">8</span>: segments <span class="tex-span">[1, 4]</span>. </li></ul><p>Out of these, <span class="tex-span">2</span>, <span class="tex-span">4</span> and <span class="tex-span">8</span> are powers of <span class="tex-span"><i>k</i> = 2</span>. Therefore, the answer is <span class="tex-span">8</span>.</p><p>In the second sample, Molly can choose segments <span class="tex-span">[1, 2]</span>, <span class="tex-span">[3, 3]</span>, <span class="tex-span">[3, 4]</span>.</p></div></div>

## 题目大意
有$n$，$k$ ($1 ≤ n ≤ 105$, $1 ≤ |k| ≤ 10$)，输入一个序列$a_1, a_2, ..., a_n(|a_i| \leq 10^9)$，输出符合$i, j, x (1 \leq i \leq j \leq n, x \geq 0)$使$a_i + ... + a_j = k^x$条件$i, j, x$的个数。

## 题解
看题目，就是求符合条件的区间和问题。穷举明显会爆炸，求区间和的典型做法线段树也不行，查询太多也会爆炸。当时做的时候上网找到一个办法，就是求前缀和。

使$sum[i] = a_1 + ... + a_i$，求$a_i + ... + a_j$便转化为求$sum[j] - sum[i - 1]$的问题，$sum[i]$ 可以很方便地用 $sum[i] = sum[i - 1] + a_n$求得。至此，问题转化为：求符合$i, j(0 \leq i < j \leq n), sum[j] - sum[i] = k^x$条件$i, j, x$个数。

转化完成了，接着怎么做？明显穷举是不行的（如果要不如直接来弄，搞那么多当然有更高的姿势水平啦T_T)。把式子转换一下，得$sum[j] = k^x + sum[i]$，如果有$sum[j]$等于$k^x + sum[i]$，把符合条件的$j$罗列下就好了。

```
#include <bits/stdc++.h>

using namespace std;

const int maxn = 10e5 + 10;
long long sum[maxn];
map<long long, vector<int> > m;
int main()
{
    int n, k, t;
    scanf("%d%d", &n, &k);
    for (int i = 1; i <= n; i++)
    {
        scanf("%d", &t);
        sum[i] = sum[i - 1] + t;
        m[sum[i]].push_back(i);
    }
    long long re = 0;
    long long tt = 1;
    while (1)
    {
        for (int i = 1; i <= n; i++)
        {
            auto ptr = m.find(sum[i - 1] + tt);
            if (ptr == m.end()) continue;
            auto &ttt = ptr->second;
            for (auto it = ttt.begin(); it != ttt.end(); it++)
            {
                if (*it >= i) re++;
            }
        }
        //printf("%d\n", re);
        if (tt > (10e14 / abs(k)) || tt < (-10e14 / abs(k))) break;
        tt *= k;
    }
    printf("%lld\n", re);
    }
}
```
这段代码参考了网上某个前缀和的代码，把数值相同的$sum[i]$丢进了map容器里，并把 $i$ 的值丢进对应的vector容器。之后用$k^x + sum[i]$去找$sum[j]$的值，注意$i < j$，找到符合条件的$j$结果加一就好了。

看似很美好，结果在过这个数据时候就爆炸了。

> 100000 -1  
1 -1 1 -1 1 -1 1 -1 1 -1 ...

分析发现，在这个数据中，map里面就存了0，1，-1几个数值，后面映射的vector带了大量的数据，在找$j$的时候效率非常低下。

能不能避免这个查找呢？反查一下，用$sum[j]$去找$k^x + sum[i]$。因为$i < j$，可以在求$sum[j]$之后在map里找到之前的数据，明显里面数据全部小于$j$，即是里面的数据都符合$i$的条件，也就不用映射vector了直接映射个long long数值表示当前符合条件的个数，答案直接加上这个值就好了。最后再塞进去$k^x + sum[j]$，符合条件的映射值加一就好了。

最后还要注意下$k^x$的求法，注意下$k = 1, k = -1$时要做下处理，以及别溢出就好了$(|k^x| \leq 10^{14})$。

第一次写前缀和的题目，也算是个笔记了，希望都能看懂吧。
## 代码
```
#include <bits/stdc++.h>

using namespace std;

const int maxn = 10e5 + 10;
long long sum[maxn];
map<long long, long long> m;
vector<long long> kn;
int main()
{
    int n, k, t;
    scanf("%d%d", &n, &k);
    long long tt = 1;
    while (1)
    {
        kn.push_back(tt);
        if (k == 1 || (k == -1 && tt == -1) || tt > (10e14 / abs(k)) || tt < (-10e14 / abs(k))) break;
        tt *= k;
    }
    long long re = 0;
    for (auto j : kn) m[j]++;
    for (int i = 1; i <= n; i++)
    {
        scanf("%d", &t);
        sum[i] = sum[i - 1] + t;
        re += m[sum[i]];
        for (auto j : kn) m[sum[i] + j]++;
    }
    printf("%lld\n", re);
}
```
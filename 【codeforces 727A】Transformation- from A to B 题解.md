# 【codeforces 727A】Transformation: from A to B 题解

## 题目
题目链接：http://codeforces.com/contest/727/problem/A

> <div class="problem-statement"><div class="header"><div class="title">A. Transformation: from A to B</div><div class="time-limit"><div class="property-title">time limit per test</div>1 second</div><div class="memory-limit"><div class="property-title">memory limit per test</div>256 megabytes</div><div class="input-file"><div class="property-title">input</div>standard input</div><div class="output-file"><div class="property-title">output</div>standard output</div></div><div><p>Vasily has a number <span class="tex-span"><i>a</i></span>, which he wants to turn into a number <span class="tex-span"><i>b</i></span>. For this purpose, he can do two types of operations:</p><ul> <li> multiply the current number by <span class="tex-span">2</span> (that is, replace the number <span class="tex-span"><i>x</i></span> by <span class="tex-span">2·<i>x</i></span>); </li><li> append the digit <span class="tex-span">1</span> to the right of current number (that is, replace the number <span class="tex-span"><i>x</i></span> by <span class="tex-span">10·<i>x</i> + 1</span>). </li></ul><p>You need to help Vasily to transform the number <span class="tex-span"><i>a</i></span> into the number <span class="tex-span"><i>b</i></span> using only the operations described above, or find that it is impossible.</p><p>Note that in this task you are not required to minimize the number of operations. It suffices to find any way to transform <span class="tex-span"><i>a</i></span> into <span class="tex-span"><i>b</i></span>.</p></div><div class="input-specification"><div class="section-title">Input</div><p>The first line contains two positive integers <span class="tex-span"><i>a</i></span> and <span class="tex-span"><i>b</i></span> (<span class="tex-span">1 ≤ <i>a</i> &lt; <i>b</i> ≤ 10<sup class="upper-index">9</sup></span>)&nbsp;— the number which Vasily has and the number he wants to have.</p></div><div class="output-specification"><div class="section-title">Output</div><p>If there is no way to get <span class="tex-span"><i>b</i></span> from <span class="tex-span"><i>a</i></span>, print "<span class="tex-font-style-tt">NO</span>" (without quotes).</p><p>Otherwise print three lines. On the first line print "<span class="tex-font-style-tt">YES</span>" (without quotes). The second line should contain single integer <span class="tex-span"><i>k</i></span>&nbsp;— the length of the transformation sequence. On the third line print the sequence of transformations <span class="tex-span"><i>x</i><sub class="lower-index">1</sub>, <i>x</i><sub class="lower-index">2</sub>, ..., <i>x</i><sub class="lower-index"><i>k</i></sub></span>, where:</p><ul> <li> <span class="tex-span"><i>x</i><sub class="lower-index">1</sub></span> should be equal to <span class="tex-span"><i>a</i></span>, </li><li> <span class="tex-span"><i>x</i><sub class="lower-index"><i>k</i></sub></span> should be equal to <span class="tex-span"><i>b</i></span>, </li><li> <span class="tex-span"><i>x</i><sub class="lower-index"><i>i</i></sub></span> should be obtained from <span class="tex-span"><i>x</i><sub class="lower-index"><i>i</i> - 1</sub></span> using any of two described operations (<span class="tex-span">1 &lt; <i>i</i> ≤ <i>k</i></span>). </li></ul><p>If there are multiple answers, print any of them.</p></div><div class="sample-tests"><div class="section-title">Examples</div><div class="sample-test"><div class="input"><div class="title">Input</div><pre>2 162<br></pre></div><div class="output"><div class="title">Output</div><pre>YES<br>5<br>2 4 8 81 162 <br></pre></div><div class="input"><div class="title">Input</div><pre>4 42<br></pre></div><div class="output"><div class="title">Output</div><pre>NO<br></pre></div><div class="input"><div class="title">Input</div><pre>100 40021<br></pre></div><div class="output"><div class="title">Output</div><pre>YES<br>5<br>100 200 2001 4002 40021 <br></pre></div></div></div></div>

## 题目大意
对一个数n，有两种操作

 - n × 10 + 1 
 - n × 2

有正整数A、B，问A怎么操作才能变为B。可以操作则输出YES，并输出操作数和操作过程，否则输出NO。

## 题解
可以倒推。

则操作化为：

 - (n - 1) / 10
 - n / 2
 
易得第一个操作只有尾数为1时才能进行，第二个操作只有偶数才能进行，其他情况就无法进行操作，也就是说这时无法实现题目要求。至此就可以得出从B到A的唯一步骤。注意倒推途中可能会得出小于A的操作，这时就可以判定无法得到符合题意要求的操作。

把每步操作记录在数组中，最后倒着输出就可以了。由于数据小于10^9，10^9 < 2^400，400的数组绰绰有余。

## 代码
```
//2016-10-15-17.11
#include <cstdio>
using namespace std;

int re[400];

int main()
{
    int a, b;

    while (scanf("%d%d", &a, &b) != EOF)
    {
        re[0] = b;
        bool flag = 1;
        int i;
        for (i = 0;; i++)
        {
            if (re[i] == a) break;
            if (re[i] < a) {flag = 0; break;}
            if (re[i] % 2 == 0 && re[i] != 0) re[i + 1] = re[i] / 2;
            else if (re[i] % 10 == 1) re[i + 1] = re[i] / 10;
            else {flag = 0; break;}
        }
        if (!flag) {puts("NO"); continue;}
        printf("YES\n%d\n", i + 1);
        for (;; i--)
        {
            printf("%d", re[i]);
            if (i == 0) break;
            else printf(" ");
        }
        printf("\n");
    }
}
``` 
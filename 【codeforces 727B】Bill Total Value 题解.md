题目链接：http://codeforces.com/contest/727/problem/A
>  A. Transformation: from A to B 
>  time limit per test 1 second 
>  memory limit per test 256 megabytes 
> output standard output  
> Vasily has a number a, which he wants to turn into a number b. 
>multiply the current number by 2 (that is, replace the number x by
> 2·x); append the digit 1 to the right of current number (that is,
> replace the number x by 10·x + 1). You need to help Vasily to
> transform the number a into the number b using only the operations
> described above, or find that it is impossible.
> 
> Note that in this task you are not required to minimize the number of
> operations. It suffices to find any way to transform a into b.
> 
> Input The first line contains two positive integers a and b
> (1 ≤ a < b ≤ 10^9) — the number which Vasily has and the number he
> wants to have.
> 
> Output If there is no way to get b from a, print "NO" (without
> quotes).
> 
> Otherwise print three lines. On the first line print "YES" (without
> quotes). The second line should contain single integer k — the length
> of the transformation sequence. On the third line print the sequence
> of transformations x1, x2, ..., xk, where:
> 
> x1 should be equal to a, xk should be equal to b, xi should be
> obtained from xi - 1 using any of two described operations
> (1 < i ≤ k). If there are multiple answers, print any of them.
> 
> Examples 
> **input** 
>2 162
>**output**
>YES
>5
>2 4 8 81 162
>**input** 
>4 42 
>**output** 
>NO
>**input** 
>100 40021 
>**output** 
>YES 
>5
>100 200 2001 4002 40021

###题目大意
对一个数n，有两种操作

 - n × 10 + 1 
 - n × 2

有正整数A、B，问A怎么操作才能变为B。可以操作则输出YES，并输出操作数和操作过程，否则输出NO。
###题解
可以倒推。
则操作化为：

 - (n - 1) / 10
 - n / 2
 
易得第一个操作只有尾数为1时才能进行，第二个操作只有偶数才能进行，其他情况就无法进行操作，也就是说这时无法实现题目要求。至此就可以得出从B到A的唯一步骤。注意倒推途中可能会得出小于A的操作，这时就可以判定无法得到符合题意要求的操作。
把每步操作记录在数组中，最后倒着输出就可以了。由于数据小于10^9，10^9 < 2^400，400的数组绰绰有余。

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
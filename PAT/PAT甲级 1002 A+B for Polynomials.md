# PAT甲级 1002 A+B for Polynomials

[PTA | 程序设计类实验辅助教学平台 (pintia.cn)](https://pintia.cn/problem-sets/994805342720868352/exam/problems/994805526272000000?type=7&page=0)

## 题目

This time, you are supposed to find *A*+*B* where *A* and *B* are two polynomials.

Input Specification:

Each input file contains one test case. Each case occupies 2 lines, and each line contains the information of a polynomial:

$K$ $N_1$ $a_{N1}$ $N_2$ $a_{N2}$ ... $N_Ka_{NK}$

where *K* is the number of nonzero terms in the polynomial, $N_i$ and $a_{Ni}$ (*i*=1,2,⋯,*K*) are the exponents and coefficients, respectively. It is given that 1≤$K$≤10，0≤$N_K$<⋯<$N_2$<$N1$≤1000.

Output Specification:

For each test case you should output the sum of *A* and *B* in one line, with the same format as the input. Notice that there must be NO extra space at the end of each line. Please be accurate to 1 decimal place.

Sample Input:

```in
2 1 2.4 0 3.2
2 2 1.5 1 0.5
```

Sample Output:

```out
3 2 1.5 1 2.9 0 3.2
```

## 代码

```c++
#include <iostream>
#include <iomanip>
using namespace std;
double a[1005];
int main(){
    int x,m,count = 0;
    double y;
    cin>>m;
    for(int i = 0; i < m; i++){
        cin>>x>>y;
        a[x] += y;
    }
    cin>>m;
    for(int i = 0; i < m; i++){
        cin>>x>>y;
        a[x] += y;
    }
    for(int i = 0; i < 1001; i++){
        if(a[i] != 0) count++;
    }
    cout<<count;
    for(int i = 1001; i >= 0; i--){
        if(a[i] != 0){
            cout<<" "<<i<<" "<<setiosflags(ios::fixed)<<setprecision(1)<<a[i];
        }
    }
    return 0;
}
```

## 坑点

1. 需要注意格式控制，浮点数保留一位小数；（setiosflags详解可参考[C++ 标准库之 iomanip 、操作符 ios::fixed 以及 setprecision 使用的惨痛教训经验总结-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1416384)）
2. 当加起来的多项式各项系数均为0时，仅输出0。
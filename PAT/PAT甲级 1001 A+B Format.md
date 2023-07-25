# PAT甲级 1001 A+B Format

[PTA | 程序设计类实验辅助教学平台 (pintia.cn)](https://pintia.cn/problem-sets/994805342720868352/exam/problems/994805528788582400?type=7&page=0)

## 题目

Calculate *a*+*b* and output the sum in standard format -- that is, the digits must be separated into groups of three by commas (unless there are less than four digits).

Input Specification:

Each input file contains one test case. Each case contains a pair of integers *a* and *b* where −106≤*a*,*b*≤106. The numbers are separated by a space.

Output Specification:

For each test case, you should output the sum of *a* and *b* in one line. The sum must be written in the standard format.

Sample Input:

```in
-1000000 9
```

Sample Output:

```out
-999,991
```

## 代码

```c++
#include<iostream>
#include<sstream>
using namespace std;
int main(){
    long a,b;
    stringstream ss;
    cin >> a >> b;
    long temp = a+b;
    string str = to_string(temp);
    if(temp<0){
        ss<<str[0];
        str.erase(str.begin());
    }
    for (int i = 0; i < str.size(); i++) {
        if ((a-b%3)%3==0 && i != str.size() - 1) {
            ss << str[i] << ',';
        } else ss << str[i];
    }
    cout << ss.str();
    return 0;
}
```
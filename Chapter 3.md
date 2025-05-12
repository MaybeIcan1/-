# 第三章 DFS
## 2025.5.13 《LEETCODE T2094 找出3位偶数》   
这题主要是把数组中的元素组成三位数，然后判断是不是偶数，一开始用的是全排列这个数组，然后去前三位元素组合，并通过快慢指针去重，这样就可以实现偶数的升序排序，但**但数组元素太多**的时候会跑不动，漏掉部分排序。
这道题可以用暴力解决，但这里只记录DFS回溯查询。   
因为是找三位数，还要是偶数，偶数的末尾一定是偶数，其他只需要注意首位不要是0就可以。看了题解之后自己写的代码如下：
~~~cpp
class Solution {
    vector<int> ans;
    int nums[10]={0}; //记录0-9出现了多少次
public:
    vector<int> findEvenNumbers(vector<int>& digits) {
        ans.clear();  //全局变量要先清空
        for(int i=0;i<10;i++){
            nums[i]=0;
        }
        for(int i:digits){
            nums[i]++;
            }

        dfs(0,0);
        return ans;
};
    void dfs(int j,int temp){
        if(j==3){
            ans.push_back(temp);
        return;}
        for(int i=0;i<10;i++){
            if(nums[i]>0&&((i%2==0&&j==2)||j==1||(j==0&&i!=0))) { //个位必须是偶数、十位随便、百位不为0 **这个条件判断值得反复琢磨**
                nums[i]--;
                dfs(j+1,temp*10+i);
                nums[i]++;
            }
    }
};
};
~~~
这里就有两个问题，一个是ans和nums数组都要记录到全局变量里，每次调用类函数还要先处理，题解给到了**lambda表达式**这个很牛逼的解法，Lambda 表达式是 C++ 中一种**匿名函数**的写法，可以理解为一种“临时定义的小函数”。   
题解完整代码如下：
~~~
class Solution {
public:
    vector<int> findEvenNumbers(vector<int>& digits) {
        int cnt[10]{};
        for (int d : digits) {
            cnt[d]++;
        }

        vector<int> ans;
        // i=0 百位，i=1 十位，i=2 个位，x 表示当前正在构造的数字
        auto dfs = [&](this auto&& dfs, int i, int x) -> void {
            if (i == 3) {
                ans.push_back(x);
                return;
            }
            for (int d = 0; d < 10; d++) {
                if (cnt[d] > 0 && (i == 0 && d > 0 || i == 1 || i == 2 && d % 2 == 0)) {
                    cnt[d]--; // 消耗一个数字 d
                    dfs(i + 1, x * 10 + d);
                    cnt[d]++; // 复原
                }
            }
        };
        dfs(0, 0);
        return ans;
    }
};
作者：Thirty-seven37
~~~
lambda的格式为:
~~~cpp
cpp auto func = []() { /* 函数体 */ };
~~~
其中，[]为**捕获列表，表示如何获取外部变量**，()是参数列表，{}为函数体，本题的代码解释就是：   
**[&]	捕获列表**：按引用捕获所有外部变量（如 cnt 数组和 ans 结果集）  
**this auto&& dfs**为显式对象参数：C++23 新特性，允许 lambda 递归调用自己   
**int i, int x**是参数列表：当前处理到第几位（i=0/1/2），当前生成的数字（x）   
**-> void**为返回值类型（这里无返回值）   
显示对象参数是递归自身用的，也就是说以后写到**需要利用全局变量但不需要递归**的时候，就用lambda表达式但不写显示对象参数即可。   
还有一个for循环值得注意：
~~~cpp
for(int i:digits){
            nums[i]++;
            }
~~~
这是C++ 11的语法**for(int a:b)**，即从数组b依次取出元素赋值给整型变量a，在一些需要遍历数组但又要记录此时遍历的值出现了几次的时候这样写又简洁又方便！

# DP动态规划
## 2025.5.13 《LEETCODE T3335 字符串转换》
这题一开始用了暴力，结果824个测试只能过502个。观察题目，![image](https://github.com/user-attachments/assets/50a26fe4-0d30-4fb8-b4da-d579226bf5a3)    其实只需要求出最后字符串的长度，每个字符的变化其实不那么重要，反而是每一次字符的数量更重要。这么品其实已经有
点dp的意思了：每一次翻转后的字符**状态**才是我们关注的。这启示我们，将同一种字符放在一起，同时进行处理。也即，3个b->3个c，不需要分开讨论
总结就是**第t次翻转，字符i的数量为字符i-1的数量转移过来（父问题：当前的字符数；子问题：上一次翻转的字符数）**，这很DP。   
由题意，字符c-z都是前一字符后移一位得来，数量上不变，a是z变来的，只有b最特殊，**由a和z两个字符转移叠加**（这也是我怕看了题解才大悟的地方，一开始受限于z要变成ab两个，把a和b看做一类了，其实dp看的是每个变量的状态，思考过程应该是：a-z每个字符单独考虑，发现c-z是一个模版、a是z变来，b最特殊）
   可以得到转移方程：   
   <img width="321" alt="1747143812002" src="https://github.com/user-attachments/assets/81763ecd-6fda-47dd-9ffc-1373c041aace" />   
具体代码如下：
   ~~~cpp
int lengthAfterTransformations(string s, int t) {
        vector<long long> cnt(26, 0);
        long long N=1e9+7;
        for(char i : s){
            cnt[i-'a']++;
        }
        vector<long long> new_cnt(26, 0);
        while(t>0){
            t--;
            
            for(int i=1;i<=26;i++){
                if(i>1&&i<=25){    //一开始写错成if(i>1 && i< 25)
                    new_cnt[i]=cnt[i-1];
                }
                else if(i==26){    //一开始写错成if(i == 25)
                    new_cnt[0]=cnt[25];
                    new_cnt[1]=(cnt[0]+cnt[25])%N;
                }
            }
        
            
            for(int i=0;i<26;i++){
                cnt[i]=new_cnt[i];
            }
            }
        long long temp=0;
       for(int i=0;i<26;i++){
                temp+=cnt[i];
            }
        return temp%N;
~~~
一开始一直运行不对test，后面发现是new_cnt数组是从小到大更新的，那么new_cnt[25]（i=25）应该是if结构中而不是else
if中。因为测试案例可能很大，而在状态变换中只有cnt[b]可能会增加，因此可以对它的取值提前取模。
## 

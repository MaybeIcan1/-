# 数据结构
## 用双栈实现队列
2025.5.9日 LeetCode-LCR T125，虽然没太读懂题意，但双栈实现队列的思想并不难。**栈是先进后出，队列是先进先出**，也就是把栈**翻转过来**，在逐个弹出栈顶，既可以和队列是一个效果。所以双栈实现队列，就是创建一个新栈B，A逐渐出栈，压入B中，具体代码如下：
```cpp
{
  stack A,B;        //存放整形
  if(!B.empty()){  //若B不为空，栈顶肯定是先进A的元素
    int x=B.top();     //因为cpp弹出栈顶不会保存那个元素，所以要先自己保存
    B.pop();
    return x;
  }
if(A.empty())return -1;
while(!A.empty()){   //执行到这说明是最开始还没翻转的时候
  int x=A.top();
  A.pop();
  B.push(x);
}   
int x=B.top();
    B.pop();
    return x;
}
```

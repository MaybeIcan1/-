# 差分数组
## &nbsp;&nbsp;&nbsp;&nbsp;定义与性质
对于数组a，定义其差分数组d如下：   
**性质1：** 从左到右**累加**d中的元素，可以得到数组a。   
**性质2：** ①把 a的子数组a[i],a[i+1],…,a[j]都加上x **等价于** ②把d[i]加上x，d[j+1]减x   
于是对于数组比较大的情况下，对子数组进行多次操作时，暴力肯定不行，就可以利用2.②将**对数组的子数组进行值改变**的问题降复杂度。   
> :bulb: **提示：** 在问题的解决中，通常先用2.② 在O(1) 的时间下可以完成对a的子数组的操作。最后利用性质 1 从差分数组复原出数组 a。   
> :memo: **注意：** 差分数组累加才得到原数组元素，即a[i]=d[0]+d[1]+...+d[i]，也表述成d[i]表示把下标≥i的数都加上d[i]
---   
2025-5-21 《LEETCODE T3355 》
~~~cpp
class Solution {
public:
    bool isZeroArray(vector<int>& nums, vector<vector<int>>& queries) {
        int n=nums.size();
        vector<int>diff(n+1,0);
        for(auto q:queries){
            diff[q[0]]++;
            diff[q[1]+1]--;
        }
        int temp=0;
        for(int i=0;i<n;i++){
            temp+=diff[i];
            if(nums[i]>temp)return false;
        }
        return 1;
    }
};
~~~
因为是把nums数组根据传入的二维数组queries的范围进行对应元素的-1，设计了多子数组的多次操作，所以想到差分数组。既然是看nums能不能一直-1直到0，可以逆向思维，创造一个全是0的数组作为差分数组，
对queries对应元素的范围进行+1，然后去比较加完之后与nums数组值的大小，如果nums比diff大，那就说明减不完。而diff数组利用差分性质，比如queries[0]=[1,3]，那就只需要对diff[1]++,diff[3+1]--即等价于
diff[1]++,diff[2]++,diff[3]++。

# 算法

## 动态规划

动态规划就是去掉重复计算

![动态1](https://github.com/sspkuxuan/algorithm_study/raw/master/dongtaiguihua/dongtai1.png)

![动态2](https://github.com/sspkuxuan/algorithm_study/raw/master/dongtaiguihua/dongtai2.png)

![动态3](https://github.com/sspkuxuan/algorithm_study/raw/master/dongtaiguihua/dongtai3.png)

![动态4](https://github.com/sspkuxuan/algorithm_study/raw/master/dongtaiguihua/dongtai4.png)

### 数字三角形

#### 题解



#### code c++

```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
//动态规划 递归 利用一个数组 保存递归产生的重复计算 数字三角形 找下面最大
int Maxsum(vector<vector<int>>a, vector<vector<int>>maxsum, int i, int j){
	int n = a.size() - 1;
	if (maxsum[i][j] != -1)return maxsum[i][j];
	if (i == n)maxsum[i][j] = a[i][j];
	else
	{
		int x = Maxsum(a, maxsum, i + 1, j);
		int y = Maxsum(a, maxsum, i+1, j + 1);
		maxsum[i][j] = max(x, y) + a[i][j];
	}
	return maxsum[i][j];
}
// 递归改为递推
int Maxsum1(vector<vector<int>>a, vector<vector<int>>maxsum, int i, int j){
	int n = a.size() - 1;
	for (int j = 0; i <= n; ++i)maxsum[n][i]=a[n][i];
	for (int i = n-1; i >=0; --i){
		for (int t = 0; t <= i; ++t){
			maxsum[i][t] = max(maxsum[i + 1][t], maxsum[i + 1][t + 1]) + a[i][t];
		}
	}
	return maxsum[0][0];
}
// 空间优化 用一个一维数组
int Maxsum2(vector<vector<int>>a, vector<int>  maxsum, int i, int j){
	
	int n = a.size() - 1;
	maxsum = a[n];
	/*for (int j = 0; i <= n; ++i)maxsum[n][i] = a[n][i];*/
	for (int i = n - 1; i >= 0; --i){
		for (int t = 0; t <= i; ++t){
			maxsum[t] = max(maxsum[t], maxsum[t + 1]) + a[i][t];
		}
	}
	return maxsum[0];
}
void main(){
	vector<vector<int>>a = { { 7 }, { 3, 8 }, { 8, 1, 0 }, { 2, 7, 4, 4 }, { 4, 5, 2, 6, 5 } };
	int n = a.size();
	vector<vector<int>>b;
	for (int i = 1; i <=n; ++i){
		vector<int>c(i, -1);
		b.push_back(c);
	}
	vector<vector<int>>t=b;
	cout<<Maxsum(a,b, 0, 0)<<endl;
	cout << Maxsum1(a, t, 0, 0) << endl;
	vector<int>  maxsum;
	cout << Maxsum2(a, maxsum, 0, 0) << endl;
	system("pause");
}
```

### 神奇的口袋

#### 题解

#### code c++

```c++
#include<iostream>
#include<vector>
using namespace std;
//递归
int Ways(int s, vector<int>a,int k){//从前k个商品里 找一些商品凑数
	if (s == 0)return 1;
	if (k <= 0)return 0;
	if (s - a[k - 1]>=0)return Ways(s, a, k - 1) + Ways(s - a[k-1], a, k - 1);	
	else return Ways(s, a, k - 1);
}
//动态规划
int Ways1(vector<vector<int>>c, vector<int>a, int k){//从前k个商品里 找一些商品凑数
	vector<int> b(k+1, 1);
	//c[w][k] 代表从前k个物品 凑出w体积的方法数目
	c[0] = b;
	for (int w = 1; w <=40; ++w){
		for (int i = 1; i <= k; ++i){
			if (w - a[i-1] >= 0)c[w][i] = c[w - a[i-1]][i - 1] + c[w][i - 1];
			else c[w][i] = c[w][i - 1];
		}
	}
	return c[40][k];
}
int main(){
	vector<int>a = { 10, 20, 30, 40, 20, 5, 5, 5, 5, 15, 25, 35 };
	int k = a.size();
	//cout << Ways(40, a,k) << endl;
	vector<int>d(k+1);
	vector<vector<int>>c(41,d);
	cout << Ways1(c, a, k) << endl;
	system("pause");
}
```



### 背包问题

#### 题解

#### code c++

```c++
#include<iostream>
using namespace std;
#include<algorithm>
int dp[5][9] = { { 0 } };//动态规划记录值
int item[5];//记录是不是最优解
int w[5] = { 0, 2, 3, 4, 5 };			//商品的体积2、3、4、5
int v[5] = { 0, 3, 4, 5, 6 };			//商品的价值3、4、5、6
int dpless[9] = { 0 };
int itemless[5];//记录是不是最优解

int bagv = 8;					        //背包大小
//递推动态规划 空间复杂度大 需要n*m记录背包总价
int beibao(){
	for (int i = 1; i <= 4; ++i){
		for (int j = 1; j <= bagv; j++){
			if (j < w[i])dp[i][j] = dp[i - 1][j];
			else dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - w[i]]+v[i]);
		}
	}
	return dp[4][8];
}
//背包问题最优解回溯，输出最优解的方案
void findwhat(int i, int j){
	if (i >= 0){
		if (dp[i][j] == dp[i - 1][j]){
			item[i] = 0;
			findwhat(i - 1, j);
		}
		else if (j >=w[i] && dp[i][j] == dp[i - 1][j - w[i]] + v[i]){
			item[i] = 1;
			findwhat(i - 1, j - w[i]);
		}
	}
}
//优化空间 一维数组 从第一行 最后一列往前走 回溯不了 前面的覆盖了
int beibaoless(){
	for (int i = 1; i <= 4; ++i){
		for (int j = bagv; j >=1; --j){
			if (j < w[i])dpless[j] = dpless[j];
			else dpless[j] = max(dpless[j - w[i]] + v[i], dpless[j]);
		}
	}
	return dpless[8];
}

void main(){
	cout<<beibao()<<endl;
	findwhat(4,8);
	for (int i = 0; i <= 4; ++i){
		if (item[i])cout << w[i] << " " << v[i]<<endl;
	}
	cout << beibaoless() << endl;
	system("pause");

}
```

### 最长子序列

时间复杂度 n*n

#### 题解

#### code c++

```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
//const int MAXN = 1000;
vector<int> a = { 1, 2, 5, 6, 8, 4, 2, 1, 3, 6, 9, 8, 44, 556, 1156, 4458, 1, 25, 5 };
void main(){
	int len = a.size();
	vector<int> b(len, 1);//存 max ak
	for (int i = 1; i < len; ++i){
		//从 0 到i 依次比较 取最大的序列长度 存起来
		for (int j = 0; j < i; ++j){
			if (a[i]>a[j])b[i] = max(b[j] + 1, b[i]);
		}
	}
	//b数组里存了以ak为终点的最长子序列长度
	cout<<*max_element(b.begin(), b.end());
	//max_element 求区间中的最大值 返回最大元素迭代器
	system("pause");

}
```

### 两字符串最长公共序列

#### 题解

#### code c++

```c++
#include<iostream>
#include<String>
#include<vector>
#include<algorithm>
using namespace std;
void main(){
	string s1, s2;
	s1 = "abcdfgtd";
	s2 = "abcfdduti";
	int len1 = s1.size();
	int len2 = s2.size();
	vector<int>a(len1+1,0);
	vector<vector<int>>b(len2+1, a);
	cout << s1[len1 - 1];
	//已经将b[i][0]和b[0][j] 赋值为0
	for (int i = 1; i <=len2; ++i){
		// b[i - 1][j - 1] 不能越界 所以 i j从1开始
		// s2[i-1] == s1[j-1] 从0开始 -1
		for (int j = 1; j <len1; ++j){
			if (s2[i-1] == s1[j-1])b[i][j] = b[i - 1][j - 1] + 1;
			else b[i][j] = max(b[i - 1][j], b[i][j - 1]);
		}
	}
	cout << b[len2-1][len1-1];
	system("pause");
}
```

### 最佳加法表达式

#### 题解

#### code c++

```c++
#include<iostream>
#include<String>
#include<vector>
#include<limits>//最大值
#include<algorithm>
using namespace std;
void main(){
	int INF = INT_MAX;
	vector<int> a = { 1, 22, 3};
	int m = 1;//加号的个数是3
	int len = a.size();
	vector<int>d(len+1);
	vector<vector<int>>b(len + 1, d);
	for (int i = 1; i <= len; ++i){
		//i 位和j位组成的数字
		b[i][i] = a[i-1];
		for (int j = i + 1; j <= len; ++j){
			int t = 1, c = a[j - 1];
			while (c / 10 >= 1){ ++t; c = c / 10; }//判断后一位的位数
				b[i][j] = b[i][j - 1] * pow(10,t) + a[j - 1];
		}
	}
	vector<int> res1(len+1,0);
	vector<vector<int>>res(m+1, res1);
	for (int i = 1; i <= len; ++i)
		res[0][i] = b[1][i];//没有加号的情况 res[i][j]代表i个加号，j个数字组成的最小数
	for (int i = 1; i <= m; ++i){
		for (int j = 0; j <= len; ++j){
			if (j < i + 1)res[i][j] = INF;//数要比加号多1
			else{
				int tmp = INF;
				for (int k = i; k < j; ++k){
					//res[i][j]= 在前k个数里面 插入i-1个加号的最小值加上 第k+1到j组成的数字大小 最小值
					tmp = min(tmp, res[i - 1][k] + b[k + 1][j]);
				}
				res[i][j] = tmp;
			}

		}
	}
	cout <<"zuixiao"<< res[m][len];
	system("pause");
	
}
```


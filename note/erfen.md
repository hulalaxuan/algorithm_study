# 算法

## 二分算法

### 二分查找算法&LowerBound

#### code c++

```c++
#include<iostream>
#include<vector>
using namespace std;

int BinarySearch(vector<int> &a, int t){
	int R = a.size()-1;
	int L = 0;
	while (L <= R){
		int M = L + (R - L) / 2;
		if (a[M] == t)return M;
		else if (a[M] > t)R = M - 1;
		else L = M + 1;
	}
	return -1;
}
int LowerBound(vector<int> &a, int t){//比t小的值的最大下标
	int R = a.size() - 1;
	int L = 0;
	int P = -1;
	while (L <= R){
		int M = L + (R - L) / 2;
		if (a[M] >=t)R = M - 1;
		else{ P = M; L = M + 1; }
	}
	return P;
}
void main(){
	int N;
	cin >> N;
	cout << endl;
	int t;
	cin >> t;
	cout << endl;
	vector<int>a(N);
	for (int i = 0; i < N; ++i){
		cin >> a[i];
	}
	//cout<<BinarySearch(a, t);
	cout << LowerBound(a, t);
	system("pause");
}
```



### 求方程的根

#### code c++

```c++
#include<iostream>
#include<cmath>
using namespace std;
double c = 1e-6;
double f(double t){ return (t*t*t - 5*t*t + 10 * t-80); }
void main(){
	double l = 0, r = 100;
	double m = l + (r - l) / 2;
	int t = 1;
	double y = f(m);
	while (y > c||y<-c){
		if (y > 0)r = m;
		else l = m;
		m = l + (r - l) / 2;
		++t;
		y = f(m);
	}
	cout << t << "ci" << m;
	system("pause");
}
```

### 找一对数

#### 题解

![题解1](https://github.com/sspkuxuan/algorithm_study/raw/master/image2/zhaoyiduishu1.png)

![题解2](https://github.com/sspkuxuan/algorithm_study/raw/master/image2/zhaoyiduishu2.png)

#### code c++

```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
vector<int> yiduishu2(vector<int>&a, int n){
	vector<int>res(2, 0);
	sort(a.begin(), a.end());
	int len = a.size();
	int l = 0;
	int r = len - 1;
	while (l < r){
		if (a[l] + a[r] == n){ res[0] = a[l]; res[1] = a[r]; return res; }
		else if (a[l] + a[r] < n)++l;
		else --r;
	}
	return res;
}
vector<int> yiduishu1(vector<int>&a, int n){
	vector<int>res(2,0);
	sort(a.begin(),a.end());
	int len = a.size();
	for (int i = 0; i < len; ++i){
		int t = n - a[i];
		int l = 0;
		int r = len - 1;
		while (l <= r){
			int m = l + (r - l) / 2;
			if (a[m] == t){ res[0]=a[i]; res[1]=t; return res; }
			else if (a[m] < t)l = m + 1;
			else r = m - 1;
		}
	}
	return res;
}
void main(){
	vector<int> a = { 5, 6, 9, 8, 98,3,50, 100, 477, 5 };
	int N;
	cin >> N;
	vector<int>res;
	res=yiduishu2(a, N);
	cout << res[0] << " " << res[1];
	system("pause");

}
```

### 农夫和奶牛

#### 题解

![题解1](https://github.com/sspkuxuan/algorithm_study/raw/master/image2/nainiu1.png)

![题解2](https://github.com/sspkuxuan/algorithm_study/raw/master/image2/nainiu2.png)

![题解3](https://github.com/sspkuxuan/algorithm_study/raw/master/image2/nainiu3.png)

#### code c++

```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
int nainiu(vector<int>&a, int c){
	sort(a.begin(), a.end());
	int len = a.size();
	int l = 1;
	int r = 1000000000 / c;
	int m = l + (r - l) / 2;
	int max = 0;
	while (l <= r){
		int t = c;
		for (int i = 0; i < len;){
			--t;
			if (t == 0){
				max = m;
				l = m + 1;
				m = l + (r - l) / 2;
				break;
			}
			int p = a[i]+m;
			int j;
			for (j = i + 1; j < len; ++j){
				if (a[j]>=p){
					i = j;
					break;
				}
			}
			if (j == len){
				r = m - 1;
				m = l + (r - l) / 2;
				break;
			}
		}
		cout << l << m << r;
	}
	return max;
}
void main(){
	vector<int> a = { 1, 200, 500, 800, 440, 1586, 9989, 1985 };
	int n;
	cin >> n;
	cout << " "<<nainiu(a, n);
	system("pause");
}
```


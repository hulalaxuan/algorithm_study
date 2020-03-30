# 算法

## 分治算法

分治算法很多是用递归进行实现的

### 归并排序

归并[排序](https://baike.baidu.com/item/排序)是稳定的排序.即相等的元素的顺序不会改变，时间复杂度nlogn,最坏时间复杂度也是nlogn，平均时间仅次于快速排序,采用分治思想。

#### 题解



#### code c++

```c++
#include<iostream>
#include<vector>
using namespace std;
void Merge(vector<int>&a, int l, int m, int r, vector<int>&b){
	//归并的结果先放在b里面，然后放在a里面
	int p = 0;
	int p1 = l, p2 = m + 1;
	while (p1 <= m&&p2 <= r){//归并
		if (a[p1] < a[p2])b[p++] = a[p1++];
		else b[p++] = a[p2++];
	}
	while (p1 <= m){ b[p++] = a[p1++]; }
	while (p2 <= r){ b[p++] = a[p2++]; }
	int c = 0;
	for (int i = l; i <= r; ++i)a[i] = b[c++];//拷贝到a里面
}
void MergeSort(vector<int>&a, int l, int r, vector<int>&b){
	if (l < r){
		int m = l + (r - l) / 2;
		MergeSort(a, l, m, b);//分治 一半一半来排序
		MergeSort(a, m+1, r, b);// 递归的终止条件  只剩下一个元素 也就是l=r
		Merge(a, l, m, r, b);//将排序好的 归并在一起
	}
}
void main(){
	vector<int>a = { 1, 58, 98, 254, 0, 4, 56, 1, 2, 35, 898, 45, 1568, 335, 5 };
	int r = a.size()-1;
	vector<int>b(a.size());
	for (int i : a)cout << i << " ";
	MergeSort(a, 0, r, b);
	for (int i : a)cout << i << " ";
	system("pause");
}
```

### 快速排序

#### 题解



#### code c++

```c++
#include<iostream>
#include<vector>
using namespace std;
void QuickSort(vector<int>&a, int l, int r){
	if (l>=r)return;//递归终止条件
	int t = a[l];
	int s = l, e = r;
	while (l<r){
		while (a[r] >= t)--r;
		swap(a[l], a[r]);
		while (r>l&&a[l] < t)++l;//因为r的值减少了 需要判断是否l<r
		swap(a[l], a[r]);
	}
	QuickSort(a, s, l - 1);
	QuickSort(a, l + 1, e);
}
void main(){
	vector<int>a = { 1, 58, 98, 254, 0, 4, 56, 1, 2, 35, 898, 45, 1568, 335, 5 };
	int r = a.size() - 1;
	for (int i : a)cout << i << " ";
	QuickSort(a, 0, r);
	for (int i : a)cout << i << " ";
	system("pause");
}
```



### 前m大数

#### 题解



#### code c++

```c++
#include<iostream>
#include<vector>
#include<queue>
#include<functional>//greater&less的头文件
using namespace std;

void BigM(vector<int>&a, int l, int r, int m) {
	//快排分治思想,时间复杂度 n+mlogm
	int s = l, e = r;
	int p = a[l];
	while (l < r){
		while (a[r] >= p)--r;
		swap(a[l], a[r]);
		while (l < r&&a[l] < p)++l;
		swap(a[l], a[r]);
	}
	int c = e - l + 1;// 排在p右面的个数 
	if (c == m)return;//m个 返回
	else if (c<m)BigM(a, s, l - 1, m - c);//小于m ，在左面继续找剩下的
	else BigM(a, l + 1, e, m);//大于 在右面找m个
}
void BigM1(vector<int>&a, int l, int r, int m){
	//优先级队列 堆排序
	priority_queue <int, vector<int>, greater<int>>res;
	for (int i = l; i <= r; ++i){
		if (res.size() >= m){
			int min = res.top();
			if (a[i]>min){ 
				res.pop();
				res.push(a[i]);
			}
		}
		else res.push(a[i]);
	}
	while (!res.empty()){
		cout << res.top() << " ";
		res.pop();
	}

}
void main(){
	vector<int>a = { 1, 58, 98, 254, 0, 4, 56, 1, 2, 35, 898, 45, 1568, 335, 5 };
	int r = a.size()-1;
	int m = 5;
	for (int i : a)cout << i << " ";
	cout << endl;
	BigM(a, 0, r, m);
	for (int i : a)cout << i << " ";
	cout << endl;
	for (int i = 0; i < m; ++i)cout << a[r - i] << " ";
	cout << endl;
	BigM1(a, 0, r, m);
	system("pause");
}
```

### 逆序数

#### 题解

#### code c++

```c++
#include<iostream>
#include<vector>
using namespace std;
int n = 0;

void Merge(vector<int>&a, int l, int m, int r, vector<int>&b){
	//归并的结果先放在b里面，然后放在a里面
	int p = 0;
	int p1 = l, p2 = m + 1;
	while (p1 <= m&&p2 <= r){//归并
		if (a[p1] > a[p2]){
			b[p++] = a[p1++]; 		
			n += r - p2 + 1;
		}
		else b[p++] = a[p2++]; 
	}
	while (p1 <= m){ b[p++] = a[p1++]; }
	while (p2 <= r){ b[p++] = a[p2++]; }
	int c = 0;
	for (int i = l; i <= r; ++i)a[i] = b[c++];//拷贝到a里面
}
void MergeSort(vector<int>&a, int l, int r, vector<int>&b){
	if (l >= r)return;//递归终止条件
	int m = l + (r - l) / 2;
	MergeSort(a, l, m, b);//分治 一半一半来排序
	MergeSort(a, m+1, r, b);// 递归的终止条件  只剩下一个元素 也就是l=r
	Merge(a, l, m, r, b);//将排序好的 归并在一起
}
void main(){
	vector<int>a = { 1, 7, 2, 9, 6, 4, 5, 3 };
	int r = a.size()-1;
	vector<int>b(a.size());
	for (int i : a)cout << i << " ";
	cout << endl;
	MergeSort(a, 0, r, b);
	for (int i : a)cout << i << " ";
	cout << endl;
	cout << n % 1000000007;
	system("pause");
}
```


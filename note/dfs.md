# 算法

## 第六章  深度优先搜索算法

### dfs

### dfs表达

### 城堡问题

#### 题解

#### code c++

```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
vector<vector<int>>rooms = {
	{11,6,11,6,3,10,6},
	{7,9,6,13,5,15,5},
	{1,10,12,7,13,7,5},
	{13,11,10,8,10,12,13}
};
vector<vector<int>>colors;
int maxRoomArea = 0, roomNum = 0;
int roomArea;
void dfs(int i, int j){
	if (colors[i][j])return;
	++roomArea;
	colors[i][j] = roomNum;
	//西北东南 rooms 某一位位0  说明 没有墙， 没有墙就继续走
	if ((rooms[i][j] & 1) == 0)dfs(i, j - 1);
	if ((rooms[i][j] & 2) == 0)dfs(i-1, j );
	if ((rooms[i][j] & 4) == 0)dfs(i, j + 1);
	if ((rooms[i][j] & 8) == 0)dfs(i+1, j );
}
void main(){
	int r = rooms.size();
	int c = rooms[0].size();
	vector<int>a(c, 0);
	colors = vector<vector<int>>(r, a);
	for (int i = 0; i < r; ++i){
		for (int j = 0; j < c; ++j){
			if (!colors[i][j]){//找完一个链接房间
				++roomNum; roomArea = 0;
				dfs(i, j);
				maxRoomArea = max(roomArea, maxRoomArea);
			}
		}
	}
	cout << roomNum << endl;
	cout << maxRoomArea << endl;
	system("pause");

}
```



### 踩方格

#### 题解



#### code c++

```c++
#include<iostream>
#include<vector>
using namespace std;
vector<int> a(50, 0);
vector<vector<int>> b(30, a);
int ways(int i, int j, int n){
	if (n == 0)return 1;
	b[i][j] = 1;
	int num = 0;
	if (!b[i][j - 1])num += ways(i, j - 1, n - 1);
	if (!b[i][j +1])num +=ways(i, j + 1, n - 1);
	if (!b[i+1][j ])num +=ways(i+1, j  , n - 1);
	b[i][j] = 0;// 保证所有方法执行 需要设为0
	return num;
}
void main(){
	int n;
	cin >> n;
	cout << ways(0, 25, n) << endl;
	system("pause");
}
```


# 算法

## 第六章  深度优先搜索算法

### dfs

![dfs](https://github.com/sspkuxuan/algorithm_study/raw/master/dfs/dfs1.png)

![dfs](https://github.com/sspkuxuan/algorithm_study/raw/master/dfs/dfs2.png)

![dfs](https://github.com/sspkuxuan/algorithm_study/raw/master/dfs/dfs3.png)

### dfs表达

![dfs表达](https://github.com/sspkuxuan/algorithm_study/raw/master/dfs/biaoshi1.png)

![dfs表达](https://github.com/sspkuxuan/algorithm_study/raw/master/dfs/biaoshi2.png)

### 城堡问题

#### 题解

![城堡](https://github.com/sspkuxuan/algorithm_study/raw/master/dfs/chengbao1.png)

![城堡](https://github.com/sspkuxuan/algorithm_study/raw/master/dfs/chengbao2.png)

![城堡](https://github.com/sspkuxuan/algorithm_study/raw/master/dfs/chengbao3.png)

![城堡](https://github.com/sspkuxuan/algorithm_study/raw/master/dfs/chengbao5.png)

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

![踩方格](https://github.com/sspkuxuan/algorithm_study/raw/master/dfs/caifangge1.png)

![踩方格](https://github.com/sspkuxuan/algorithm_study/raw/master/dfs/caifangge2.png)



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

### Roads

#### 题解

![road](https://github.com/sspkuxuan/algorithm_study/raw/master/dfs/roads1.png)

![road](https://github.com/sspkuxuan/algorithm_study/raw/master/dfs/roads2.png)

#### code c++

```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;
int K, N, R;//k 有k元 N终点城市N 城市间有R条单向道路
struct Road{
	int d, L, t;
	//d 从当前路走到终点城市d需要的路长L和过路费t
};
vector<vector<Road>>G(110);
int minL[110][10010];//minl[i][j]代表从起点1走到了城市i 所花费的钱j 所走的路长
//G[i] 存放从i开始的边 Road 所以只需要记录终点d
int minLen;//最佳终点路径的长度
int totalLen;// 正在看的这条路走了多长
int totalCost;//正在走的这条路花了多少钱
int visited[110];//记录城市有没有走过

//会超时 交到oj上 需要剪枝 这样时间才能通过
void dfs(int s){
	if (s == N){
		minLen = min(totalLen, minLen);
		return;
	}
	for (int i = 0; i < G[s].size(); ++i){
		Road r = G[s][i];
		if (totalCost + r.t>K)continue;//可行性剪枝
		if (!visited[r.d]){
			if (totalLen + r.L >= minLen)continue;//最优性剪纸
			if (totalLen + r.L >= minL[r.d][totalCost + r.t])
				continue;//最优性剪纸2
			minL[r.d][totalCost + r.t] = totalLen + r.L;
			
			totalLen += r.L;
			totalCost += r.t;
			visited[r.d] = 1;
			for (int i = 0; i < 110;++i)
				for (int j = 0; j < 10010l; ++j)
					minL[i][j] = INT_MAX;
			dfs(r.d);
			// dfs(r.d)这条路试玩以后 从s开始的要实验下一条路
			//因为下一条路 可能再走到d上，所以要删除这些标志信息
			visited[r.d] = 0;
			totalLen -= r.L;
			totalCost -= r.t;
			//进入下一个路
		}
		
	}

}
int main(){
	//读数据
	cin >> K >> N >> R;
	for (int i = 0; i < R; ++i){
		int s; Road r;
		cin >> s >> r.d >> r.L >> r.t;
		if (s != r.d)G[s].push_back(r);
	}
	memset(visited, 0, sizeof(visited));
	totalLen = 0;
	minLen = INT_MAX;
	totalCost = 0;
	visited[1] = 1;
	dfs(1);
	if (minLen < INT_MAX)cout << minLen << endl;
	else cout << "-1" << endl;
	system("pause");
	return 0;
}
```


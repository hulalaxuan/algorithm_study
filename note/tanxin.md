# 算法

## 第八章 贪心算法

### 圣诞老人

#### 题解

![shengdan](https://github.com/sspkuxuan/algorithm_study/raw/master/tanxin/shengdan1.png)

![shengdan](https://github.com/sspkuxuan/algorithm_study/raw/master/tanxin/shengdan2.png)

![shengdan](https://github.com/sspkuxuan/algorithm_study/raw/master/tanxin/shengdan3.png)

#### code c++

```c++
#include<iostream>
#include<algorithm>
#include<iomanip>
#include<cstdio>
#include<vector>
using namespace std;

const double eps = 1e-6;
class Candy
{
public:
	int v, w;
	bool operator<(const Candy &c)const{
		return double(v) / w - double(c.v) / c.w>eps;
	}
};
int main(){
	vector<Candy>candies(110);
	int n, w;
	cin >> n >> w;
	for (int i = 0; i < n; ++i){
		cin >> candies[i].v >> candies[i].w;
	}
	sort(candies.begin(), candies.begin()+n);
	int totalW = 0;
	double totalV = 0;
	for (int i = 0; i < n; ++i){
		if (totalW + candies[i].w <= w){
			totalW += candies[i].w;
			totalV += candies[i].v;
		}
		else{
			totalV += candies[i].v*double(w - totalW) / candies[i].w;
			break;

		}
	}
	printf("%.1f",totalV);
	system("pause");
	return 0;

}
```

#### 测试用例

样例输入 

4 15

100 4

 412 8 

266 7 

591 2 

样例输出 1193.0





### 看电影

#### 题解

![dianying](https://github.com/sspkuxuan/algorithm_study/raw/master/tanxin/dianying1.png)

![dianying](https://github.com/sspkuxuan/algorithm_study/raw/master/tanxin/dianying2.png)

![dianying](https://github.com/sspkuxuan/algorithm_study/raw/master/tanxin/dianying3.png)

#### code c++

```c++
#include<iostream>
#include<algorithm>
#include<iomanip>
#include<cstdio>
#include<vector>
using namespace std;
const double eps = 1e-6;
struct Move{
	int start;
	int end;
	bool operator<(const Move &c)const{
		return  end - c.end < eps;
	}
};
void main(){
	vector<Move>move(110);
	int n;
	cin >> n;
	for (int i = 0; i < n; ++i){
		cin >> move[i].start >> move[i].end;
	}
	int res = 1;
	sort(move.begin(), move.begin() + n);
	int end =0 ;
	for (int i = 0; i < n; ++i){
		end = move[i].end;
		while (i < n){
			if (end <= move[i + 1].start){
				++res;
				break;
			}
			else ++i;
		}
	}
	cout << res;
	system("pause");
}
```



#### 样例输入

12
1 3
3 4
0 7
3 8
15 19
15 20
10 15
8 18
6 12
5 10
4 14
2 9
out is 5请按任意键继续. . .



### 喂奶牛

#### 题解

![nainiu](https://github.com/sspkuxuan/algorithm_study/raw/master/tanxin/nainiu1.png)

![nainiu](https://github.com/sspkuxuan/algorithm_study/raw/master/tanxin/nainiu2.png)

![nainiu](https://github.com/sspkuxuan/algorithm_study/raw/master/tanxin/nainiu3.png)

#### code c++

```c++
#include<iostream>
#include<algorithm>
#include<iomanip>
#include<cstdio>
#include<vector>
#include<queue>
using namespace std;
const double eps = 1e-6;
struct Cow
{
	int a, b;//挤奶牛区间
	int No;//编号
	bool operator<(const Cow & c)const{
		return a < c.a;
	}
};
int pos[50100];
struct Stall
{
	int end;
	int No;
	bool operator<(const Stall & s)const{
		return end>s.end;
	}
	Stall(int e, int n) :end(e), No(n){}

};
void main(){
	int n;
	cin >> n;
	vector<Cow>cows(50100);
	for (int i = 0; i < n; ++i){
		cin >> cows[i].a >> cows[i].b;
		cows[i].No = i;
	}
	sort(cows.begin(), cows.begin() + n);
	int total = 0;
	priority_queue<Stall>pq;
	for (int i = 0; i < n; ++i){
		if (pq.empty()){
			++total;
			pq.push(Stall(cows[i].b, i));
			pos[cows[i].No] = total;
		}
		else{
			Stall st = pq.top();
			if (st.end < cows[i].a){
				//当前吃奶结束时间小于下头牛开始时间，不需要添加牛棚
				pq.pop();
				pos[cows[i].No] = st.No;
				pq.push(Stall(cows[i].b, st.No));

			}
			else{
				++total;//需要加牛棚
				pq.push(Stall(cows[i].b, total));
				//将吃奶结束时间和牛棚号写进去
				pos[cows[i].No] = total;
				//牛在第几个牛棚排队吃奶
			}
		}
	}
	cout <<"out is"<<endl<< total << endl;
	for (int i = 0; i < n; ++i){
		cout << pos[i] << endl;
		//第i个牛在第几号牛棚
	}
	system("pause");
}
```

#### 样例输入

5
1 10
2 4
3 6
5 8
4 7
out is
4
1
2
3
2
4
请按任意键继续. . .
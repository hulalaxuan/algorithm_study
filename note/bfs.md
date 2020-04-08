# 算法

## 第七章  宽度优先搜索算法

### bfs





### 奶牛

[百炼4001](http://bailian.openjudge.cn/practice/4001)

#### 题解



#### code c++

```c++
#include<iostream>
#include<queue>
using namespace std;
int N, K;
const int MAXN = 100000;
int visited[MAXN + 10];
struct Step{
	int x;
	int steps;
	Step(int xx, int s) :x(xx), steps(s){}
};
queue<Step> q;
int main(){
	cin >> N >> K;
	memset(visited, 0, sizeof(visited));
	q.push(Step(N, 0));
	visited[N] = 1;
	while (!q.empty()){
		Step s = q.front();
		if (s.x == K){ cout << s.steps; system("pause"); return 0; }
		else
		{
			if (s.x - 1 >= 0 && !visited[s.x - 1]){
				q.push(Step(s.x - 1, s.steps + 1));
				visited[s.x - 1] = 1;
			}
			if (s.x +1 <=MAXN && !visited[s.x + 1]){
				q.push(Step(s.x + 1, s.steps + 1));
				visited[s.x + 1] = 1;
			}
			if (s.x*2 <= MAXN && !visited[s.x*2]){
				q.push(Step(s.x*2, s.steps + 1));
				visited[s.x *2] = 1;
			}
		}
		q.pop();
	}
	system("pause");
	return 0;
}
```

### 八数码问题

#### 题解





#### code

```c++
#include<iostream>
#include<bitset>
#include<cstring>
#include<cstdio>
#include<cstdlib>
#include<set>
using namespace std;
int goalStatus = 123456780;
const int MAXS = 400000;
char result[MAXS];
struct Node{
	int status;
	int father;
	char move;
	Node(int s, int f, char m) :status(s), father(f), move(m){}
	Node(){}
};
Node myQueue[MAXS];
int qHead = 0;//队头指针
int qTail = 1;//队尾指针
char moves[] = "udrl";

int NewStatus(int status, char move){
	//求status 经过move得到的新状态
	char tmp[20];
	int zeroPos;//字符‘0’的位置
	//int j = 8;
	//while (status ){
	//	int t = status % 10;
	//	tmp[j--] = t;
	//	status = status / 10;
	//}
	sprintf_s(tmp, "%09d", status);//保存前导0
	
	for (int i = 0; i < 9; ++i){
		if (tmp[i] == '0'){
			zeroPos = i;
			break;
		}//返回空格的位置

	}
	switch (move)
	{
	case 'u':
		if (zeroPos - 3 < 0)
			return -1;//空格在第一行
		else{
			tmp[zeroPos] = tmp[zeroPos - 3];
			tmp[zeroPos - 3] = '0';
		}
		break;
	case 'd':
		if (zeroPos + 3 >8)
			return -1;//空格在第san行
		else{
			tmp[zeroPos] = tmp[zeroPos + 3];
			tmp[zeroPos + 3] = '0';
		}
		break;
	case 'l':
		if (zeroPos % 3 == 0)
			return -1;//空格在第一lie
		else{
			tmp[zeroPos] = tmp[zeroPos - 1];
			tmp[zeroPos - 1] = '0';
		}
		break;
	case 'r':
		if (zeroPos % 3 == 2)
			return -1;//空格在第一行
		else{
			tmp[zeroPos] = tmp[zeroPos + 1];
			tmp[zeroPos + 1] = '0';
		}
		break;
	default:
		break;
	}
	return atoi(tmp);
}



bool Bfs(int status){
	int newStatus;
	set<int> expanded;
	myQueue[qHead] = Node(status, -1, 0);
	expanded.insert(status);
	while (qHead != qTail){
		//队列不为空
		status = myQueue[qHead].status;
		if (status == goalStatus)//找到目标状态
			return true;
		for (int i = 0; i < 4; ++i){
			//尝试四种移动
			newStatus = NewStatus(status, moves[i]);
			if (newStatus == -1)continue;//不可以移动下一种
			if (expanded.find(newStatus) != expanded.end())
				continue;//已经扩展过
			expanded.insert(newStatus);
			myQueue[qTail++] = Node(newStatus, qHead, moves[i]);
		}
		qHead++;
	}
	return false;
}



int main(){
	char line1[50]; char line2[20];
	while (cin.getline(line1, 48)){
		int i, j;
		for (i = 0, j = 0; line1[i]; i++){
			if (line1[i] != ' '){
				if(line1[i] == 'x')
					line2[j++] = '0';
				else line2[j++] = line1[i];
			}
		}
		line2[j] = 0;
		for (auto c : line2)cout << c << "  ";
		cout << endl;
		if (Bfs(atoi(line2))){
			int t = 0;
			int pos = qHead;
			do{
				result[t++] = myQueue[pos].move;
				pos = myQueue[pos].father;
			} while (pos);
			for (int i = t - 1; i >= 0; --i){
				cout << result[i];
			}
		}
		else
		{
			cout << "unsolvable" << endl;
		}
	}
}
```


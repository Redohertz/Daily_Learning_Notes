### DFS-深度优先搜索
**1. 回溯算法**  
（1）核心思想：递归遍历所有可能，错则**回退**。  
（2）核心代码：
```cpp
void backtrack(参数) {
 if (终止条件) {
  记录结果;
  return;
 }
 for (选择 : 本层可选项) {
  if (不合法) continue;
  标记选择;
  backtrack(更新参数); // 递归
  撤销标记;          // 回溯
 }
}
```
P.S. 写DFS代码时，为提高运行效率也常使用**剪枝优化**与**状态压缩**。  
[ 剪枝优化：在DFS中通过约束条件（如数独的行、列、块冲突检查）提前终止无效分支。]  
[ 状态压缩：将复杂状态用整数等表示，利用位运算高效处理以优化空间和提高运算效率。]

**2. 常见DFS模板**  
（1）排列问题模板  
生成所有排列，如全排列：  
````cpp
void dfs(int pos) {
 if (pos == n) {
  // 处理排列结果
  return;
 }
 for (int i = 0; i < n; i++) {
  if (!used[i]) {
   used[i] = true;
   path[pos] = i + 1; // 记录当前排列
   dfs(pos + 1);
   used[i] = false;   // 回溯
   }
 }
}
````
（2）组合问题模板  
从集合中选择k个数，生成所有组合：
````cpp
void dfs(int start, int depth) {
 if (depth == k) {
  // 处理组合结果
  return;
 }
 for (int i = start; i <= n; i++) {
  path.push_back(i);
  dfs(i + 1, depth + 1); // 从下一个位置开始选数
  path.pop_back();       // 回溯
 }
}
````
（3） 棋盘类问题模板  
如N皇后、数独等需要二维遍历的问题：  
````cpp
bool dfs(int x, int y) {
 if (y >= n) {            // 列越界，换行
  y = 0;
  x++;
  if (x >= n) return true; // 所有行已填完
 }
 if (board[x][y] != 0) {  // 跳过已填数字
  return dfs(x, y + 1);
 }
 for (int num = 1; num <= 4; num++) {
  if (isValid(x, y, num)) { // 检查行、列、块是否合法
   board[x][y] = num;
   if (dfs(x, y + 1)) return true;
   board[x][y] = 0;     // 回溯
  }
 }
 return false;
}
````

**3. 常见例题演示（难度从低到高）**  
（1）全排列型  
Q：生成1-n的所有排列（n<=8）  
A：
````cpp
void dfs(int pos){
 if(pos==n){
  输出当前排列;
  return;
 }
 for(int i=1;i<=n;i++){
  if(!used[i]){
   used[i] = true;
   path[pos] = i;
   dfs(pos+1);
   used[i] = false;
   }
 }
}
````
（2）迷宫路径计数（原题为**洛谷-P1605 迷宫**）  
Q： 给定一个 N×M 方格的迷宫，迷宫里有 T 处障碍，障碍处不可通过。在迷宫中移动有上下左右四种方式，每次只能移动一个方格。数据保证起点上没有障碍。  
给定起点坐标和终点坐标，每个方格最多经过一次，问有多少种从起点坐标到终点坐标的方案。  
A:
````cpp
int map[6][6];//储存地图；
bool temp[6][6];//标记；
int dx[4]={0,0,1,-1};//x方向偏移量；
int dy[4]={-1,1,0,0};//y方向偏移量；
int total,fx,fy,sx,sy,T,n,m,l,r;//total计数器，fx，fy是终点坐标，sx，sy是起点坐标，T是障碍总数，n，m是地图的长和宽，l，r是障碍的横坐标和纵坐标；

void walk(int x,int y){
 if(x==fx&&y==fy){//fx表示结束x坐标，fy表示结束y坐标；
  total++;//总数增加；
  return;//返回，继续搜索；
 }
 else{
 for(int i=0;i<=3;i++){//遍历左，右，下，上四个方向；
  if(temp[x+dx[i]][y+dy[i]]==0&&map[x+dx[i]][y+dy[i]]==1)//判断没有走过和没有障碍；{
   temp[x][y]=1;//走过的地方打上标记；
   walk(x+dx[i],y+dy[i]);
   temp[x][y]=0;//还原状态；
   }    
  } 
 }
}
````
（3）八皇后问题（剪枝强化）  
Q：在8×8棋盘上放置8个皇后，使得它们互不攻击（即不在同一行、列、对角线）。反馈方案。  
A：
````cpp
bool check(int row, int col) {
 for (int i = 0; i < row; i++) {
  if (queen[i] == col || abs(i - row) == abs(queen[i] - col)) 
  return false;
 }
 return true;
}
````
（4）吃奶酪（原题为**洛谷-P1433 吃奶酪**）（状压DP）  
Q：房间里放着 n 块奶酪。一只小老鼠要把它们都吃掉，问至少要跑多少距离？老鼠一开始在 (0,0) 点处。  
第一行有一个整数，表示奶酪的数量 n。 第2到第 (n+1) 行，每行两个实数，第 (i+1) 行的实数分别表示第 i 块奶酪的横纵坐标 x_i，y_i。  
输出一行一个实数，表示要跑的最少距离，保留 2 位小数。  
A：
````cpp
#include<bits/stdc++.h>
int n;
double a[20],b[20],dp[65000][20];
bool v[20];
double ans=1e9;
double dis(int x,int y){
	sqrt(pow(a[x]-a[y]),2)+pow(b[x]-b[y],2));//距离公式
}
void dfs(int t,int now,double s,int b){
	if(s>ans) return; //剪枝
	if(t==n){
		ans=ans<s?ans:s;
		return;
	}
	for(register int i=1;i<=n;i++){
		if(!v[i]){
			int p=b+(1<<(i-1));//此处使用位运算，状压DP
			if(dp[p][i]!=0 && dp[p][i]<=s+dis(now,i)) 
			 continue;//判断条件+优化
			v[i]=1;
			dp[p][i]=s+dis(now,i);
			dfs(t+1,i,dp[p][i],p);
			v[i]=0;//回溯
		}
	}
}
main(){
	scanf("%d",&n);
	for(register int i=1;i<=n;i++)
		scanf("%lf%lf",&a[i],&b[i]);
	dfs(0,0,0,0);
	printf("%.2lf\n",ans);
	return 0;
	
}
````
**4.练习记录**  
2025-03-22-蓝桥云课-**[玩具蛇](https://www.lanqiao.cn/problems/1022/learning/)**
````cpp
#include <bits/stdc++.h>
using namespace std;
int dx[]={-1,0,1,0}; // x方向偏移量
int dy[]={0,-1,0,1}; // y方向偏移量
// 两组偏移量合并表示四个方向（上下左右）
int st[4][4]; // 记录已放入🐍身的格子
int cnt=0; // 记录方案数

void dfs(int x,int y,int len)
{
  if(x<0||y<0||x>=4||y>=4) // 越界检查
  return;
  if(st[x][y]==1) // 已放置检查
  return;
  if(len==15)
  {
    cnt++; // 方案+1！
    return;
  }
  st[x][y]=1;
  for(int i=0;i<4;i++) // 四个方向都需要走
    dfs(x+dx[i],y+dy[i],len+1); // 递归探索
  st[x][y]=0; //结束一次探索，取消所有已放置标记
}
int main()
{
  for(int x=0;x<4;x++)
  for(int y=0;y<4;y++)
    dfs(x,y,0);

  cout << cnt << endl;
  return 0;
}
````
2025-03-23(1)-蓝桥云课-**[小朋友崇拜圈](https://www.lanqiao.cn/problems/182/learning/)**
````cpp
#include <iostream>
using namespace std;
int n,bgn,m=0;
int a[100005],b[100005];//a为储存数组，b为标记数组
// 要求：使小朋友形成最大圈
void dfs(int x,int y)
{
  if(b[x])
  {
  if(a[x]==a[bgn]) //若要成为【圈】，最后一个小朋友必定崇拜第一个小朋友
  {
  if(y>m)
  {
  m = y;
  return;
  }
  }
  }

  else
  {
    b[x]=1;
    dfs(a[x],y+1);
    b[x]=0;
  }
}
int main()
{
  cin >> n;
  for(int i=1;i<=n;i++)
  cin >> a[i];
  for(int i=1;i<=n;i++)
  {
    bgn = i;
    dfs(bgn,0);
  }
  cout << m << endl;
  return 0;
}
````
2025-03-23(2)-蓝桥云课-**[n皇后问题](https://www.lanqiao.cn/problems/1508/learning/)**
````cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 15;
int queen[N];
int n,ans = 0;
bool check(int row, int col) {
 for (int i = 0; i < row; i++) {
  if (queen[i] == col || abs(i - row) == abs(queen[i] - col)) 
  return false;
 }
 return true;
}
void dfs(int row){
  if(row==n){
    ans++;
    return;
  }
  for(int col=0;col<n;col++){
    if(check(row,col)){
      queen[row] = col;
      dfs(row+1);
      queen[row] -= 1;
    }
  }
}
int main()
{
  cin >> n;
  dfs(0);
  cout << ans << endl;
  return 0;
}
````

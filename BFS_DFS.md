# BFS

## 思路及应用场景

BFS在求无权边的最短路径时往往是最优解。
常用于走迷宫。
也常用于节点覆盖。

由于其算法思想特性：
**节点被访问后，访问他们的临近节点，已访问过的不在访问。**
（这样一层层的漫过）
高度契合先进先出的队列特性。
用队列实现最为便捷：
设计思想为：

**节点一旦被访问，他们的临近节点就会被加入队列末尾，当前访问节点从队列顶端取出**

可以认为实现关键是：**搜索相邻节点**

- 临接矩阵
  临接矩阵存储图时，通常采用
  
  1. 给节点编号（0开始，实际上数组本身下标用于编号即可）
  2. 标记相邻节点为1；例如arr[0][1]=1，就意味着0这个顶点和1这个顶点有连线。反之存0。
  
  ```CPP
  bool visited[V_NUM]; //访问标技数组
  int G[V_NUM][V_NUM];//邻接矩阵结构的图
  
  void BFS(Graph G,int v){
  	visit(v);//访问顶点v
  	visited[v] = true;//标记顶点v已访问
  	Enqueue(Q,v);//顶点v入队列Q
  	//队列不为空则继续循环
  	while(isEmpty(Q) == false){
  	DeQueue(Q,v);//队头出队，并复制给变量v
  	//查找邻接顶点，并对顶点v对应的邻接矩阵进行遍历
  	for(int i=0;i<V_NUM;i++){
  		//判断是否存在边，并且从未访问过
  		if(G[v][i] == 1 && visited[i] == false){
  			visit(i);//访问邻接顶点
  			visited[i] = true;
  			EnQueue(Q,i);//邻接顶点i入队
  		}
  	}
  	}
  }
  ```




- 临接链表
  临接链表存储图时，通常采用

1. 声明一个数组，其中每个节点存储一张链表。
2. 其中arr[2]存储了：顶点为2的临接链表。该链表存储数据前后顺序无所谓，代表了节点为2的点的所有临接节点。最后存null。

代码实现思想与前者一致，不过遍历方式发生一丝结构性变化。


## 实际应用代码参考

``` CPP
#include <iostream>
#include <vector>
#include <queue>

int main() {
    int H, W;
    std::cin >> H >> W;
    std::vector<std::string> grid(H);
    std::vector<std::vector<bool>> visited(H,std::vector<bool>(W,false));
    for (int i = 0; i < H; i++) std::cin >> grid[i];

    // 四方向（右，下，左，上）
    int dx[4] = {1, 0, -1, 0};
    int dy[4] = {0, 1, 0, -1};
    
    //先找到起始位置*
    int nx,ny;
    for(int i=0;i<H;i++){
        for(int j=0;j<W;j++){
            if(grid[i][j] == '*'){ nx = j;ny = i;}
        }
    }

    
    //std::cout<<"--test--\n"<<nx<<" "<<ny<<std::endl;
    //BFS
    std::queue<std::pair<int,int>> queue;
    visited[ny][nx] = true;
    queue.push({nx,ny});
    while(!queue.empty()){
        nx = queue.front().first;
        ny = queue.front().second;
        queue.pop();

        //弹出队头，并寻找其邻接节点
        for(int i=0;i<4;i++){
            //先计算新坐标，再验证该坐标是否越界，再访问
            int tx = nx + dx[i];
            int ty = ny + dy[i];
            if (ty < 0 || ty >= H || tx < 0 || tx >= W) continue;
            if(grid[ty][tx] == '.' && visited[ty][tx] == false){
                visited[ty][tx] = true;
                grid[ty][tx] = '*';
                queue.push({tx,ty});

            }
        }
    }



    //输出结果
    for (int i = 0; i < H; i++) {
        std::cout << grid[i] << '\n';
    }

    return 0;
}

```









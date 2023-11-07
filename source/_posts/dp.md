---
title: 【算法基础】动态规划（Dynamic Programming）
date: 2023-02-24 15:03:26
tags: [算法, 动态规划]
math: true
---

动态规划可以应用于多种问题，例如背包问题、最长公共子序列、最短路径问题等。
其灵活性也导致了此类题没有固定的算法模板，蓝桥杯省赛将至，将我之前的零散的笔记整理一下。
后续会完善残缺的题目，并且为代码加上详细注释。


<!-- more -->

# 数字三角形模型

## ****[898.数字三角形](https://www.acwing.com/problem/content/900/)****

- 题目
    
    给定一个如下图所示的数字三角形，从顶部出发，在每一结点可以选择移动至其左下方的结点或移动至其右下方的结点，一直走到底层，要求找出一条路径，使路径上的数字的和最大。
    
    ```
            7
          3   8
        8   1   0
      2   7   4   4
    4   5   2   6   5
    ```
    
    ### 输入格式
    
    第一行包含整数 n，表示数字三角形的层数。
    
    接下来 n 行，每行包含若干整数，其中第 i 行表示数字三角形第 i 层包含的整数。
    
    ### 输出格式
    
    输出一个整数，表示最大的路径数字和。
    
    ### 数据范围
    
    $1 \\le n \\le 500$,
    $\-10000 \\le 三角形中的整数 \\le 10000$
    
    ### 输入样例：
    
    ```
    5
    7
    3 8
    8 1 0
    2 7 4 4
    4 5 2 6 5
    ```
    
    ### 输出样例：
    
    ```
    30
    ```
    
- 解答
    
    ```cpp
    #include <iostream>
    #include <cstring>
    #include <algorithm>
     
    using namespace std;
    const int maxn = 510;
    int a[maxn][maxn];
     
    int main()
    {
    	int n;
        cin >> n;
        for (int i = 1; i <= n; i ++ )
        {
        	for (int j = 1; j <= i; j ++ )
        	{
        		cin >> a[i][j];
    		}
    	}                    
        for (int i = n - 1; i; i -- )
        {
        	for (int j = 1; j <= i; j ++ )
        	{
        		a[i][j] += max(a[i + 1][j], a[i + 1][j + 1]);
    		}
    	}                    
        cout << a[1][1] << endl;
        return 0;
    }
    ```
    

## ****[1015.摘花生](https://www.acwing.com/problem/content/1017/)****

- 题目
    
    Hello Kitty想摘点花生送给她喜欢的米老鼠。
    
    她来到一片有网格状道路的矩形花生地(如下图)，从西北角进去，东南角出来。
    
    地里每个道路的交叉点上都有种着一株花生苗，上面有若干颗花生，经过一株花生苗就能摘走该它上面所有的花生。
    
    Hello Kitty只能向东或向南走，不能向西或向北走。
    
    问Hello Kitty最多能够摘到多少颗花生。
    
    ![https://cdn.acwing.com/media/article/image/2019/09/12/19_a8509f26d5-1.gif](https://cdn.acwing.com/media/article/image/2019/09/12/19_a8509f26d5-1.gif)
    
    ### 输入格式
    
    第一行是一个整数T，代表一共有多少组数据。
    
    接下来是T组数据。
    
    每组数据的第一行是两个整数，分别代表花生苗的行数R和列数 C。
    
    每组数据的接下来R行数据，从北向南依次描述每行花生苗的情况。每行数据有C个整数，按从西向东的顺序描述了该行每株花生苗上的花生数目M。
    
    ### 输出格式
    
    对每组输入数据，输出一行，内容为Hello Kitty能摘到得最多的花生颗数。
    
    ### 数据范围
    
    $1 \\le T \\le 100$,
    $1 \\le R,C \\le 100$,
    $0 \\le M \\le 1000$
    
    ### 输入样例：
    
    ```
    2
    2 2
    1 1
    3 4
    2 3
    2 3 4
    1 6 5
    ```
    
    ### 输出样例：
    
    ```
    8
    16
    ```
    
- 解答
    
    ```cpp
    #include <iostream>
    #include <algorithm>
    
    using namespace std;
    
    const int N = 110;
    
    int n, m;
    int w[N][N];
    int f[N][N];
    
    int main()
    {
        int T;
        scanf("%d", &T);
        while (T -- )
        {
            scanf("%d%d", &n, &m);
            for (int i = 1; i <= n; i ++ )
                for (int j = 1; j <= m; j ++ )
                    scanf("%d", &w[i][j]);
    
            for (int i = 1; i <= n; i ++ )
                for (int j = 1; j <= m; j ++ )
                    f[i][j] = max(f[i - 1][j], f[i][j - 1]) + w[i][j];
    
            printf("%d\n", f[n][m]);
        }
    
        return 0;
    }
    ```
    

## 全新的DP问题思考方式——从集合的角度来考虑问题

![动态规划 (1).png](/img/group.png)

## ****[1018. 最低通行费](https://www.acwing.com/problem/content/1020/)****

- 题目
    
    一个商人穿过一个 N×N 的正方形的网格，去参加一个非常重要的商务活动。
    
    他要从网格的左上角进，右下角出。
    
    每穿越中间 1 个小方格，都要花费 1 个单位时间。
    
    **商人必须在 (2N-1) 个单位时间穿越出去**。
    
    而在经过中间的每个小方格时，都需要缴纳一定的费用。
    
    这个商人期望在规定时间内用最少费用穿越出去。
    
    请问至少需要多少费用？
    
    注意：不能对角穿越各个小方格（即，只能向上下左右四个方向移动且不能离开网格）。
    
    ### 输入格式
    
    第一行是一个整数，表示正方形的宽度 N。
    
    后面 N 行，每行 N 个不大于 100 的正整数，为网格上每个小方格的费用。
    
    ### 输出格式
    
    输出一个整数，表示至少需要的费用。
    
    ### 数据范围
    
    $1 \\le N \\le 100$
    
    ### 输入样例：
    
    ```
    5
    1  4  6  8  10
    2  5  7  15 17
    6  8  9  18 20
    10 11 12 19 21
    20 23 25 29 33
    ```
    
    ### 输出样例：
    
    ```
    109
    ```
    
    ### 样例解释
    
    样例中，最小值为 109=1+2+5+7+9+12+19+21+33。
    
- 解答
    
    商人要在2N-1的时间内出去，说明商人不能走回头路。
    
    即只能向下或者向右移动。
    
    ```cpp
    #include<iostream>
    #include<algorithm>
    const int N = 110, INF = 1e9;
    
    int f[N][N], w[N][N];
    
    using namespace std;
    
    int main(){
        int n;
        scanf("%d", &n);
        
        for(int i = 1; i <= n; ++ i)
            for(int j = 1; j <= n; ++ j)
                scanf("%d", &w[i][j]);
                
        for(int i = 1; i <= n; ++ i)
            for(int j = 1; j <= n; ++ j)
                if(i == 1 && j == 1) f[i][j] = w[i][j];
                else {
                    f[i][j] = INF;
                    if(i > 1) f[i][j] = min(f[i][j], f[i - 1][j] + w[i][j]);
                    if(j > 1) f[i][j] = min(f[i][j], f[i][j - 1] + w[i][j]);
                }
                
        printf("%d", f[n][n]);
        return 0;
    }
    ```
    

## ****[1027.方格取数](https://www.acwing.com/problem/content/1029/)****

- 题目
    
    设有 N×N 的方格图，我们在其中的某些方格中填入正整数，而其它的方格中则放入数字0。如下图所示：
    
    ![https://cdn.acwing.com/media/article/image/2019/09/12/19_764ece6ed5-2.gif](https://cdn.acwing.com/media/article/image/2019/09/12/19_764ece6ed5-2.gif)
    
    某人从图中的左上角 A 出发，可以向下行走，也可以向右行走，直到到达右下角的 B 点。
    
    在走过的路上，他可以取走方格中的数（取走后的方格中将变为数字0）。
    
    此人从 A 点到 B 点共走了两次，试找出两条这样的路径，使得取得的数字和为最大。
    
    ### 输入格式
    
    第一行为一个整数N，表示 N×N 的方格图。
    
    接下来的每行有三个整数，第一个为行号数，第二个为列号数，第三个为在该行、该列上所放的数。
    
    行和列编号从 1 开始。
    
    一行“0 0 0”表示结束。
    
    ### 输出格式
    
    输出一个整数，表示两条路径上取得的最大的和。
    
    ### 数据范围
    
    $N \\le 10$
    
    ### 输入样例：
    
    ```
    8
    2 3 13
    2 6 6
    3 5 7
    4 4 14
    5 2 21
    5 6 4
    6 3 15
    7 2 14
    0 0 0
    ```
    
    ### 输出样例：
    
    ```
    67
    ```
    
- 解答
    
    👇在此题中“最优解一定不会由两段相交的路径组成”
    
    文章：[AcWing 275. 证明传纸条为何可以使用方格取数的代码](https://www.acwing.com/solution/content/12389/)
    
    同一个格子不能被重复选择
    
    不能分两步进行选择，因为一次的最优解不一定是整体的最优解
    
    当i1+j1==i2+j2时格子才有可能重合
    
    令i1+j1=i2+j2=k，f[k, i1, i2]表示从(1, 1)分别走到(i1, k-i1)和(i2, k-i2)的集合的最大值
    
    ```cpp
    #include<iostream>
    using namespace std;
    
    const int N = 12;
    
    int w[N][N];
    
    int dp[N * 2][N][N];
    
    int main(){
        int n;
        scanf("%d", &n);
        
        int a, b ,c;
        
        while(cin >> a >> b >> c, a || b || c) w[a][b] = c;
        
        for(int k = 2; k <= 2 * n; k ++){
            for(int i1 = 1; i1 <= n; i1 ++){
                for(int i2 = 1; i2 <= n; i2 ++){
                    int j1 = k - i1, j2 = k - i2;
                    if(j1 >= 1 && j1 <= n && j2 >= 1 && j2 <= n){
                        int t = w[i1][j1];
                        if(i1 != i2) t += w[i2][j2];
                        int &x = dp[k][i1][i2];
                        x = max(x, dp[k - 1][i1 - 1][i2 - 1] + t);
                        x = max(x, dp[k - 1][i1 - 1][i2] + t);
                        x = max(x, dp[k - 1][i1][i2 - 1] + t);
                        x = max(x, dp[k - 1][i1][i2] + t);
                    }
                }
            }
        }
        
        printf("%d", dp[n * 2][n][n]);
        
        return 0;
        
    }
    ```
    

## ****[275.传纸条](https://www.acwing.com/problem/content/277/)****

- 题目
    
    小渊和小轩是好朋友也是同班同学，他们在一起总有谈不完的话题。
    
    一次素质拓展活动中，班上同学安排坐成一个 $m$ 行 $n$ 列的矩阵，而小渊和小轩被安排在矩阵对角线的两端，因此，他们就无法直接交谈了。
    
    幸运的是，他们可以通过传纸条来进行交流。
    
    纸条要经由许多同学传到对方手里，小渊坐在矩阵的左上角，坐标 $(1,1)$，小轩坐在矩阵的右下角，坐标 $(m,n)$。
    
    从小渊传到小轩的纸条只可以向下或者向右传递，从小轩传给小渊的纸条只可以向上或者向左传递。
    
    在活动进行中，小渊希望给小轩传递一张纸条，同时希望小轩给他回复。
    
    班里每个同学都可以帮他们传递，但只会帮他们一次，也就是说如果此人在小渊递给小轩纸条的时候帮忙，那么在小轩递给小渊的时候就不会再帮忙，反之亦然。
    
    还有一件事情需要注意，全班每个同学愿意帮忙的好感度有高有低（注意：小渊和小轩的好心程度没有定义，输入时用 $0$ 表示），可以用一个 $0 \\sim 100$ 的自然数来表示，数越大表示越好心。
    
    小渊和小轩希望尽可能找好心程度高的同学来帮忙传纸条，即找到来回两条传递路径，使得这两条路径上同学的好心程度之和最大。
    
    现在，请你帮助小渊和小轩找到这样的两条路径。
    
    ### 输入格式
    
    第一行有 $2$ 个用空格隔开的整数 $m$ 和 $n$，表示学生矩阵有 $m$ 行 $n$ 列。
    
    接下来的 $m$ 行是一个 $m \\times n$ 的矩阵，矩阵中第 $i$ 行 $j$ 列的整数表示坐在第 $i$ 行 $j$ 列的学生的好心程度，每行的 $n$ 个整数之间用空格隔开。
    
    ### 输出格式
    
    输出一个整数，表示来回两条路上参与传递纸条的学生的好心程度之和的最大值。
    
    ### 数据范围
    
    $1 \\le n,m \\le 50$
    
    ### 输入样例：
    
    ```
    3 3
    0 3 9
    2 8 5
    5 7 0
    ```
    
    ### 输出样例：
    
    ```
    34
    ```
    
- 解答
    
    [AcWing 275. 传纸条, 不必使用方格取数的代码, 强制不让其走同一个格子即可](https://www.acwing.com/solution/content/57696/)
    
    ```cpp
    #include<iostream>
    using namespace std;
    
    const int N = 60, INF = 1e9;
    
    int w[N][N], f[N * 2][N][N];
    
    int main(){
        
        int n, m;
        cin >> m >> n;
        
        for(int i = 1; i <= m; i ++)
            for(int j = 1; j <= n; j ++)
                cin >> w[i][j];
                
        for(int k = 2; k <= n + m; k ++)
            for(int i1 = 1; i1 <= m; i1 ++)
                for(int i2 = 1; i2 <= m; i2 ++){
                    int j1 = k - i1, j2 = k - i2;
                    if(j1 >= 1 && j1 <= n && j2 >= 1 && j2 <= n){
                        if(i1 == i2 && k != m + n) {
                            f[k][i1][i2] = -INF;
                            continue;
                        }
                        int t = w[i1][j1] + w[i2][j2];
                        int &x = f[k][i1][i2];
                        x = max(x, f[k - 1][i1 - 1][i2 - 1] + t);
                        x = max(x, f[k - 1][i1][i2 - 1] + t);
                        x = max(x, f[k - 1][i1 - 1][i2] + t);
                        x = max(x, f[k - 1][i1][i2] + t);
                    }
                }
                
        cout << f[n + m][m][m];
        
        return 0;
    }
    ```


# 最长上升子序列模型

**最长上升子序列**（Longest Increasing Subsequence）

![最长上升子序列问题 (1).png](/img/LIS.png)

## ****[1017. 怪盗基德的滑翔翼](https://www.acwing.com/activity/content/problem/content/1259/)****

- 题目
    
    怪盗基德是一个充满传奇色彩的怪盗，专门以珠宝为目标的超级盗窃犯。
    
    而他最为突出的地方，就是他每次都能逃脱中村警部的重重围堵，而这也很大程度上是多亏了他随身携带的便于操作的滑翔翼。
    
    有一天，怪盗基德像往常一样偷走了一颗珍贵的钻石，不料却被柯南小朋友识破了伪装，而他的滑翔翼的动力装置也被柯南踢出的足球破坏了。
    
    不得已，怪盗基德只能操作受损的滑翔翼逃脱。
    
    假设城市中一共有 $N$ 幢建筑排成一条线，每幢建筑的高度各不相同。
    
    初始时，怪盗基德可以在任何一幢建筑的顶端。
    
    他可以选择一个方向逃跑，但是不能中途改变方向（因为中森警部会在后面追击）。
    
    因为滑翔翼动力装置受损，他只能往下滑行（即：只能从较高的建筑滑翔到较低的建筑）。
    
    他希望尽可能多地经过不同建筑的顶部，这样可以减缓下降时的冲击力，减少受伤的可能性。
    
    请问，他最多可以经过多少幢不同建筑的顶部(包含初始时的建筑)？
    
    ### 输入格式
    
    输入数据第一行是一个整数K，代表有K组测试数据。
    
    每组测试数据包含两行：第一行是一个整数N，代表有N幢建筑。第二行包含N个不同的整数，每一个对应一幢建筑的高度h，按照建筑的排列顺序给出。
    
    ### 输出格式
    
    对于每一组测试数据，输出一行，包含一个整数，代表怪盗基德最多可以经过的建筑数量。
    
    ### 数据范围
    
    $1 \\le K \\le 100$,
    $1 \\le N \\le 100$,
    $0 < h < 10000$
    
    ### 输入样例：
    
    ```
    3
    8
    300 207 155 299 298 170 158 65
    8
    65 158 170 298 299 155 207 300
    10
    2 1 3 4 5 6 7 8 9 10
    ```
    
    ### 输出样例：
    
    ```
    6
    6
    9
    ```
    
- 解答
    
    ```cpp
    #include<iostream>
    using namespace std;
    
    const int N = 110;
    
    int f[N];
    
    int a[N];
    
    int main(){
        int T;
        cin >> T;
        while(T --){
            int n;
            cin >> n;
            for(int i = 0; i < n; i ++) cin >> a[i];
            
            int res = 0;
            for(int i = 0; i < n; i ++){
                f[i] = 1;
                for(int j = 0; j < i; j ++)
                    if(a[i] < a[j])
                        f[i] = max(f[i], f[j] + 1);
                res = max(res, f[i]);
            }
            
            for(int i = n; i >= 0; i --){
                f[i] = 1;
                for(int j = n - 1; j > i; j --)
                    if(a[i] < a[j])
                        f[i] = max(f[i], f[j] + 1);
                res = max(res, f[i]);
            }
            
            cout << res << endl;
        }
        
        return 0;
    }
    ```
    

## ****[1014. 登山](https://www.acwing.com/activity/content/problem/content/1260/)****

- 题目
    
    五一到了，ACM队组织大家去登山观光，队员们发现山上一共有 $N$ 个景点，并且决定按照顺序来浏览这些景点，即每次所浏览景点的编号都要大于前一个浏览景点的编号。
    
    同时队员们还有另一个登山习惯，就是不连续浏览海拔相同的两个景点，并且一旦开始下山，就不再向上走了。
    
    队员们希望在满足上面条件的同时，尽可能多的浏览景点，你能帮他们找出最多可能浏览的景点数么？
    
    ### 输入格式
    
    第一行包含整数N，表示景点数量。
    
    第二行包含N个整数，表示每个景点的海拔。
    
    ### 输出格式
    
    输出一个整数，表示最多能浏览的景点数。
    
    ### 数据范围
    
    $2 \\le N \\le 1000$
    
    ### 输入样例：
    
    ```
    8
    186 186 150 200 160 130 197 220
    ```
    
    ### 输出样例：
    
    ```
    4
    ```
    
- 解答
    
    ```cpp
    #include<iostream>
    
    using namespace std;
    
    const int N = 1010;
    
    int a[N], f[N], g[N]; //分别保存最长上升和下降的子序列长度
    
    int main(){
        
        int n;
        cin >> n;
        for(int i = 1; i <= n; i ++) cin >> a[i];
        
        for(int i = 1; i <= n; i ++){
            f[i] = 1;
            for(int j = 1; j < i; j ++)
                if(a[i] > a[j])
                    f[i] = max(f[i], f[j] + 1);
        }
        
        for(int i = n; i ; i --){
            g[i] = 1;
            for(int j = n; j > i; j --)
                if(a[i] > a[j])
                    g[i] = max(g[i], g[j] + 1);
        }
        
        int res = 0;
        
        for(int i = 1; i <= n; i ++){
            res = max(res, f[i] + g[i] - 1); //其本身被重复选择了一次
        }
        
        cout << res << endl;
        
        return 0;
    }
    ```
    

## ****[1012. 友好城市](https://www.acwing.com/problem/content/1014/)****

- 题目
    
    Palmia国有一条横贯东西的大河，河有笔直的南北两岸，岸上各有位置各不相同的N个城市。
    
    北岸的每个城市有且仅有一个友好城市在南岸，而且不同城市的友好城市不相同。
    
    每对友好城市都向政府申请在河上开辟一条直线航道连接两个城市，但是由于河上雾太大，政府决定避免任意两条航道交叉，以避免事故。
    
    编程帮助政府做出一些批准和拒绝申请的决定，使得在保证任意两条航线不相交的情况下，被批准的申请尽量多。
    
    ### 输入格式
    
    第1行，一个整数N，表示城市数。
    
    第2行到第n+1行，每行两个整数，中间用1个空格隔开，分别表示南岸和北岸的一对友好城市的坐标。
    
    ### 输出格式
    
    仅一行，输出一个整数，表示政府所能批准的最多申请数。
    
    ### 数据范围
    
    $1 \\le N \\le 5000$,
    $0 \\le x\_i \\le 10000$
    
    ### 输入样例：
    
    ```
    7
    22 4
    2 6
    10 3
    15 12
    9 8
    17 17
    4 2
    
    ```
    
    ### 输出样例：
    
    ```
    4
    
    ```
    
- 解答
    
    ```cpp
    #include<iostream>
    #include<algorithm>
    using namespace std;
    
    const int N = 100010;
    
    typedef pair<int, int> PII;
    
    PII a[N];
    
    int f[N];
    
    int main(){
        
        int n;
        cin >> n;
        for(int i = 0; i < n; i ++) cin >> a[i].first >> a[i].second;
        
        sort(a, a + n);
        
        int res = 0;
        
        for(int i = 0; i < n; i ++){
            f[i] = 1;
            for(int j = 0; j < i; j ++)
                if(a[i].second > a[j].second)
                    f[i] = max(f[i], f[j] + 1);
                    
            res = max(res, f[i]);
        }
        
        cout << res << endl;
        
    }
    ```
    

## ****[1016. 最大上升子序列和](https://www.acwing.com/problem/content/1018/)****

- 题目
    
    给定一个长度为 $n$ 的整数序列 $a\_1,a\_2,…,a\_n$。

    请你选出一个该序列的严格上升子序列，要求所选子序列的各元素之和尽可能大。

    请问这个最大值是多少？   
 
    你的任务，就是对于给定的序列，求出最大上升子序列和。
    
    注意，最长的上升子序列的和不一定是最大的，比如序列(100,1,2,3)的最大上升子序列和为100，而最长上升子序列为(1,2,3)。
    
    ### 输入格式
    
    输入的第一行是序列的长度N。
    
    第二行给出序列中的N个整数，这些整数的取值范围都在0到10000(可能重复)。
    
    ### 输出格式
    
    输出一个整数，表示最大上升子序列和。
    
    ### 数据范围
    
    $1 \\le N \\le 1000$
    
    ### 输入样例：
    
    ```
    7
    1 7 3 5 9 4 8
    ```
    
    ### 输出样例：
    
    ```
    18
    ```
    
- 解答
    
    ```cpp
    #include<iostream>
    using namespace std;
    const int N = 1010;
    int f[N], a[N];
    int main(){
        int n;
        cin >> n;
        for(int i = 0; i < n; i ++) cin >> a[i];
        
        int res = 0;
        for(int i = 0; i < n; i ++) {
            f[i] = a[i];
            for(int j = 0; j < i; j ++)
                if(a[i] > a[j])
                    f[i] = max(f[i], f[j] + a[i]);
                    
            res = max(res, f[i]);
        }
        cout << res << endl;
    		return 0;
    }
    ```
    

---

## ****[1010. 拦截导弹](https://www.acwing.com/activity/content/problem/content/1264/)****

- 题目
    
    某国为了防御敌国的导弹袭击，发展出一种导弹拦截系统。
    
    但是这种导弹拦截系统有一个缺陷：虽然它的第一发炮弹能够到达任意的高度，但是以后每一发炮弹都不能高于前一发的高度。
    
    某天，雷达捕捉到敌国的导弹来袭。
    
    由于该系统还在试用阶段，所以只有一套系统，因此有可能不能拦截所有的导弹。
    
    输入导弹依次飞来的高度（雷达给出的高度数据是不大于30000的正整数，导弹数不超过1000），计算这套系统最多能拦截多少导弹，如果要拦截所有导弹最少要配备多少套这种导弹拦截系统。
    
    ### 输入格式
    
    共一行，输入导弹依次飞来的高度。
    
    ### 输出格式
    
    第一行包含一个整数，表示最多能拦截的导弹数。
    
    第二行包含一个整数，表示要拦截所有导弹最少要配备的系统数。
    
    ### 数据范围
    
    雷达给出的高度数据是不大于 $30000$ 的正整数，导弹数不超过 $1000$。
    
    ### 输入样例：
    
    ```
    389 207 155 300 299 170 158 65
    ```
    
    ### 输出样例：
    
    ```
    6
    2
    ```
    
- 解答
    
    贪心的流程：
    
    从前往后扫描每个数，对于每个数
    
    情况1：如果现有的子序列的结尾都小于当前数。则创建新子数列；
    
    情况2：将当前的数放到结尾大于等于它的最小的子序列后面；
    
    如何证明两个数相等？A≥B, A≤B
    
    A表示贪心算法得到的序列的个数，B表示最优解
    
    B≤A
    
    A≤B，调整法
    
    假设最优解对应的方案和当前的方案不同。
    
    找到第一个不同的数


# 背包问题

## 01背包问题：只能用0或者1次

有 $N$ 件物品和一个容量为 $V$ 的背包，每件物品有各自的价值且只能被选择一次，要求在有限的背包容量下，装入的物品总价值最大。
    
```cpp
for(int i = 1; i <= n; i++) 
        for(int j = 1; j <= m; j++)
        {
            //  当前背包容量装不进第i个物品，则价值等于前i-1个物品
            if(j < v[i]) 
                f[i][j] = f[i - 1][j];
            // 能装，需进行决策是否选择第i个物品
            else    
                f[i][j] = max(f[i - 1][j], f[i - 1][j - v[i]] + w[i]);
        }  
```

#### 将数组优化到一维，在一维的状况下体积要[从大到小枚举](https://www.acwing.com/solution/content/1374/)

将f[i][j]优化到f[j]，过程中只需要做一个等价变形

一维时，j的枚举是从m(总体积)到v[i]，因为体积大的由体积小的更新，我们要确保第i的状态是由i-1的状态更新而来。

    
```cpp
for(int i = 1; i <= n; i++)
{
    for(int j = m; j >= v[i]; j--)  
        f[j] = max(f[j], f[j - v[i]] + w[i]);
}
```
    
## 完全背包问题：每种物品可以用无限次（求前缀的最大值）

有 $N$ 件物品和一个容量为 $V$ 的背包，每种物品都有无限件可用。要求在有限的背包容量下，装入的物品总价值最大。
    
#### 最朴素的做法
    
```cpp
for(int i = 1; i<=n; i ++)
    for(int j = 0; j<=m; j ++)
    {
        for(int k = 0; k * v[i] <= j; k ++)
            f[i][j] = max(f[i][j], f[i - 1][j - k * v[i]] + k * w[i]);
    }
```


#### 优化的思路
    

```cpp
f[i , j ] = max( f[i-1,j] , f[i-1,j-v]+w ,  f[i-1,j-2*v]+2*w , f[i-1,j-3*v]+3*w , .....)
f[i , j-v]= max(            f[i-1,j-v]   ,  f[i-1,j-2*v] + w , f[i-1,j-3*v]+2*w , .....)
// 由上两式，可得出如下递推关系： 
                f[i][j] = max(f[i, j - v] + w, f[i - 1][j])
```
    
```cpp
// 有了上面的关系，那么其实k循环可以不要了，核心代码优化成这样：
for(int i = 1 ; i <=n ;i++)
	for(int j = 0 ; j <=m ;j++)
 	{
 	    f[i][j] = f[i - 1][j];
    	if(j - v[i] >= 0)
    	    f[i][j] = max(f[i][j], f[i][j - v[i]] + w[i]);
    }
    
for(int i = 1; i <= n; i ++)
    for(int j = v[i]; j <= m; j ++)//注意了，这里的j是从小到大枚举，和01背包不一样
    {
            f[j] = max(f[j], f[j - v[i]] + w[i]);
    }
```
    
## 多重背包问题：每种物品有数量限制（求滑动窗口内的最大值）（单调队列优化）

有 $N$ 种物品和一个容量是 $V$ 的背包。

第 $i$ 种物品最多有 $s\_i$ 件，每件体积是 $v\_i$，价值是 $w\_i$。

求解将哪些物品装入背包，可使物品体积总和不超过背包容量，且价值总和最大。输出最大价值。

### 技巧1

$s\_i$ == 1 时，可看作是01背包；$s\_i$ > 1 时，可拆解成01背包。

所以可以把多重背包拆成01背包，👇见代码。

``` cpp
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 10010;
int v[N], w[N], f[N], _v, _w, s, n, m, t;
int main() {
    cin >> n >> m;
    for(int i = 1; i <= n; i ++) {
        cin >> _v >> _w >> s;
        while(s --) {
            v[++ t] = _v;
            w[t] = _w;
        }
    }
    for(int i = 1; i <= t; i ++)
        for(int j = m; j >= v[i]; j --)
            f[j] = max(f[j], f[j - v[i]] + w[i]);
            
    cout << f[m] << endl;
}
```

### 技巧2

上段代码可以解决 

$0 \\lt N，V \\le 100$
$0 \\lt v\_i, w\_i, s\_i \\le 200$ 时的问题

但是在

$0 \\lt N \\le 1000$  
$0 \\lt V \\le 2000$  
$0 \\lt v\_i, w\_i, s\_i \\le 2000$  的范围下会超时

我们可以改进算法，将多重背包按二进制位进行拆分，👇见代码。

``` cpp
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;
const int N = 20010, M = 2010;
int v[N], w[N], f[M], _v, _w, s, n, m, t;
int main() {
    cin >> n >> m;
    for(int i = 1; i <= n; i ++) {
        cin >> _v >> _w >> s;
        int k = 1;
        while(k <= s) {
            v[++ t] = k * _v;
            w[t] = k * _w;
            s -= k;
            k <<= 1;
        }
        if(s > 0) {
            v[++ t] = s * _v;
            w[t] = s * _w;
        }
    }
    for(int i = 1; i <= t; i ++)
        for(int j = m; j >= v[i]; j --)
            f[j] = max(f[j], f[j - v[i]] + w[i]);
            
    cout << f[m] << endl;
}
```


总结：

当空间优化成一维后，只有完全背包的体积是从小到大循环的

背包问题的循环方式：物品 → 体积 → 决策（集合中）


# 状态机模型

## **1049. 大盗阿福**

- 题目
    
    阿福是一名经验丰富的大盗。趁着月黑风高，阿福打算今晚洗劫一条街上的店铺。
    
    这条街上一共有 $N$ 家店铺，每家店中都有一些现金。
    
    阿福事先调查得知，只有当他同时洗劫了两家相邻的店铺时，街上的报警系统才会启动，然后警察就会蜂拥而至。
    
    作为一向谨慎作案的大盗，阿福不愿意冒着被警察追捕的风险行窃。
    
    他想知道，在不惊动警察的情况下，他今晚最多可以得到多少现金？
    
    ### 输入格式
    
    输入的第一行是一个整数 $T$，表示一共有 $T$ 组数据。
    
    接下来的每组数据，第一行是一个整数 $N$ ，表示一共有 $N$ 家店铺。
    
    第二行是 $N$ 个被空格分开的正整数，表示每一家店铺中的现金数量。
    
    每家店铺中的现金数量均不超过1000。
    
    ### 输出格式
    
    对于每组数据，输出一行。
    
    该行包含一个整数，表示阿福在不惊动警察的情况下可以得到的现金数量。
    
    ### 数据范围
    
    $1 \\le T \\le 50$,
    $1 \\le N \\le 10^5$
    
    ### 输入样例：
    
    ```
    2
    3
    1 8 2
    4
    10 7 6 14
    ```
    
    ### 输出样例：
    
    ```
    8
    24
    ```
    
    ### 样例解释
    
    对于第一组样例，阿福选择第2家店铺行窃，获得的现金数量为8。
    
    对于第二组样例，阿福选择第1和4家店铺行窃，获得的现金数量为10+14=24。
    
- 解答
    
    ```cpp
    #include <iostream>
    #include <algorithm>
    
    using namespace std;
    
    const int N = 100010;
    
    int n;
    int w[N], f[N][2];
    
    int main()
    {
        int T;
        scanf("%d", &T);
        while (T -- )
        {
            scanf("%d", &n);
            for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);
    
            for (int i = 1; i <= n; i ++ )
            {
                f[i][0] = max(f[i - 1][0], f[i - 1][1]);
                f[i][1] = f[i - 1][0] + w[i];
            }
    
            printf("%d\n", max(f[n][0], f[n][1]));
        }
    
        return 0;
    }
    ```
    

## **1057. 股票买卖 IV**

- 题目
    
    给定一个长度为 $N$ 的数组，数组中的第 $i$ 个数字表示一个给定股票在第 $i$ 天的价格。
    
    设计一个算法来计算你所能获取的最大利润，你最多可以完成 $k$ 笔交易。
    
    注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。一次买入卖出合为一笔交易。
    
    ### 输入格式
    
    第一行包含整数 $N$ 和 $k$，表示数组的长度以及你可以完成的最大交易笔数。
    
    第二行包含 $N$ 个不超过 $10000$ 的正整数，表示完整的数组。
    
    ### 输出格式
    
    输出一个整数，表示最大利润。
    
    ### 数据范围
    
    $1 \\le N \\le 10^5$,
    $1 \\le k \\le 100$
    
    ### 输入样例1：
    
    ```
    3 2
    2 4 1
    ```
    
    ### 输出样例1：
    
    ```
    2
    ```
    
    ### 输入样例2：
    
    ```
    6 2
    3 2 6 5 0 3
    ```
    
    ### 输出样例2：
    
    ```
    7
    ```
    
    ### 样例解释
    
    样例1：在第 1 天 (股票价格 = 2) 的时候买入，在第 2 天 (股票价格 = 4) 的时候卖出，这笔交易所能获得利润 = 4-2 = 2 。
    
    样例2：在第 2 天 (股票价格 = 2) 的时候买入，在第 3 天 (股票价格 = 6) 的时候卖出, 这笔交易所能获得利润 = 6-2 = 4 。随后，在第 5 天 (股票价格 = 0) 的时候买入，在第 6 天 (股票价格 = 3) 的时候卖出, 这笔交易所能获得利润 = 3-0 = 3 。共计利润 4+3 = 7.
    
- 解答
    
    ```cpp
    #include<iostream>
    #include<cstring>
    const int N = 100010, M = 110;
    int f[N][M][2], w[N];
    int n, m;
    using namespace std;
    int main(){
        cin >> n >> m;
        for(int i = 1; i <= n; i ++) cin >> w[i];
        memset(f, -0x3f, sizeof f);
        for(int i = 0; i <= n; i ++) f[i][0][0] = 0;//昨天没有交易，初始化为0
        for(int i = 1; i <= n; i ++)
            for(int j = 1; j <= m; j ++){
    						//我们将买入一次看做交易次数加一
                f[i][j][0] = max(f[i - 1][j][0], f[i - 1][j][1] + w[i]);
    						//第i天状态是空仓状态(j=0)，则第i天可能产生的行为是卖出行为或空仓行为
                f[i][j][1] = max(f[i - 1][j][1], f[i - 1][j - 1][0] - w[i]);
    						//第i天状态是持仓状态(j=1)，则第i天可能产生的行为是买入行为或持仓行为
            }
        int res = 0; // 不能亏本
        for(int i = 0; i <= m; i ++) res = max(res, f[n][i][0]);
        cout << res ;
        return 0;
    }
    ```
    

## ****1058. 股票买卖 V****

- 题目
    
    给定一个长度为 $N$ 的数组，数组中的第 $i$ 个数字表示一个给定股票在第 $i$ 天的价格。
    
    设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:
    
    - 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
    - 卖出股票后，你无法在第二天买入股票 (即冷冻期为 $1$ 天)。
    
    ### 输入格式
    
    第一行包含整数 $N$，表示数组长度。
    
    第二行包含 $N$ 个不超过 $10000$ 的正整数，表示完整的数组。
    
    ### 输出格式
    
    输出一个整数，表示最大利润。
    
    ### 数据范围
    
    $1 \\le N \\le 10^5$
    
    ### 输入样例：
    
    ```
    5
    1 2 3 0 2
    ```
    
    ### 输出样例：
    
    ```
    3
    ```
    
    ### 样例解释
    
    对应的交易状态为: \[买入, 卖出, 冷冻期, 买入, 卖出\]，第一笔交易可得利润 2-1 = 1，第二笔交易可得利润 2-0 = 2，共得利润 1+2 = 3。
    
- 解答
    
    ![Untitled](img/status_machine.png)
    
    ```cpp
    #include<iostream>
    #include<cstring>
    using namespace std;
    const int N = 100010, INF = 0x3f3f3f3f;
    int f[N][3], w[N];
    int n;
    int main (){
        cin >> n;
        for(int i = 1; i <= n; i ++) cin >> w[i];
        f[0][0] = f[0][1] = -INF;
        f[0][2] = 0;
        for(int i = 1; i <= n; i ++){
            f[i][0] = max(f[i - 1][0], f[i - 1][2] - w[i]);
            f[i][1] = f[i - 1][0] + w[i];
            f[i][2] = max(f[i - 1][1], f[i - 1][2]);
        }
        cout << max(f[n][1], f[n][2]) << endl;
    		// 如果股票的价格是单调下降的，那么最好的策略是一次交易也不做
    		// 取两个出口的max
        return 0;
    }
    ```
    

## ****1052. 设计密码****

- 题目
- 解答
    
    AC自动机=Trie+KMP

    等待填充


# 状态压缩DP

## 1.棋盘（基于联通性）

## **1064. 小国王**

- 题目
    
    在 $n×n$ 的棋盘上放 $k$ 个国王，国王可攻击相邻的 8 个格子，求使它们无法互相攻击的方案总数。
    
    ### 输入格式
    
    共一行，包含两个整数 $n$ 和 $k$。
    
    ### 输出格式
    
    共一行，表示方案总数，若不能够放置则输出 0 。
    
    ### 数据范围
    
    $1 \\le n \\le 10$,
    $0 \\le k \\le n^2$
    
    ### 输入样例：
    
    ```
    3 2
    ```
    
    ### 输出样例：
    
    ```
    16
    ```
    
- 解答
    
    状态表示：f[i, j, s] 代表只摆在前i行，已经摆了j个国王，并且第i行的状态是s(二进制)的集合
    
    属性：Count
    
    状态计算：集合划分的过程👇
    
    第i-1行不能放在相邻的，两个格子 并且 第i-1行和第i行之间也不能互相攻击到
    
    a, b分别代表第i-1和第i行的状态的二进制表示
    
    则应该满足(a & b) == 0 并且 (a | b) 不能有相邻的1
    
    已经摆完前i行，第i行的状态为a，第i-1的状态为b，已经摆了j个国王的所有方案
    
    已经摆完前i-1行，第i-1行的状态为b，已经摆了j-count(a)个国王的所有方案
    
    DP问题的时间的复杂度：状态数量*状态计算的计算量
    
    状态数量：行数
    
    ```cpp
    #include<iostream>
    #include<vector>
    
    using namespace std;
    
    typedef long long ll;
    
    const int N = 12, M = 1 << 10, K = 110;
    
    int n, m;
    ll f[N][K][M];
    int cnt[M];
    vector<int> state;
    vector<int> head[M];
    
    bool check(int state){  // 检查此状态是否合法，状态中不应该出现两个1相邻的情况
        for(int i = 0; i < n; i ++)
            if((state >> i & 1) && (state >> i + 1 & 1))
                return false;
        return true;
    }
    
    int count(int state){  // 计算当前的状态中用掉了几个国王
        int res = 0;
        for(int i = 0; i < n; i ++) res += state >> i & 1;
        return res;
    }
    
    int main(){
        
        cin >> n >> m;
        
        for(int i = 0; i < 1 << n; i ++) // 初始化所有能满足没有两个1相邻的状态
            if(check(i)){
                state.push_back(i);
                cnt[i] = count(i);
            }
        
        for(int i = 0; i < state.size(); i ++) // 初始化 如果两个状态可以成为相邻的两行 则加入到head数组中
            for(int j = 0; j < state.size(); j ++){
                int a = state[i], b = state[j];
                if((a & b) == 0 && check(a | b))
                    head[i].push_back(j);
            }
            
        f[0][0][0] = 1;
        
        for(int i = 1; i <= n + 1; i ++) // 我们要求的最终状态是n+1的状态 这样可以减少一次循环
            for(int j = 0; j <= m; j ++)
                for(int a = 0; a < state.size(); a ++)
                    for(int b : head[a]){
                        int c = cnt[state[a]];
                        if(c <= j) // 如果当前国王的数量没有超过
                            f[i][j][a] += f[i - 1][j - c][b]; // 由b的状态转移过来
                    }
                    
        cout << f[n + 1][m][0] << endl;
        
        return 0;
    }
    ```
    

## **327. 玉米田**

- 题目
    
    农夫约翰的土地由 $M \\times N$ 个小方格组成，现在他要在土地里种植玉米。
    
    非常遗憾，部分土地是不育的，无法种植。
    
    而且，相邻的土地不能同时种植玉米，也就是说种植玉米的所有方格之间都不会有公共边缘。
    
    现在给定土地的大小，请你求出共有多少种种植方法。
    
    土地上什么都不种也算一种方法。
    
    ### 输入格式
    
    第 $1$ 行包含两个整数 $M$ 和 $N$。
    
    第 $2..M+1$ 行：每行包含 $N$ 个整数 $0$ 或 $1$，用来描述整个土地的状况，$1$ 表示该块土地肥沃，$0$ 表示该块土地不育。
    
    ### 输出格式
    
    输出总种植方法对 $10^8$ 取模后的值。
    
    ### 数据范围
    
    $1 \\le M,N \\le 12$
    
    ### 输入样例：
    
    ```
    2 3
    1 1 1
    0 1 0
    ```
    
    ### 输出样例：
    
    ```
    9
    ```
    
- 解答
    
    ```cpp
    #include<iostream>
    #include<vector>
    using namespace std;
    const int N = 15, M = 1 << 12, MOD = 1e8;
    int f[N][M], g[N];
    int n, m;
    vector<int> state;
    vector<int> head[M];
    bool check(int state){
        for(int i = 0; i < m; i ++)
            if((state >> i & 1) && (state >> i + 1 & 1))
                return false;
        return true;
    }
    int main(){
        cin >> n >> m;
        for(int i = 1; i <= n; i ++)
            for(int j = 0; j < m ; j ++){
                int t;
                cin >> t;
                g[i] += !t << j;
            }
        for(int i = 0; i < 1 << m; i ++)
            if(check(i))
                state.push_back(i);
        for(int i = 0; i < state.size(); i ++)
            for(int j = 0; j < state.size(); j ++){
                int a = state[i], b = state[j];
                if((a & b) == 0)
                    head[i].push_back(j);
            }
        f[0][0] = 1;
        for(int i = 1; i <= n + 1; i ++)
            for(int a = 0; a < state.size(); a ++){
                if(g[i] & state[a]) continue; //将植物种植在不育的土地上不符合要求
                for(int b : head[a]){ //找满足条件的第i-1个状态b
                    f[i][a] = (f[i][a] + f[i - 1][b]) % MOD;
                }
            }
        cout << f[n + 1][0] << endl;
        return 0;
    }
    ```
    

## ****292. 炮兵阵地****

- 题目
    
    司令部的将军们打算在 $N \\times M$ 的网格地图上部署他们的炮兵部队。
    
    一个 $N \\times M$ 的地图由 $N$ 行 $M$ 列组成，地图的每一格可能是山地（用 `H` 表示），也可能是平原（用 `P` 表示），如下图。
    
    在每一格平原地形上最多可以布置一支炮兵部队（山地上不能够部署炮兵部队）；一支炮兵部队在地图上的攻击范围如图中黑色区域所示：
    
    如果在地图中的灰色所标识的平原上部署一支炮兵部队，则图中的黑色的网格表示它能够攻击到的区域：沿横向左右各两格，沿纵向上下各两格。
    
    图上其它白色网格均攻击不到。
    
    从图上可见炮兵的攻击范围不受地形的影响。
    
    现在，将军们规划如何部署炮兵部队，在防止误伤的前提下（保证任何两支炮兵部队之间不能互相攻击，即任何一支炮兵部队都不在其他支炮兵部队的攻击范围内），在整个地图区域内最多能够摆放多少我军的炮兵部队。
    
    ![Untitled](img/yvmitian.png)
    
    ### 输入格式
    
    第一行包含两个由空格分割开的正整数，分别表示 $N$ 和 $M$；
    
    接下来的 $N$ 行，每一行含有连续的 $M$ 个字符(`P` 或者 `H`)，中间没有空格。按顺序表示地图中每一行的数据。
    
    ### 输出格式
    
    仅一行，包含一个整数 $K$，表示最多能摆放的炮兵部队的数量。
    
    ### 数据范围
    
    $N \\le 100,M \\le 10$
    
    ### 输入样例：
    
    ```
    5 4
    PHPP
    PPHH
    PPPP
    PHPP
    PHHP
    ```
    
    ### 输出样例：
    
    ```
    6
    ```
    
- 解答

## 2.集合

## **524. 愤怒的小鸟**

- 题目
    
    Kiana 最近沉迷于一款神奇的游戏无法自拔。
    
    简单来说，这款游戏是在一个平面上进行的。
    
    有一架弹弓位于 $(0,0)$ 处，每次 Kiana 可以用它向第一象限发射一只红色的小鸟， 小鸟们的飞行轨迹均为形如 $y=ax^2+bx$ 的曲线，其中 $a,b$ 是 Kiana 指定的参数，且必须满足 $a<0$。
    
    当小鸟落回地面（即 $x$ 轴）时，它就会瞬间消失。
    
    在游戏的某个关卡里，平面的第一象限中有 $n$ 只绿色的小猪，其中第 $i$ 只小猪所在的坐标为 $(x\_i,y\_i)$。
    
    如果某只小鸟的飞行轨迹经过了 $(x\_i, y\_i)$，那么第 $i$ 只小猪就会被消灭掉，同时小鸟将会沿着原先的轨迹继续飞行；
    
    如果一只小鸟的飞行轨迹没有经过 $(x\_i, y\_i)$，那么这只小鸟飞行的全过程就不会对第 $i$ 只小猪产生任何影响。
    
    例如，若两只小猪分别位于 $(1,3)$ 和 $(3,3)$，Kiana 可以选择发射一只飞行轨迹为 $y=−x^2+4x$ 的小鸟，这样两只小猪就会被这只小鸟一起消灭。
    
    而这个游戏的目的，就是通过发射小鸟消灭所有的小猪。
    
    这款神奇游戏的每个关卡对 Kiana 来说都很难，所以 Kiana 还输入了一些神秘的指令，使得自己能更轻松地完成这个这个游戏。
    
    这些指令将在输入格式中详述。
    
    假设这款游戏一共有 $T$ 个关卡，现在 Kiana 想知道，对于每一个关卡，至少需要发射多少只小鸟才能消灭所有的小猪。
    
    由于她不会算，所以希望由你告诉她。
    
    **注意**:本题除 NOIP 原数据外，还包含加强数据。
    
    ### 输入格式
    
    第一行包含一个正整数 $T$，表示游戏的关卡总数。
    
    下面依次输入这 $T$ 个关卡的信息。
    
    每个关卡第一行包含两个非负整数 $n,m$，分别表示该关卡中的小猪数量和 Kiana 输入的神秘指令类型。
    
    接下来的 $n$ 行中，第 $i$ 行包含两个正实数 $(x\_i,y\_i)$，表示第 $i$ 只小猪坐标为 $(x\_i,y\_i)$，数据保证同一个关卡中不存在两只坐标完全相同的小猪。
    
    如果 $m=0$，表示 Kiana 输入了一个没有任何作用的指令。
    
    如果 $m=1$，则这个关卡将会满足：至多用 $⌈n/3+1⌉$ 只小鸟即可消灭所有小猪。
    
    如果 $m=2$，则这个关卡将会满足：一定存在一种最优解，其中有一只小鸟消灭了至少 $⌊n/3⌋$ 只小猪。
    
    保证 $1 \\le n \\le 18，0 \\le m \\le 2，0<x\_i,y\_i<10$，输入中的实数均保留到小数点后两位。
    
    上文中，符号 $⌈c⌉$ 和 $⌊c⌋$ 分别表示对 $c$ 向上取整和向下取整，例如 ：$⌈2.1⌉=⌈2.9⌉=⌈3.0⌉=⌊3.0⌋=⌊3.1⌋=⌊3.9⌋=3$。
    
    ### 输出格式
    
    对每个关卡依次输出一行答案。
    
    输出的每一行包含一个正整数，表示相应的关卡中，消灭所有小猪最少需要的小鸟数量。
    
    ### 数据范围
    
    ![https://cdn.acwing.com/media/article/image/2021/03/11/19_f187a0f582-QQ%E6%88%AA%E5%9B%BE20210311115727.png](https://cdn.acwing.com/media/article/image/2021/03/11/19_f187a0f582-QQ%E6%88%AA%E5%9B%BE20210311115727.png)
    
    ### 输入样例：
    
    ```
    2
    2 0
    1.00 3.00
    3.00 3.00
    5 2
    1.00 5.00
    2.00 8.00
    3.00 9.00
    4.00 8.00
    5.00 5.00
    ```
    
    ### 输出样例：
    
    ```
    1
    1
    ```
    
- 解答
    
    抛物线，重复覆盖，状态压缩DP
    
    ```cpp
    #include<iostream>
    #include<algorithm>
    #include<cmath>
    #include<cstring>
    #define x first
    #define y second
    
    using namespace std;
    
    const int N = 18, M = 1 << 18;
    const double eps = 1e-6;
    typedef pair<double, double> PDD;
    
    int n, m;
    PDD q[N];
    int f[M];
    int path[N][N]; // 经过第i个点和第j个点的抛物线所能够覆盖的点集
    
    int cmp(double x, double y){
        if(fabs(x - y) < eps) return 0;
        if(x < y) return -1;
        return 1;
    }
    int main(){
        int T;
        cin >> T;
        while(T --){
            cin >> n >> m;
            for(int i = 0; i < n; i ++)
                cin >> q[i].x >> q[i].y;
            memset(path, 0, sizeof path);
            for(int i = 0; i < n; i ++){
                path[i][i] = 1 << i; // 可能存在只有一个点的情况，需要特判
                for(int j = 0; j < n; j ++){
                    double x1 = q[i].x, y1 = q[i].y;
                    double x2 = q[j].x, y2 = q[j].y;
                    if(!cmp(x1, x2)) continue;
                    double a = (y1 / x1 - y2 / x2) / (x1 - x2);
                    double b = y1 / x1 - a * x1;
                    
                    if(cmp(a, 0) >= 0) continue;
                    int state = 0;
                    for(int k = 0; k < n ; k ++){
                        double x = q[k].x, y = q[k].y;
                        if(!cmp(a * x * x + b * x, y)) state += 1 << k;
                    }
                    path[i][j] = state;
                }
            }
            memset(f, 0x3f, sizeof f);
            f[0] = 0;
            for(int i = 0; i + 1 < 1 << n; i ++){
                int x = 0;
                for(int j = 0; j < n; j ++)
                    if(!(i >> j & 1)){
                        x = j;
                        break;
                    }
                for(int j = 0; j < n; j ++)
                    f[i | path[x][j]] = min(f[i | path[x][j]], f[i] + 1);
            }
            cout << f[(1 << n) - 1] << endl;
        }
        return 0;
    }
    ```
    
- 优秀代码赏析
    
    ```cpp
    #include <iostream>
    #include <cstring>
    #include <cmath>
    
    #define x first
    #define y second
    
    using namespace std;
    
    typedef pair<double, double> PDD;
    
    const int N = 20, M = 1 << N;
    const double eps = 1e-8;
    
    int n, m;
    PDD ver[N];
    int path[N][N];
    int f[M];
    
    int cmp_lf(double a, double b)  //浮点数比较
    {
        if (fabs(a - b) < eps) return 0;
        if (a > b) return 1;
        return -1;
    }
    void init_path()    //预处理所有的抛物线
    {
        memset(path, 0, sizeof path);
        for (int i = 0; i < n; ++ i)
        {
            path[i][i] = 1 << i;    //单独生成一条只经过(0,0)和(x,y)的抛物线
            for (int j = 0; j < n; ++ j)
            {
                double x1 = ver[i].x, y1 = ver[i].y;
                double x2 = ver[j].x, y2 = ver[j].y;
                if (!cmp_lf(x1, x2)) continue;  //单独一个点无法生成参数a,b的抛物线
                double a = (y1 / x1 - y2 / x2) / (x1 - x2);
                double b = (y1 / x1) - a * x1;
                if (cmp_lf(a, 0.0) >= 0) continue;  //a < 0
                //参数a,b的抛物线已生成，枚举他经过的所有点
                for (int k = 0; k < n; ++ k)
                {
                    double x = ver[k].x, y = ver[k].y;
                    if (!cmp_lf(y, a * x * x + b * x)) path[i][j] += 1 << k;
                }
            }
        }
    }
    void solve()
    {
        //input
        cin >> n >> m;
        for (int i = 0; i < n; ++ i) cin >> ver[i].x >> ver[i].y;
        //prework
        init_path();
        //dp
        memset(f, 0x3f, sizeof f);
        f[0] = 0;
        for (int cur_st = 0; cur_st + 1 < 1 << n; ++ cur_st)
        {
            int t = -1;
            for (int i = 0; i < n; ++ i)
                if (!(cur_st >> i & 1))
                    t = i;
            for (int i = 0; i < n; ++ i)
            {
                int ne_st = path[t][i] | cur_st;
                f[ne_st] = min(f[ne_st], f[cur_st] + 1);
            }
        }
        cout << f[(1 << n) - 1] << endl;
    }
    int main()
    {
        int T = 1; cin >> T;
        while (T -- ) solve();
        return 0;
    }
    ```

## **91.最短Hamilton路径**

- 题目

    给定一张 $n$ 个点的带权无向图，点从 $0 \\sim n-1$ 标号，求起点 $0$ 到终点 $n-1$ 的最短 Hamilton 路径。

    Hamilton 路径的定义是从 $0$ 到 $n-1$ 不重不漏地经过每个点恰好一次。

    ### 输入格式

    第一行输入整数 $n$。

    接下来 $n$ 行每行 $n$ 个整数，其中第 $i$ 行第 $j$ 个整数表示点 $i$ 到 $j$ 的距离（记为 $a\[i,j\]$）。

    对于任意的 $x,y,z$，数据保证 $a\[x,x\]=0，a\[x,y\]=a\[y,x\]$ 并且 $a\[x,y\]+a\[y,z\] \\ge a\[x,z\]$。

    ### 输出格式

    输出一个整数，表示最短 Hamilton 路径的长度。

    ### 数据范围

    $1 \\le n \\le 20$  
    $0 \\le a\[i,j\] \\le 10^7$

    ### 输入样例：

    ```
    5
    0 2 4 5 1
    2 0 6 5 3
    4 6 0 8 3
    5 5 8 0 5
    1 3 3 5 0
    ```

    ### 输出样例：

    ```
    18
    ```

- 解答

    ``` cpp
    #include <iostream>
    #include <algorithm>
    #include <cstring>
    using namespace std;
    const int N = 20, M = 1 << 20;
    int n, f[M][N], w[N][N];  // f[i][j]的含义是走到了j，走过的点是i的路径

    int main() {
        cin >> n;
        
        for(int i = 0; i < n; i ++) {
            for(int j = 0; j < n; j ++) {
                cin >> w[i][j];
            }
        }
        memset(f, 0x3f, sizeof f);
        f[1][0] = 0;  // 从0开始走，走过了0号点
        
        for(int i = 0; i < 1 << n; i ++) {
            for(int j = 0; j < n; j ++) {
                if(i >> j & 1) {
                    for(int k = 0; k < n; k ++) { // 列举上一个状态的点
                        if(i - (1 << j) >> k & 1) {
                            f[i][j] = min(f[i][j], f[i - (1 << j)][k] + w[j][k]);
                        }
                    }
                }
            }
        }
        cout << f[(1 << n) - 1][n - 1] << endl;
            
        return 0;
    }
    ```


# 区间DP

### 基础：

## **282.** 石子合并

- 题目
    
    设有 N 堆石子排成一排，其编号为 1，2，3，…，N。
    
    每堆石子有一定的质量，可以用一个整数来描述，现在要将这 N 堆石子合并成为一堆。
    
    每次只能合并相邻的两堆，合并的代价为这两堆石子的质量之和，合并后与这两堆石子相邻的石子将和新堆相邻，合并时由于选择的顺序不同，合并的总代价也不相同。
    
    找出一种合理的方法，使总的代价最小，输出最小代价。
    
- 解答
    
    先循环区间长度 在循环左端点
    
    ```cpp
    #include<iostream>
    #include<cstring>
    using namespace std;
    const int N = 310;
    int f[N][N];
    int w[N], s[N];
    int main(){
        int n;
        cin >> n;
        for(int i = 1; i <= n; i ++){
            cin >> w[i];
            s[i] = s[i - 1] + w[i];
        }
        memset(f, 0x3f, sizeof f);
        for(int len = 1; len <= n; len ++)
            for(int l = 1; l + len - 1 <= n; l ++){
                int r = l + len - 1;
                if(len == 1){
                    f[l][r] = 0;
                    continue;
                }
                for(int k = l; k < r; k ++)
                    f[l][r] = min(f[l][r], f[l][k] + f[k + 1][r] + s[r] - s[l - 1]);
            }
        cout << f[1][n] << endl;
        return 0;
    }
    ```
    

### 常用技巧：

## 1. 将环形的区间变为一条链

将两段长为n的区间连在一起，按照朴素的方法进行预处理，然后枚举出长度为n的最小值

## **1068. 环形石子合并**

- 题目
    
    将 $n$ 堆石子绕圆形操场排放，现要将石子有序地合并成一堆。
    
    规定每次只能选相邻的两堆合并成新的一堆，并将新的一堆的石子数记做该次合并的得分。
    
    请编写一个程序，读入堆数 $n$ 及每堆的石子数，并进行如下计算：
    
    - 选择一种合并石子的方案，使得做 $n-1$ 次合并得分总和最大。
    - 选择一种合并石子的方案，使得做 $n-1$ 次合并得分总和最小。
    
    ### 输入格式
    
    第一行包含整数 $n$，表示共有 $n$ 堆石子。
    
    第二行包含 $n$ 个整数，分别表示每堆石子的数量。
    
    ### 输出格式
    
    输出共两行：
    
    第一行为合并得分总和最小值，
    
    第二行为合并得分总和最大值。
    
    ### 数据范围
    
    $1 \\le n \\le 200$
    
    ### 输入样例：
    
    ```
    4
    4 5 9 4
    ```
    
    ### 输出样例：
    
    ```
    43
    54
    ```
    
- 解答
    
    ```cpp
    #include<iostream>
    #include<algorithm>
    #include<cstring>
    using namespace std;
    const int N = 410, INF = 0x3f3f3f3f;
    int w[N], s[N];
    int f[N][N], g[N][N];
    int n;
    
    int main(){
        cin >> n;
        for(int i = 1; i <= n; i ++){
            cin >> w[i];
            w[i + n] = w[i];
        }
        for(int i = 1; i <= n * 2; i ++)
            s[i] = s[i - 1] + w[i];
        memset(f, INF, sizeof f);
        memset(g, - INF, sizeof g);
        for(int len = 1; len <= n; len ++)
            for(int l = 1; l + len - 1 <= n * 2; l ++){
                int r = l + len - 1;
                if(len == 1) f[l][r] = g[l][r] = 0;
                else
                    for(int k = l; k < r; k ++){ // k + 1 <= r
                        f[l][r] = min(f[l][r], f[l][k] + f[k + 1][r] + s[r] - s[l - 1]);
                        g[l][r] = max(g[l][r], g[l][k] + g[k + 1][r] + s[r] - s[l - 1]);
                    }
            }
            
        int maxv = -INF, minv = INF;
        for(int i = 1; i <= n; i ++){
            minv = min(minv, f[i][i + n - 1]);
            maxv = max(maxv, g[i][i + n - 1]);
        }
        cout << minv << endl << maxv << endl;
        return 0;
    }
    ```
    

## **320. 能量项链**

- 题目
    
    在 Mars 星球上，每个 Mars 人都随身佩带着一串能量项链，在项链上有 $N$ 颗能量珠。
    
    能量珠是一颗有头标记与尾标记的珠子，这些标记对应着某个正整数。
    
    并且，对于相邻的两颗珠子，前一颗珠子的尾标记一定等于后一颗珠子的头标记。
    
    因为只有这样，通过吸盘（吸盘是 Mars 人吸收能量的一种器官）的作用，这两颗珠子才能聚合成一颗珠子，同时释放出可以被吸盘吸收的能量。
    
    如果前一颗能量珠的头标记为 $m$，尾标记为 $r$，后一颗能量珠的头标记为 $r$，尾标记为 $n$，则聚合后释放的能量为 $m \\times r \\times n$（Mars 单位），新产生的珠子的头标记为 $m$，尾标记为 $n$。
    
    需要时，Mars 人就用吸盘夹住相邻的两颗珠子，通过聚合得到能量，直到项链上只剩下一颗珠子为止。
    
    显然，不同的聚合顺序得到的总能量是不同的，请你设计一个聚合顺序，使一串项链释放出的总能量最大。
    
    例如：设 $N=4$，$4$ 颗珠子的头标记与尾标记依次为 $(2，3) (3，5) (5，10) (10，2)$。
    
    我们用记号 $⊕$ 表示两颗珠子的聚合操作，$(j⊕k)$ 表示第 $j$，$k$ 两颗珠子聚合后所释放的能量。则
    
    第 $4、1$ 两颗珠子聚合后释放的能量为：$(4⊕1)=10 \\times 2 \\times 3=60$。
    
    这一串项链可以得到最优值的一个聚合顺序所释放的总能量为 $((4 \\oplus 1) \\oplus 2) \\oplus 3) = 10 \\times 2 \\times 3+10 \\times 3 \\times 5+10 \\times 5 \\times 10=710$。
    
    ### 输入格式
    
    输入的第一行是一个正整数 $N$，表示项链上珠子的个数。
    
    第二行是 $N$ 个用空格隔开的正整数，所有的数均不超过 $1000$，第 $i$ 个数为第 $i$ 颗珠子的头标记，当 $i<N$ 时，第 $i$ 颗珠子的尾标记应该等于第 $i+1$ 颗珠子的头标记，第 $N$ 颗珠子的尾标记应该等于第 $1$ 颗珠子的头标记。
    
    至于珠子的顺序，你可以这样确定：将项链放到桌面上，不要出现交叉，随意指定第一颗珠子，然后按顺时针方向确定其他珠子的顺序。
    
    ### 输出格式
    
    输出只有一行，是一个正整数 $E$，为一个最优聚合顺序所释放的总能量。
    
    ### 数据范围
    
    $4 \\le N \\le 100$,
    $1 \\le E \\le 2.1 \\times 10^9$
    
    ### 输入样例：
    
    ```
    4
    2 3 5 10
    ```
    
    ### 输出样例：
    
    ```
    710
    ```
    
- 解答
    
    状态表示：f[L, R] 是所有将L, R合并在一个矩阵的所有的方式。属性：Max
    
    状态计算：通过最后一个不同点将集合划分
    
    第k类的最大的值是f[l, k] + f[k, r] + w[l] * w[k] * w[r];
    
    求每一类的最大值
    
    ```cpp
    #include<iostream>
    #include<cstring>
    using namespace std;
    const int N = 110, M = N << 1;
    int f[M][M], w[M], n;
    int main(){
        cin >> n;
        for(int i = 1; i <= n; i ++){
            cin >> w[i];
            w[i + n] = w[i];
        }
        //memset(f, 0xcf, sizeof f); // 由于不存在负数的状态所以可以不初始化
        for(int len = 3; len <= n + 1; len ++)
            for(int l = 1; l + len - 1 <= n << 1; l ++){
                int r = l + len - 1;
                for(int k = l + 1; k < r; k ++)
                    f[l][r] = max(f[l][r], f[l][k] + f[k][r] + w[l] * w[k] * w[r]);
            }
        int res = 0;
        for(int i = 1; i <= n; i ++)
            res = max(res, f[i][i + n]);
        cout << res << endl;
        return 0;
    }
    ```
    
- 其他写法
    
    ```cpp
    #include <iostream>
    #include <cstring>
    using namespace std;
    const int N = 110, M = N << 1;
    int n, w[M], f[M][M];
    
    int main(){
        scanf("%d", &n);
        for (int i = 1; i <= n; ++ i) scanf("%d", &w[i]), w[n + i] = w[i];
        memset(f, -0x3f, sizeof f); //本题可以不用初始化，因为不存在负数的状态值，因此0就是最小的
        for (int len = 2; len <= n + 1; ++ len)
            for (int l = 1, r; (r = l + len - 1) <= n << 1; ++ l)
                if (len == 2) f[l][r] = 0; //初始化初始状态
                else
                    for (int k = l + 1; k < r; ++ k)
                        f[l][r] = max(f[l][r], f[l][k] + f[k][r] + w[l] * w[k] * w[r]);
        int res = 0;
        for (int l = 1; l <= n; ++ l) res = max(res, f[l][l + n]);
        printf("%d\n", res);
        return 0;
    }
    ```
    
- 其他解法
    
    ```cpp
    #include<iostream>
    #include<algorithm>
    using namespace std;
    const int N=210,INF=0x3f3f3f3f;
    int n;
    int a[N];
    int g[N][N];
    
    int main(){
        cin>>n;
        for(int i=1;i<=n;i++){
            cin>>a[i];
            a[i+n]=a[i];
        }
    
        for(int len=1;len<=n;len++)
            for(int i=1;i+len-1<=n*2;i++){
                int l=i,r=i+len-1;
                if(len==1)  g[l][r]=0;
                for(int k=l;k<r;k++)    g[l][r]=max(g[l][r],g[l][k]+g[k+1][r]+a[l]*a[k+1]*a[r+1]);
                //这里的过程同石子合并，这里不难想到若将l到k的珠子合并之后会变成一个首是l而尾k+1的珠子；
                //同理若将k+1到r的珠子合并之后会变成一个首是k+1而尾r+1的珠子；
            }
        int maxv=-INF;
        for(int i=1;i<=n;i++)
            maxv=max(g[i][i+n-1],maxv);//区间长度为n
        cout<< maxv <<endl;
        return 0;
    }
    ```
    

## 2. 区间DP记录方案数

## **479. 加分二叉树**

- 题目
    
    设一个 $n$ 个节点的二叉树 tree 的中序遍历为（$1,2,3,…,n$），其中数字 $1,2,3,…,n$ 为节点编号。
    
    每个节点都有一个分数（均为正整数），记第 $i$ 个节点的分数为 $d\_i$，tree 及它的每个子树都有一个加分，任一棵子树 subtree（也包含 tree 本身）的加分计算方法如下：
    
    subtree的左子树的加分 $×$ subtree的右子树的加分 $＋$ subtree的根的分数
    
    若某个子树为空，规定其加分为 $1$。
    
    叶子的加分就是叶节点本身的分数，不考虑它的空子树。
    
    试求一棵符合中序遍历为（$1,2,3,…,n$）且加分最高的二叉树 tree。
    
    要求输出：
    
    （1）tree的最高加分
    
    （2）tree的前序遍历
    
    ### 输入格式
    
    第 $1$ 行：一个整数 $n$，为节点个数。
    
    第 $2$ 行：$n$ 个用空格隔开的整数，为每个节点的分数（$0<$分数$<100$）。
    
    ### 输出格式
    
    第 $1$ 行：一个整数，为最高加分（结果不会超过`int`范围）。
    
    第 $2$ 行：$n$ 个用空格隔开的整数，为该树的前序遍历。如果存在多种方案，则输出字典序最小的方案。
    
    ### 数据范围
    
    $n < 30$
    
    ### 输入样例：
    
    ```
    5
    5 7 1 2 10
    ```
    
    ### 输出样例：
    
    ```
    145
    3 1 2 4 5
    ```
    
- 解答
    - 区间DP
        
        ```cpp
        #include<iostream>
        #include<cstring>
        using namespace std;
        const int N = 35;
        int f[N][N]; // 表示区间的最高的加分
        int g[N][N]; // 保存l, r区间的根节点的序号
        int w[N], n;
        
        void dfs(int l, int r){ // 递归打印树的前序遍历
            if(l > r) return ;
            int k = g[l][r];
            cout << k << " ";
            dfs(l, k - 1);
            dfs(k + 1, r);
        }
        
        int main(){
            cin >> n;
            for(int i = 1; i <= n; i ++) cin >> w[i];
            // 保存中序遍历为1,2,3...n的根节点的值
            for(int len = 1; len <= n; len ++)
                for(int l = 1; l + len - 1 <= n; l ++){
                    int r = l + len - 1;
                    if(len == 1){ // 初始化，此时r=l
                        f[l][r] = w[l];
                        g[l][r] = l;
                        continue;
                    }
                    for(int k = l; k <= r; k ++){
        								// 如果没有左(右)节点, 将其加分设置为1
                        int left = k == l ? 1 : f[l][k - 1];
                        int right = k == r ? 1 : f[k + 1][r];
                        int score = left * right + w[k];
                        if(f[l][r] < score){
        										// 求字典序最小的一个答案
        										//可知其根节点越靠前 字典序越小
                            f[l][r] = score;
                            g[l][r] = k;
                        }
                    }
                }
            cout << f[1][n] << endl;
            dfs(1, n);
            return 0;
        }
        ```
        
    - 记忆化搜索

        等待填充

## 3. 区间DP+高精度

## **1069. 凸多边形的划分**

- 题目
    
    给定一个具有 $N$ 个顶点的凸多边形，将顶点从 $1$ 至 $N$ 标号，每个顶点的权值都是一个正整数。
    
    将这个凸多边形划分成 $N-2$ 个互不相交的三角形，对于每个三角形，其三个顶点的权值相乘都可得到一个权值乘积，试求所有三角形的顶点权值乘积之和至少为多少。
    
    ### 输入格式
    
    第一行包含整数 $N$，表示顶点数量。
    
    第二行包含 $N$ 个整数，依次为顶点 $1$ 至顶点 $N$ 的权值。
    
    ### 输出格式
    
    输出仅一行，为所有三角形的顶点权值乘积之和的最小值。
    
    ### 数据范围
    
    $N \\le 50$,
    数据保证所有顶点的权值都小于$10^9$
    
    ### 输入样例：
    
    ```
    5
    121 122 123 245 231
    ```
    
    ### 输出样例：
    
    ```
    12214884
    ```
    
- 解答
    - 不带有高精度的写法
        
        ```cpp
        #include<iostream>
        #include<cstring>
        using namespace std;
        const int N = 55, M = 35, INF = 1e9;
        int n;
        int w[N];
        int f[N][N]; // 当前划分到的多边形的左端点是 l，右端点是 r 的方案
        
        int main (){
            cin >> n;
            for(int i = 1; i <= n; i ++) cin >> w[i];
            memset(f, 0x3f, sizeof f);
            for(int len = 2; len <= n; len ++)
                for(int l = 1; l + len - 1 <= n; l ++){
                    int r = l + len - 1;
                    if(len == 2) f[l][r] = 0; // 这是一条边的情况 初始化f[l][l+1]为0
                    for(int k = l + 1; k < r; k ++)
                        f[l][r] = min(f[l][r], f[l][k] + f[k][r] + w[l] * w[k] * w[r]);      
                }
            cout << f[1][n] << endl;
            return 0;
        }
        ```
        
        ```cpp
        #include<iostream>
        #include<cstring>
        using namespace std;
        const int N = 55, M = 35, INF = 1e9;
        int n;
        int w[N];
        int f[N][N];
        
        int main (){
            cin >> n;
            for(int i = 1; i <= n; i ++) cin >> w[i];
        
            for(int len = 3; len <= n; len ++)
                for(int l = 1; l + len - 1 <= n; l ++){
                    int r = l + len - 1;
                    f[l][r] = INF;
                    for(int k = l + 1; k < r; k ++)
                        f[l][r] = min(f[l][r], f[l][k] + f[k][r] + w[l] * w[k] * w[r]);      
                }
            cout << f[1][n] << endl;
            return 0;
        }
        ```
        
    - 高精度的写法
        
        ```cpp
        #include <cstring>
        #include <iostream>
        #include <algorithm>
        using namespace std;
        typedef long long LL;
        const int N = 55, M = 35, INF = 1e9;
        int n;
        int w[N];
        LL f[N][N][M];
        
        void add(LL a[], LL b[])
        {
            static LL c[M];
            memset(c, 0, sizeof c);
            for (int i = 0, t = 0; i < M; i ++ )
            {
                t += a[i] + b[i];
                c[i] = t % 10;
                t /= 10;
            }
            memcpy(a, c, sizeof c);
        }
        void mul(LL a[], LL b)
        {
            static LL c[M];
            memset(c, 0, sizeof c);
            LL t = 0;
            for (int i = 0; i < M; i ++ )
            {
                t += a[i] * b;
                c[i] = t % 10;
                t /= 10;
            }
            memcpy(a, c, sizeof c);
        }
        
        int cmp(LL a[], LL b[])
        {
            for (int i = M - 1; i >= 0; i -- )
                if (a[i] > b[i]) return 1;
                else if (a[i] < b[i]) return -1;
            return 0;
        }
        
        void print(LL a[])
        {
            int k = M - 1;
            while (k && !a[k]) k -- ;
            while (k >= 0) cout << a[k -- ];
            cout << endl;
        }
        
        int main()
        {
            cin >> n;
            for (int i = 1; i <= n; i ++ ) cin >> w[i];
        
            LL temp[M];
            for (int len = 3; len <= n; len ++ )
                for (int l = 1; l + len - 1 <= n; l ++ )
                {
                    int r = l + len - 1;
                    f[l][r][M - 1] = 1;
                    for (int k = l + 1; k < r; k ++ )
                    {
                        memset(temp, 0, sizeof temp);
                        temp[0] = w[l];
                        mul(temp, w[k]);
                        mul(temp, w[r]);
                        add(temp, f[l][k]);
                        add(temp, f[k][r]);
                        if (cmp(f[l][r], temp) > 0)
                            memcpy(f[l][r], temp, sizeof temp);
                    }
                }
        
            print(f[1][n]);
        
            return 0;
        }
        ```
        

## 4. 二维区间DP

    等待填充


# 数位DP

## 技巧1：[X, Y] ⇒ f(Y) - f(X-1)

## 技巧2：树的角度来考虑

## **1081. 度的数量**

- 题目
    
    求给定区间 [X,Y] 中满足下列条件的整数个数：这个数恰好等于 K 个互不相等的 B 的整数次幂之和。
    
    例如，设 X = 15, Y = 20, K = 2, B = 2，则有且仅有下列三个数满足题意：
    
    17 = 2^4 + 2^0
    18 = 2^4 + 2^1
    20 = 2^4 + 2^2
    
    ### 输入格式
    
    第一行包含两个整数 X 和 Y，接下来两行包含整数 K 和 B。
    
    ### 输出格式
    
    只包含一个整数，表示满足条件的数的个数。
    
    ### 数据范围
    
    1 ≤ X ≤ Y ≤ 2^{31}-1,
    1 ≤ K ≤ 20,
    2 ≤ B ≤ 10
    
    ### 输入样例：
    
    ```
    15 20
    2
    2
    ```
    
    ### 输出样例：
    
    ```
    3
    ```
    
- [解答](https://www.acwing.com/solution/content/34003/)
    
    ```cpp
    #include <iostream>
    #include <cstring>
    #include <algorithm>
    #include <vector>
    using namespace std;
    const int N = 35;
    int f[N][N];
    int X, Y, K, B;
    void init(){
        for(int i = 0; i < N; i ++)
            for(int j = 0; j <= i; j ++)
                if(!j) f[i][j] = 1;
                else f[i][j] = f[i - 1][j] + f[i - 1][j - 1];
    }
    int cnt(int n){
        if(n == 0) return 0;
        vector<int> nums;
        while(n) nums.push_back(n % B), n /= B;
        int res = 0;
        int last = 0;
        for(int i = nums.size() - 1; i >= 0; i --){
            int x = nums[i];
            if(x > 0){
                res += f[i][K - last];
                if(x > 1){
                    if(K - last - 1 >= 0) res += f[i][K - last - 1];
                    break;
                }
                else{
                    last ++;
                    if(last > K) break;
                }
            }
            if(!i && last == K) res ++;
        }
        return res;
    }
    int main(){
        init();
        cin >> X >> Y >> K >> B;
        cout << cnt(Y) - cnt(X - 1) << endl;
        return 0;
    }
    ```
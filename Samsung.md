### Question 1.

Mr. Kim has to deliver refrigerators to N customers. From the office, he is going to visit all the customers and then return to his home. Each location of the office, his home, and the customers is given in the form of integer coordinates (x,y) (0≤x≤100, 0≤y≤100) . The distance between two arbitrary locations (x1, y1) and (x2, y2) is computed by |x1-x2| + |y1-y2|, where |x| denotes the absolute value of x; for instance, |3|=|-3|=3. The locations of the office, his home, and the customers are all distinct. You should plan an optimal way to visit all the N customers and return to his among all the possibilities.

You are given the locations of the office, Mr. Kim’s home, and the customers; the number of the customers is in the range of 5 to 10. Write a program that, starting at the office, finds a (the) shortest path visiting all the customers and returning to his home. Your program only have to report the distance of a (the) shortest path.

Constraints

5≤N≤10. Each location (x,y) is in a bounded grid, 0≤x≤100, 0≤y≤100, and x, y are integers.


Input:

You are given 10 test cases. Each test case consists of two lines; the first line has N, the number of the customers, and the following line enumerates the locations of the office, Mr. Kim’s home, and the customers in sequence. Each location consists of the coordinates (x,y), which is represented by ‘x y’.


Output:

Output the 10 answers in 10 lines. Each line outputs the distance of a (the) shortest path. Each line looks like ‘#x answer’ where x is the index of a test case. ‘#x’ and ‘answer’ are separated by a space.


I/O Example ::::   Input (20 lines in total. In the first test case, the locations of the office and the home are (0, 0) and (100, 100) respectively, and the locations of the customers are (70, 40), (30, 10), (10, 5), (90, 70), (50, 20).)

5  Starting test case #1

0 0 100 100 70 40 30 10 10 5 90 70 50 20

6  Starting test case #2

88 81 85 80 19 22 31 15 27 29 30 10 20 26 5 14

10  Starting test case #3

39 9 97 61 35 93 62 64 96 39 36 36 9 59 59 96 61 7 64 43 43 58 1 36

Output (10 lines in total)

#1 200

#2 304

#3 366
 

```
#include <bits/stdc++.h>

using namespace std;

int dp[13][1 << 12];
int x[13], y[13];

int n;

int calc(int p, int mask) {
    if (p == 0) return (mask != 0) * 1e9;
    int &ret = dp[p][mask];
    if (ret != -1) return ret;
    ret = 1e9;
    for (int i = 0; i <= n; ++i) {
        if (mask & (1 << i)) {
            int dist = abs(x[p] - x[i]) + abs(y[p] - y[i]);
            ret = min(ret, calc(i, mask ^ (1 << i)) + dist);
        }
    }
    return ret;
}

int main() {
    for (int i = 0; i < 10; ++i) {
        scanf("%d", &n);
        scanf("%d %d", &x[n + 1], &y[n + 1]);
        scanf("%d %d", &x[0], &y[0]);
        for (int j = 1; j <= n; ++j) {
            scanf("%d %d", &x[j], &y[j]);
        }
        memset(dp, -1, sizeof dp);
        printf("#%d %d\n", i + 1, calc(n + 1, (1 << (n + 1)) - 1));
    }
    return 0;
}
```

### Question 2.

Samsung Delhi IITD 2018

Initially you have H amount of energy and D distance to travel. You may travel
with any of the given 5 speeds. But you may only travel in units of 1 km. For 
each km distance traveled, you will spend corresponding amount of energy.
e.g. the given speeds are:

Cost of traveling 1 km: [4, 5, 2, 3, 6]
Time taken to travel 1 km: [200, 210, 230, 235, 215]

Find minimum time required to cover total D km with remaining H >= 0
1 <= H <= 4000
1 <= D <= 1000

```
#include <bits/stdc++.h>
using namespace std;
const int INF = 2E9;

int dp[4040][1010][5];
inline int fun(int i, int j, int k, vector<int> &a, vector<int> &b) {
    if (i < 0) return INF;
    if (j == 0) return 0;
    if (k < 0) return INF;
    if (dp[i][j][k] != -1) return dp[i][j][k];
    return dp[i][j][k] = min(fun(i, j, k - 1, a, b), b[k] + fun(i - a[k], j - 1, k, a, b));
}

int getMinTime(vector<int> &cost, vector<int> &time, int H, int D) {
    memset(dp, -1, sizeof dp);
    return fun(H, D, 4, cost, time);
}

int main() {
    int t (14);
    vector<int> cost {4, 5, 2, 3, 6};
    vector<int> time {200, 210, 230, 235, 215};
    cout << getMinTime(cost, time, t, 4);
    return 0;
}

```

/* Verify for the following t values..
 * 
 * t = 16, 17, … -> 800
 * t = 14, 15 -> 830
 * t = 13 -> 860
 */

### Question 3.

There is one spaceship. X and Y co-ordinate of source of spaceship and destination spaceship is given. There are N (0<=N<=5) number of wormholes each wormhole has 5 values. First 2 values are starting co-ordinate of wormhole and after that value no. 3 and 4 represents ending co-ordinate of wormhole and last 5th value is represents cost to pass through this wormhole. Now these wormholes are bi-directional. Now to go from (x1,y1) to (x2,y2) is abs(x1-x2)+abs(y1-y2). The main problem here is to find minimum distance to reach spaceship from source to destination co-ordinate using any number of wormhole. It is ok if you won’t use any wormhole.

```
#include<bits/stdc++.h>
using namespace std;
vector<pair<int,int>>entry;
vector<pair<int,int>>exit1;
vector<int>cost;
int main()
{
    int n;
    cin>>n;

    int x1,y1,x2,y2,d;

    for(int i=0;i<n;i++)
    {
         cin>>x1>>y1>>x2>>y2>>d;
         entry.push_back({x1,y1});
         exit1.push_back({x2,y2});
         cost.push_back(d);
    }


    int sx=-1,sy=-1,ex=-1,ey=-1;

    cin>>sx>>sy>>ex>>ey;

    entry.push_back({sx,sy});
    entry.push_back({ex,ey});
    exit1.push_back({sx,sy});
    exit1.push_back({ex,ey});
    cost.push_back(0);
    cost.push_back(0);
    int dis[n+2][n+2];
    memset(dis,0,sizeof(dis));

    for(int i=0;i<n+2;i++)
    {
        for(int j=0;j<n+2;j++)
        {
            if(i==j)
                continue;

             int d1=abs(exit1[i].first-entry[j].first)+abs(exit1[i].second-entry[j].second)+cost[i];
             int d2=abs(exit1[i].first-exit1[j].first)+abs(exit1[i].second-exit1[j].second)+cost[i];
             d1=min(d1,d2);
                dis[i][j]=d;
        }
    }
     for(int i=0;i<n+2;i++)
    {
        for(int j=0;j<n+2;j++)
        {
            cout<<dis[i][j]<<"  ";
        }
        cout<<endl;
    }
    for(int k=0;k<n+2;k++)
    {
        for(int i=0;i<n+2;i++)
        {
            for(int j=0;j<n+2;j++)
            {
                 if(dis[i][k]+dis[k][j]<dis[i][j])
                 {
                     dis[i][j]=dis[i][k]+dis[k][j];
                 }
            }
        }
    }
 for(int i=0;i<n+2;i++)
    {
        for(int j=0;j<n+2;j++)
        {
            cout<<dis[i][j]<<"  ";
        }
        cout<<endl;
    }
    cout<<dis[n][n+1];
    return 0;
}
```

### Question 4.

Given an integer k and a monotonic increasing sequence:
f(n) = an + bn [log2(n)] + cn^3 where (a = 1, 2, 3, …), (b = 1, 2, 3, …), (c = 0, 1, 2, 3, …)

Here, [log2(n)] means, taking the log to the base 2 and round the value down Thus,
if n = 1, the value is 0.
if n = 2-3, the value is 1.
if n = 4-7, the value is 2.
if n = 8-15, the value is 3.

The task is to find the value n such that f(n) = k, if k doesn’t belong to the sequence then print 0.
Note: Values are in such a way that they can be expressed in 64 bits and the three integers a, b and c do not exceed 100.

```
#include <iostream> 
#include <math.h> 
#define SMALL_N 1000000 
#define LARGE_N  1000000000000000 
using namespace std; 
  
// Function to return the value of f(n) for given values of a, b, c, n 
long long func(long long a, long long b, long long c, long long n) 
{ 
    long long res = a * n; 
    long long logVlaue = floor(log2(n)); 
    res += b * n * logVlaue; 
    res += c * (n * n * n); 
    return res; 
} 
  
long long getPositionInSeries(long long a, long long b,  
                             long long c, long long k) 
{ 
    long long start = 1, end = SMALL_N; 
  
    // if c is 0, then value of n can be in order of 10^15. 
    // if c!=0, then n^3 value has to be in order of 10^18 
    // so maximum value of n can be 10^6. 
    if (c == 0) { 
        end = LARGE_N; 
    } 
    long long ans = 0; 
  
    // for efficient searching, use binary search. 
    while (start <= end) { 
        long long mid = (start + end) / 2; 
        long long val = func(a, b, c, mid); 
        if (val == k) { 
            ans = mid; 
            break; 
        } 
        else if (val > k) { 
            end = mid - 1; 
        } 
        else { 
            start = mid + 1; 
        } 
    } 
    return ans; 
} 
  
// Driver code 
int main() 
{ 
    long long a = 2, b = 1, c = 1; 
    long long k = 12168587437017; 
  
    cout << getPositionInSeries(a, b, c, k); 
  
    return 0; 
} 
```

### Question 5

A Doctor travels from a division to other division where divisions are connected like a graph(directed graph) and the edge weights are the probabilities of the doctor going from that division to other connected division but the doctor stays 10mins at each division now there will be given time and had to find the division in which he will be staying by that time and is determined by finding division which has high probability.

Input is number of test cases followed by the number of nodes, edges, time after which we need to find the division in which he will be there, the edges starting point, end point, probability.

Note: If he reaches a point where there are no further nodes then he leaves the lab after 10 mins and the traveling time is not considered and during that 10 min at 10th min he will be in next division, so be careful.

```
#include<iostream>
using namespace std;

void findans(double **graph, int nodes, int time, int start, double p, double answer[])
{
    
    if(time <= 0)
    {
        answer[start] += p;
        return;
    }

    for(int i=1;i<=nodes;i++)
        if(graph[start][i] != 0)
        {
            p *= graph[start][i];
            findans(graph, nodes, time-10 , i , p ,answer);
            p /= graph[start][i];
        }
}

int main()
{
    int t;
    cin>>t;
    while(t--)
    {
        int nodes,edges,time;
        cin>>nodes>>edges>>time;
        double **graph = new double*[nodes];
        for(int i=1;i<=nodes;i++)
        {
            graph[i] = new double[nodes];
            for(int j=1;j<=nodes;j++)
                graph[i][j] = 0;
        }

        for(int i=0;i<edges;i++)
        {   
            int start, end;
            double prob;
            cin>>start>>end>>prob;
            graph[start][end] = prob;
        }
        double answer[nodes] = {0.0};
        findans(graph, nodes, time, 1, 1.0, answer);
        double final_prob = -1; int final_div = 0;
        for(int i=1;i<=nodes;i++)
        {
            if(answer[i]>final_prob)
            {
                final_prob = answer[i];
                final_div = i;
            }
        }
        cout<<final_div<<" "<<final_prob<<endl;
    }
    return 0;
}
```

### Question 6

FIND THE DIFFICULTY OF THE CLIMB.
Given a grid( nXm matrix) containing values 0,1,2 and 3.
0 represents no path.
1 represent an existing path.
2 is the starting point.
3 is the destination.
Starting point is always at left bottom matrix[n,1].
Destination can be anywhere in the matrix. It is assured that a path exist.
A rockclimber can move right or left if the adjacent element is also 1.
The rockclimber however climb up or down skip any number of rows the more rows he skip the greater will the difficulty.


```
#include<iostream>
using namespace std;
void printMat(int a[1000][1000],int n,int m){
  int i,j;
  cout<<"\n\n";
  for(i=1;i<=n;i++){
    for(j=1;j<=m;j++){
      cout<<a[i][j]<<" ";
    }
    cout<<endl;
  }
}
int traverse(int a[1000][1000],int n,int m,int i,int j,int h){
  int ans,k;
  printMat(a,n,m);
  //cout<<i<<" "<<j;
  if(i<=0 or j<=0 or i>n or j>m)
    return -1;
  if(a[i][j]==1 or a[i][j]==3){
    //for avoiding cycles during traversal,
    //equivalent to marking as visited
    a[i][j]*=-1;

    //moving left in matrix
    cout<<"left";
    ans=traverse(a,n,m,i,j-1,h);
    if(ans>=1){
        return ans;
    }

    //moving down in matrix
    cout<<"down";
    for(k=1;k<=h;k++){
      ans=traverse(a,n,m,i+k,j,h);
      if(ans>=1){
        return ans;
      }
    }

    //moving right in matrix
    cout<<"right";
    ans=traverse(a,n,m,i,j+1,h);
    if(ans>=1){
        return ans;
    }

    //moving up in matrix
    cout<<"up";
    for(k=1;k<=h;k++){
      ans=traverse(a,n,m,i-k,j,h);
      if(ans>=1){
        return ans;
      }
    }
    // restoring initial value after backtracking
    a[i][j]*=-1;
    return ans;
  }
  else if (a[i][j]<=0){
    return -1;
  }
  else if(a[i][j]==2)
    return h;
  else
    return -1;
}
int main(){
  
  //di,dj is the destination indices
  //h is the height for the jump or the difficulty
  int i,j,di,dj,t,T,test_cases,n,m,h,ans=-1;
  int a[1000][1000];
  cout<<"Enter no of testcases \n";
  cin>>T;
  for(t=1;t<=T;t++){
    cout<<" Enter dimensions  \n";
    cin>>n>>m;
    h=n-1;
    cout<<" Enter Elements \n";
    for(i=1;i<=n;i++)
      for(j=1;j<=m;j++){
        cin>>a[i][j];
        if(a[i][j]==3){
          di=i;
          dj=j;
        }
      }
    for(i=1;i<=h;i++){
        ans=traverse(a,n,m,di,dj,i);
        if(ans>=1)
          break;
    }
    cout<<"#"<<t<<" "<<ans<<endl;
  }
  return 0;
}
```



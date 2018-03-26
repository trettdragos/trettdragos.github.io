Roy-Floyd
----------------
```
#define INF 100000
int n,c[101][101],d[101][101];
void citire()
{
ifstream f("date.in");
f>>n;
int x,y,cost;
for(int i=1;i<=n;i++)
   for(int j=1;j<=n;j++)
     c[i][j]=INF;
for(int i=1;i<=n;i++)
   c[i][i]=0;
while(f>>x>>y>>cost)
   {
    c[x][y]=cost;
    d[x][y]=x;
   }
}
void Roy_Floyd()
{
for(int k=1;k<=n;k++)
 for(int i=1;i<=n;i++)
   for(int j=1;j<=n;j++)
     if(c[i][j]>c[i][k]+c[k][j])
       {
        c[i][j]=c[i][k]+c[k][j];
        d[i][j]=d[k][j];
       }
}
void drum(int i,int j)
{
if(i!=j)
   drum(i,d[i][j]);
cout<<j<<" ";
}
void afisare()
{
for(int i=1;i<=n;i++)
  for(int j=1;j<=n;j++)
     if(i!=j)
}
```


###Exeplu, am prins numai finalul sorry boss
k= 4
C:

|-|1|2|3|4|5|
|-|-|-|-|-|-|
|1|0|5|3|2|7|
|2|-|0|5|7|2|
|3|-|3|0|2|7|
|4|-|3|1|0|5|
|5|-|7|2|4|0|

D:

|-|1|2|3|4|5|
|-|-|-|-|-|-|
|1|0|4|4|1|2|
|2|0|0|2|3|2|
|3|0|4|0|3|2|
|4|0|4|4|0|2|
|5|0|4|3|3|0|
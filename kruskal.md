Kruskal
---------

```
struct muchie
{int x,y,cost;
}u[1000];
int n,m,c[101];
void citire()
{
ifstream f("date.in");
f>>n>>m;
for(int i=0;i<m;i++)
   f>>u[i].x>>u[i].y>>u[i].cost;
void sortare_muchii()
{
for(int i=0;i<m-1;i++)
  for(int j=i+1;j<m;j++)
   if(u[i].cost>u[j].cost)
    {
    muchie aux;
    aux=u[i];
    u[i]=u[j];
    u[j]=aux;
    }
}
void kruskal()
{
for(int i=1;i<=n;i++)
   c[i]=i;
sortare_muchii();
int cost = 0, nr=0, i=0;
while(nr<n-1){
  if(c[u[i].x]!=c[u[i].y])
    {
     cout<<u[i]x<<" "<<u[i].y<<"\n";
     nr++;
     int a=c[u[i].x];
     int b = c[u[i].y];
     for(int j=1; j<=n; j++)
     	if(x[j]==b)
     		c[j]=a;
     cost+=u[i].cost;
    }
    i++;
}
cout<<cost;
}

```
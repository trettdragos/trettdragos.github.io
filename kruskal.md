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
int NrSel=0,costA=0,i=0;
while(NrSel<n-1)
  if(c[u[i].x]!=c[u[i].y])
    {
     cout<<u[i]x<<" "<<u[i].y<<"\n";
     NrSel++;
    }
}

```
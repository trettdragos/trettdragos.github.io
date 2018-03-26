Roy-Floyd
----------------
```
#include <iostream>
#include <fstream>
#define INF 10000000

using namespace std;

int cm[101], c[101][101], t[101],n,start;

void citire()
{
    ifstream f("date.in");
    f>>n>>start;
    for(int i=1; i<=n; i++)
        for(int j=1; j<=n; j++)  ///punem infinit in toata matricea c
            c[i][j]=INF;
    for(int i=1; i<=n; i++)
        c[i][i]=0;               ///punem 0 pe diagonala principala
    int x,y,cost;
    while(f>>x>>y>>cost)
    {
        c[x][y]=cost;
        d[x][y]=x;              /// marcam costul muchiei [x,y] in c
                                ///x in d[x][y]
    }
}

void Roy-Floyd()
{
    for(int k=1; k<=n; k++)
        for(int i=1; i<=n; i++)
            for(int j=1; j<=n; j++)
                if(c[i][j]>c[i][k]+c[k][j])
                    {
                        c[i][j]=c[i][k]+c[k][j];
                        d[i][j]=d[k][j];
                    }
}

void drum(int i, int j)
{
    if(i!=j)
        drum(i,d[i][j]);
    cout<<j<<" ";
}

void afisare()
{
    for(int i=1; i<=n; i++)
        for(int j=1; j<=n; j++)
            if(i!=j)
    {
        cout<<"Drumul minim de la " << i<< " la "<<j<<"are costul: ";
        if(c[i][j]==INF)
            cout<<"inexistent \n";
        else
        {
            cout<<c[i][j]<<". Drumul: ";
            drum(i,j);
            cout<<"\n";
        }
    }
}
int main()
{
    citire();
    Roy-Floyd();
    afisare();
    return 0;
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
Algoritmul lui Prim
---------------------

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
        for(int j=1; j<=n; j++)
            c[i][j]=INF;
    for(int i=1; i<=n; i++)
        c[i][i]=0;
    int x,y,cost;
    while(f>>x>>y>>cost)
        c[x][y]=c[y][x]=cost;
}

void Prim()
{
    for(int i=1; i<n; i++)
    {
        cm[i]=c[start][i];
        t[i]=start;
    }
    viz[start]=1;
    t[start]=0;
    nrvfsel=1;
    while(nrvfsel<n)
    {
        int mini=INF,vf;
        for(int i=1; i<n; i++)
            if(cm[i]<mini && viz[i]==0)
        {
            mini=cm[i];
            vf=i;
        }
        viz[vf]=1;
        for(int i=1; i<n; i++)
            if(viz[i]==0 && cm[i]>c[vf][i])
        {
            cm[i]=c[vf][i];
            t[i]=vf;
        }
        nrvfsel++

    }
}

void afisare()
{
    int costAPM=0;
    for(int i=1; i<=n; i++)
        if(i!=start)
            {
                cout<<i<<" "<<t[i]<<"\n";
                costAPM+=c[i][t[i]];
            }
            cout<<"Costul APM va fi: "<<costAPM;
}
int main()
{
    citire();
    Prim();
    afisare();
    return 0;
}
```


Determina Apm al unui graf conex n vf si definitia fucntia const.

###Algoritm:

* se pleaca de la un vf de start
* initial in arbore avem vf de start
* la fiecare pas selectam o muchie de cost minim care are o extremitate in arbore, iar cealalta extremitate in afara arborelui
* Algoritmul se termina cand arborele are n varfuri

###Structuri de date utilizate

* se pleaca de la matricea costurilor: ```Cnxn, c[i][j]={0, daca i=j; costul([i, j]) daca exista muchie i, j; infinit in alte cazuri;} ```
* un sir ```viz[n]={1, daca iapartine arborelui; 0 in caz contrar}```
* Cmin cu n eleme
	```Cmin[i]=min{cost([i, j])}```
* T cu n elem unde
	```T[i]=```tatal vf;(din ce vf fost atins)

##Code
Initial:

| vf   | 1 | 2 | 3   | 4   | 5   | 6 | 7 |
|------|---|---|-----|-----|-----|---|---|
| viz  | 1 | 0 | 0   | 0   | 0   | 0 | 0 |
| Cmin | 0 | 2 | inf | inf | inf | 5 | 7 |
| T    | 0 | 1 | 0   | 0   | 0   | 1 | 1 |

start = 1;

1) Alegem minimul din Cmin pt vf neselectate

|vf  |1|2|3|4|5|6|7|
|----|-|-|-|-|-|-|-|
|viz |1|1|0|0|0|0|0|
|Cmin|0|2|1|i|3|2|7|
|T   |0|1|2|0|2|2|1|

2) Selectam vf 3

|vf  |1|2|3|4|5|6|7|
|----|-|-|-|-|-|-|-|
|viz |1|1|1|0|0|0|0|
|Cmin|0|2|1|1|2|2|7|
|T   |0|1|2|3|3|2|1|

3) Selectam vf 4

|vf  |1|2|3|4|5|6|7|
|----|-|-|-|-|-|-|-|
|viz |1|1|1|1|0|0|0|
|Cmin|0|2|1|1|1|2|7|
|T   |0|1|2|3|4|2|1|

4) Selectam vf 5

|vf  |1|2|3|4|5|6|7|
|----|-|-|-|-|-|-|-|
|viz |1|1|1|1|1|0|0|
|Cmin|0|2|1|1|1|2|7|
|T   |0|1|2|3|4|2|1|

5) Selectam vf 6

|vf  |1|2|3|4|5|6|7|
|----|-|-|-|-|-|-|-|
|viz |1|1|1|1|1|1|0|
|Cmin|0|2|1|1|1|2|3|
|T   |0|1|2|3|4|2|6|

6) Selectam vf 7 

|vf  |1|2|3|4|5|6|7|
|----|-|-|-|-|-|-|-|
|viz |1|1|1|1|1|1|1|
|Cmin|0|2|1|1|1|2|3|
|T   |0|1|2|3|4|2|6|

##Afisare arbore
```
1 (start)
2, T[2] (2, 1)
3, T[3] (3, 2)
4, T[4] (4, 3)
5, T[5] (5, 4)
6, T[6] (6, 2)
7, T[7] (7, 6)
```
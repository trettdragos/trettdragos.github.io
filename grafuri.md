Grafuri
===========================

Reprezentarea grafurilor in memorie
-------------

* Matricea de adiacenta
``` c++
int a[100][100], n;
void cititre()
{
    int m,x,y;
    cin>>n>>m;
    for(int k=1;k<=m;k++) {
        cin>>x>>y;
        a[x][y]=a[y][x]=1;//pentru graf neorientat
        a[x][y]=1;//pentru graf orientat
    }
}
```

* Liste de adiacenta
```
int l[100][100], n;
void cititre()
{
    int m,x,y;
    cin>>n>>m;
    for(int k=1;k<=m;k++) {
        cin>>x>>y;
        l[x][++l[x][0]]=y; l[y][++l[y][0]]=x; //graf neorientat
        l[x][++l[x][0]]=y; //graf orientat
    }
}
```

* Alocare dinamica
```
struct nod{
    nod* urm;
    int vf;
}*l[101];

void adaug(nod *&prim,int v)
{
    nod *nou=new nod;
    nou->vf=v;
    nou->urm=prim;
    prim=nou;
}

void cititre()
{
    int m,x,y;
    cin>>n>>m;
    for(int k=1;k<=m;k++) {
        cin>>x>>y;
        adaug(l[x],y); //graf orientat
        adaug(l[x],y); adaug(l[y],x); //graf neorientat
    }
}
```


Parcurgerea unui graf
-------------
**OBS** vecinii unui varf x sunt acele varfuri y cu propietatea ca muchia/arcul [x, y] exista

##Modalitati:

* In latime BFS
* In adancime DFS

##BFS

* Se pleaca dintr-un varf de start x
* se viziteaza vecinii nevizitati ai lui x, apoi vecinii nevizitati ai acestora si asa mai departe
* Se utilizeaza o coada si un vector de vizitare

####Algoritm

 1. adaugam varful de start la coada si il vizitam
 2. cat timp coada este nevida, extragem primul element din coada(v) pentru toti vecinii lui v nevizitati ii adaugam la coada si ii vizitam
 **OBS** coada implementata static -> vector cu doi indici p, u -> pozitia primul si ultimul
```
int n, a[100][100], c[100], viz[100];
void bfs(int x){
	int p, u;
	c[1]=x;
	p=u=1;
	viz[x]=1;
	while(p<=u){
		int v = c[p++];
		cout<<v<<" ";
		for(int i=1; i<=n; i++){
			if(!viz[i] && a[v][i]){
				c[++u]=i;
				viz[i]=1;
			}
		}
	} 
}
```

##DFS

* se pleaca dintr-un varf x, pe care il vizitam
* cautam primul vecin al lui x, apoi se cauta primul vecin nevizitat al lui si asa mai departe, pana cand varful curent nu mai are vecini nevizitati
* ne intoarcem la parintele lui si ii luam primul vecin nevizitat
* se poate face utilizand o stiva si un vector viz

####Algoritm

```
int n, a[100][100], viz[100];
void DFS(int x){
	viz[x]=1;  
	cout<<x<<" ";  
	for(int i=1; i<=n; i++)  
		if(a[x][i] && !viz[i])  
			DFS(i);  
}  
```

Determinarea componentelor conexe dintr-un graf
-------------

* se aplica parcurgerea din 1
* repeta pana cand toate vf sunt vizitate:  daca in urma aplicari parc mai exista vf neviz se identifica primul si se aplica alg de parcurgere 

```
int n, a[101][101], vic[101], nc=0;  

void citire(){  
	....  
}  

void DFS(int x){  
	viz[i]=1;
	cout<<x<<" ";  
	for(int i=1; i<=n; i++)  
		if(a[x][i] && !viz[i])  
			DFS(i);  
}  
void detCompConexe(){  
	for(int i=1; i<=n; i++)  
		if(!viz[i]){  
		nc++;  
		cout<<"comp conexe"<<nc<<" ";  
		DFS(i);  
		cout<<endl;  
	}	  
} 
```

Determinarea matricei lanturilor(drumurilor): **Roy-Worshal**
-------------

Matricea lanturilor = fie G un graf cu n varfuri Mnxn unde M[i][j]={1 daca exista lant; 0 daca nu exista}

Se aplica n tranf matricei de adiacenta astfel la posul k, orica k apartine 1->n matr devine ```a[i][j]=max{a[i][j], a[i][k]*a[k][j]} i, j apartin 1->n```
```
int a[101][101], n;
void RoyWorshall(){
	for(int k=1; k<=n; k++)
		for(int i=1; i<=n; i++)
			for(int j=1; j<=n; j++)
				if(a[i][j]==0)
					a[i][j]=a[i][k]*a[k][j];
}
```


Determinarea CTC (componente tare conexe)
-------------

###Metoda plus-minus

cat timp exista vf care nu sunt intr-o ctc:

* determinam un astfel de varf, fie acesta x
* aplicam un algoritm de parcurgere din x si notam varfurile atinse cu +
* aplicam un algoritm de parcurgere din x in graful transpus si notam varfurile atinse cu -
* CTC din care face parte x va contine vf care la cele 2 parcurgeri au fost etichetate si cu plus si cu minus

**OBS** Se numeste graf transpus al grafului G, graful notat cu G' in care schimbam directia tuturor arcelor din G

```
int n, a[101][101], AT[101][101], pluss[101], minuss[101], viz[101];

void citire(){
    freopen("date.in", "r", stdin);
    cin>>n;
    int x, y;
    while(cin>>x>>y){
        a[x][y]=1;
        AT[y][x]=1;
    }
}

void dfs(int x, int A[][101], int viz[]){
    viz[x]=1;
    for(int i=1; i<=n; i++){
        if(A[x][i] && !viz[i]){
            dfs(i, A, viz);
        }
    }
}

void reset(int aux[]){
    for(int i=1; i<=n; i++)
        aux[i]=0;
}

void ctc(){
    int nr=0;
    for(int x=1; x<=n; x++){
        if(!viz[x]){
            nr++;
            dfs(x, a, pluss);
            dfs(x, AT, minuss);
            for(int i=1; i<=n; i++){
                if(minuss[i] && pluss[i]) {
                    cout << i << " ";
                    viz[i] = 1;
                }
            }
            cout<<endl;
            reset(pluss);
            reset(minuss);
        }
    }
}

int main() {
    citire();
    ctc();
    return 0;
}
```


Determinarea unui graf/ciclu Hamiltonian
-------------

```
int a[101][101], n, viz[101], x[101], ok;

void citire(){
    freopen("date.in", "r", stdin);
    cin>>n;
    int x, y;
    while(cin>>x>>y){
        a[x][y]=1;
    }
}

void bkt(int k){
    if(k==n+1 && a[x[1]][x[k-1]]){
        cout<<"graf hamiltonian"<<endl;
        for(int i=1; i<=n; i++){
            cout<<x[i]<<" ";
            ok=1;
            return;
        }
    }
    if(!ok){
        for(int i=1; i<=n; i++){
            if(!viz[i]){
                if(a[x[k-1]][i]){
                    x[k]=i;
                    viz[i]=1;
                    bkt(k+1);
                    if(ok){
                        return;
                    }
                }
                viz[i]=0;
            }
        }
    }
}

int main() {
    citire();
    x[1]=1;
    bkt(2);
    if(!ok) cout<<"! hamiltonian";
    return 0;
}
```

Determinarea unui ciclu Eulerian (g neorientate)
-------------

```
int n, a[101][101], grade[101], viz[101];

void citire(){
    freopen("date.in", "r", stdin);
    cin>>n;
    int x, y;
    while(cin>>x>>y) {
        a[x][y]=a[y][x]=1;
        grade[x]++;
        grade[y]++;
    }
}

void detCiclu(int v, int c[], int &nr){
    c[1]=v;
    nr=1;
    do{
        for (int i = 1; i <= n ; i++) {
            if(a[v][i]){
                c[++nr]=i;
                a[v][i]=a[i][v]=0;
                grade[v]--;
                grade[i]--;
                v=i;
                break;
            }
        }
    }while (c[1]!=v);
}

void cicluEulerian(){
    int c1[201], c2[201], n1, n2, v, poz=0;
    int start;
    for(int i=1; i<=n; i++){
        if(grade[i]){
            start=i;
            break;
        }
    }
    detCiclu(start, c1, n1);
    //det primul vf din c1 care are gradul nenul
    do{
        v=0;
        for(int i=poz+1; i<=n1; i++){
            if(grade[c1[i]]){
                v=c1[i];
                poz=i;
                break;
            }
        }
        if(v){
            detCiclu(v, c2, n2);
            //concatenam c1 la c2
            //deplasam elem din c1 spre dr cu n2-1 poz
            for(int i=n1; i>=poz+1; i--){
             c1[i+n2-1] = c1[i];
            }
            for (int i = 2; i <=n2 ; i++) {
                c1[poz+i-1] = c2[i];
            }
            n1 = n1 + n2 - 1;
        }
    }while (v);
    for (int i = 1; i <= n1; i++) {
        cout<<c1[i]<<" ";
    }
}
```

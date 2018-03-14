#Algoritmul lui Dijkstra
#C++
###Graf orientat

```
define inf 100000
int c[101][101], D[101], S[101], PREC[101], n, x0;

void citire(){
	ifstream f("date.in");
	f>>n>>x0;
	for(int i=1; i<=n; i++){
		for(int j=1; j<=n; j++){
			C[i][j]=inf;
		}
	}
	for(int i=1; i<=n; i++){
		C[i][i]=0;
	}
	int x, y, cost;
	while(f>>x>>y>>cost){
		c[x][y]=cost;
	}
}

void dijkstra(){
	for(int i=1; i<=n; i++){
		if(C[x0][i]<inf){
			D[i]=C[x0][i];
			PREC[i]=x0;
		}
		else{
			D[i]=inf;
			PREC[i]=0;
		}
	}
	S[x0]=1;
	int k, minim;
	do{
		k=-1;
		minim=inf;
		//determinam minimul din D pt vf care nu apartine lui S
		//aflam varful urmator
		for(int i=1; i<=n; i++){
			if(S[i]==0D[i]<minim){
				minim=D[i];
				k=i;
			}
		}
		if(k!=-1){
			S[k]=1;
			for(int i=1; i<=n; i++){
				if(S[i]==0 && D[k]+C[k][i]<D[i]){
					D[i]=D[k]+C[k][i];
					PREC[i]=k;
				}
			}
		}

	}while(k!=-1);
}

void drum(int i){
	if(i!=x0){
		drum(PREC[i]);
		cout<<i<<" ";
	}
}

void afisare(){
	for(int i=1; i<=n; i++){
		if(D[i]==inf){
			cout<<"nu exista drum de la "<<x0<<" la "<<i;
		}else if(D[i]>0){
			cout<<"drumul minim de la "<<x0<<" la "<<i<<" : ";
			drum(i);
			cout<<endl;
		}
	}
}
```
Determina drumul minim intr-un graf plecand de la un nod dat catre toate celalate noduri ale grafului

Algotirmul det. drumurile de lungime minima din graf in ordine crescatoare astfel:

1)Initial drumul minim care pleaca din x0 este dat de arcul(muchia) de cost minim care iese din x0. Notam arcul (x0, xi)
2)Urmatorul drum minim selectat va fi: fie un arc/muchie care pleaca din x0, fie drumul (x0, xi, xj)
3)Algoritmul se termina in mom in care am gasit drumul minim de la vf de start(x0) la toate celalalte vf la care exista drum

###Structuri de date utilizate

- Matricea costurilor/lungimilor
- Multime S in care adaugam vf catre careau aflat dr. minim din x0
initial in S se afla vf de start
- vector Prec cu n elem utilizat pt afisarea drumurilor; Prec[i]={<br>
	x0, (x0, i)apartine U;
	0, daca i=x0 sau (x0, i) nu apartine U
<br>}
- Vector de n elem unde, D[i]={<br>lung drum minim de la x0 la i, daca i apartine lui S;
								lungimea unui drum de la x0 la i, daca i nu apartine S<br>}
- initial D[i]={<br>c(x0, i), daca (x0, i)apartine U<br>0, daca i = x0 <br> INF, daca (x0, i) nu apartine U <br>}

###Algoritmul

- alg selecteaza vf K, care nu apartine S, astfel incat D[k]=min{D[i]|i nu apartine S}
- k se adauga la S
- actualizam drumurile pt toti vecinii lui k care nu sunt in S daca ```D[j]>D[k]+C[k][j]``` unde j este vecinul lui K
- => ```D[j]=D[k]+c[k][j]```

###Exemplu
!!! antentie
i = infinit
####Initial

||1|2|3|4|5|6|
|----|-|-|-|-|-|-|
|S   |1|0|0|0|0|0|
|D   |0|1|i|i|3|i|
|Prec|0|1|0|0|1|0|

####Pasul 1: Selectam vf 2

||1|2|3|4|5|6|
|----|-|-|-|-|-|-|
|S   |1|2|0|0|0|0|
|D   |0|1|8|i|2|i|
|Prec|0|1|2|0|1|0|

####Pasul 2: Selectam vf 5

||1|2|3|4|5|6|
|----|-|-|-|-|-|-|
|S   |1|2|0|0|0|0|
|D   |0|1|4|i|2|i|
|Prec|0|1|5|0|1|0|

####Pasul 3: Selectam vf 3

||1|2|3|4|5|6|
|----|-|-|-|-|-|-|
|S   |1|1|1|0|1|0|
|D   |0|1|4|5|2|i|
|Prec|0|1|5|3|2|0|

####Pasul 4: Selectam vf 4 => nu are vecini

||1|2|3|4|5|6|
|----|-|-|-|-|-|-|
|S   |1|1|1|1|1|0|
|D   |0|1|4|5|2|i|
|Prec|0|1|5|3|2|0|

####Pasul 5: Selectam vf 

||1|2|3|4|5|6|
|----|-|-|-|-|-|-|
|S   |1|1|1|1|1|0|
|D   |0|1|4|5|2|i|
|Prec|0|1|5|3|2|0|

#Test din:  Alg prim, kruskal, Roy floyd, Dijkstra
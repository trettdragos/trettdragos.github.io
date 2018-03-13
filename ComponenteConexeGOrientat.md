#Algoritm pentru determinarea componentelor conexe dintr-un graf neorientat!  
Exemplu:  
parcurgere adancime din 1: 1, 4, 3, 7, 9	I comp conexa  
parc din 2: 2, 8, 10, 11					II comp conexa  
parc din 5: 5, 6							III comp conexa  

se aplica parcurgere pentru vf 1  
repeta pana cand toate vf sunt vizitate:  
	daca in urma aplicari parc mai exista vf neviz se identifica primul si se aplica alg de parcurgere  

Plecam din mat de adiacenta  

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


#Matricea lanturilor/drumurilor  

Fie G un graf cu n vf  
Mnxn, M[i][j]={1 daca de la vf i exista lant/drum  
			  {0 caz contrar  
Ex Gr orientat 1-2, 1-5, 2-3, 2-4, 4-1, 4-3, 4-5  
Matrice :  
  1 2 3 4 5  
1 1 1 1 1 1  
2 1 1 1 1 1  
3 0 0 0 0 0  
4 1 1 1 1 1  
5 0 0 0 0 0  
  
#Algoritmul Roy-Worshal  
Det matricea lanturilor/drumurilor plecand de la matricea de adiacenta  
Se aplica n tranf matricei de adiacenta astfel la posul k, orica k apartine 1->n matr devine  
a[i][j]=max{a[i][j], a[i][k]*a[k][j]} i, j apartin 1->n  
alg:  
int a[101][101], n;
void RoyWorshall(){
	for(int k=1; k<=n; k++)
		for(int i=1; i<=n; i++)
			for(int j=1; j<=n; j++)
				if(a[i][j]==0)
					a[i][j]=a[i][k]*a[k][j];
}

Lectia Continuare

1) aij=1 => exista ciclu/circuit de la i la j
2) aij=0 => oricare j=1->n  exista drum/cai pleaca din i
3) i, j 0 +> izolat
4) aij = 1

#Algoritm pentru determinare  unui ciclu eulerian intr-un graf neorientat
graf fara vf izolate este eulerian <=> g conex si toate vf au gradul par


#Arborele partial de cost minim (apm)
Fir G = (X, U) graf conex cu n vf
Consideram functia c:U->R+
care asociaza fiecareio muchii o valoare reala poz

Def - H=(X, V) este un graf partial al lui G, numit costul grafului H =  suma costurilor tuturor muchilor din V
cost(H)=suma(c(u))

Pb. Se ceree gasirea unui graf partial al lui G de cost minim	=> arbore de cost minim

#Algoritmul lui Kruskal

-contruieste arborele partial de cost minim
Se pleaca de la n arboti H1, H2....Hn unde ...
	Hi=({i}, null) (fiecare vf este un arbore)
Selectam n-1 muchii astfel
	-se alege muchia de cost minim neselectata anterior si care nu conduce la aparitia unui ciclu in graful nou

Exemplu:
--caiet--

Alg:Clion;
Graful se retine prin vectorul de muchii
Acest vec se sorteaza cresc dupa cost
C n elem unde C[i] = comp conexa din care face parte vf i
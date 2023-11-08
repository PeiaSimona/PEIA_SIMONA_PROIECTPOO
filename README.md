//PEIA_SIMONA_PROIECTPOO
// fazele 1,2 si 3
#include<iostream>
using namespace std;
#include<string>
 // domeniul ales este plafar



class Medicament
{

private:

	static int contor;
	const int id;
	char * denumire;
	float   pret;

public:

	//operatori 
	friend ostream& operator<<(ostream& out, Medicament m){

		out << "Denumire: " << m.denumire << endl;
		out << "ID " << m.id << endl;
		out << "Pret: " << m.pret << " roni" << endl;

		return out;
	}

	Medicament operator +(float p){
		Medicament c = *this;
		c.pret += p;
		return c;
	}

	Medicament& operator++(){
		this->pret += 1;
		return *this;
	}

	void operator()(float p){
		this->pret += p;
	}




	//set-eri

	static void setContor(int c){
		if (c >= 0){
			Medicament::contor = c;
		}
	}

	void setDenumire(const char* den)
	{
		if (strlen(den) > 2){
			delete[] this->denumire;
			this->denumire = new char[strlen(den) + 1];
			strcpy(this->denumire, den);
		}
	}

	void setPret(float p){
		if (p >= 0){
			this->pret = p;
		}
	}


	// get-eri

	char* getDenumire(){
		return this->denumire;
	}

	float getPret(){
		return this->pret;
	}

	int getId(){
		return this->id;
	}


	static int getNrMedicamente(){
		return Medicament::contor;
	}


	Medicament() :id(contor++)
	{
		this->denumire = new char[strlen("na") + 1];
		strcpy(this->denumire, "na");
		this->pret = 0; 

	}

	Medicament(const char* den, float p) :id(contor++)
	{
		this->denumire = new char[strlen(den) + 1];
		strcpy(this->denumire, den);
		this->pret = p;

	}


	Medicament(const char* den ) :id(contor++)
	{
		this->denumire = new char[strlen(den) + 1];
		strcpy(this->denumire, den);
		this->pret =0;

	}



	Medicament(const Medicament& s) :id(contor++)
	{
		this->denumire = new char[strlen(s.denumire) + 1];
		strcpy(this->denumire, s.denumire);
		this->pret = s.pret;

	}




	Medicament&operator=(const Medicament& s)  
	{
		delete[] this->denumire;
		this->denumire = new char[strlen(s.denumire) + 1];
		strcpy(this->denumire, s.denumire);
		this->pret = s.pret;
		return *this;
	}



	void afisare(){
		cout << "ID " << this->id << endl;
		cout << "Denumire: " << this->denumire << endl;
		cout << "Pret " << this->pret << endl;
	}
 
	~Medicament(){
		contor--;
		delete[] this->denumire;
	}
	friend void setMarestePret(Medicament& m, float marire);

};
int Medicament::contor = 0;

void setMarestePret(Medicament& m, float marire){
	m.pret += marire;
}


class Farmacist
{

private:
	const int id;
	string nume;
	static string denumireFarmacie;
	int nrLuni;
	float * bonusuriPerLuna;

public:

	// operatori

	Farmacist& operator+=(float lunaNoua){

		float * vn = new float[this->nrLuni + 1];
		for (int i = 0; i < this->nrLuni; i++){
			vn[i] = this->bonusuriPerLuna[i];
		}
		vn[this->nrLuni] = lunaNoua;
		delete[] this->bonusuriPerLuna;
		this->bonusuriPerLuna = vn;
		this->nrLuni += 1;
 
		return *this;
	}
 
	Farmacist operator++(){
		Farmacist copie = *this;
		 this->operator+=(100);
		return copie;
	}


	friend istream& operator>>(istream& in, Farmacist & sursa){

		cout << "Numele este: ";
		in >> sursa.nume;
		cout << "Numarul de luni este: ";
		in >> sursa.nrLuni;
		delete[] sursa.bonusuriPerLuna;
		sursa.bonusuriPerLuna = new float[sursa.nrLuni];
		for (int i = 0; i < sursa.nrLuni; i++){
			cout  << "Bonusul din luna este : " << i + 1 << ": ";
			in >> sursa.bonusuriPerLuna[i];
		}


		return in;
	}


	float & operator[](int i){
		if (i >= 0 && i <= this->nrLuni){

			return this->bonusuriPerLuna[i];
		}
		else{
			throw new exception();
		}
	}

	//set-eri

	static void setDenumireFarmacie(string  d){
		if (d.length() >= 2){
			Farmacist::denumireFarmacie = d;
		}
	}

	void setNume(string n){
		if (n.length() > 2){
			this->nume = n;
		}
	}

	void setNrLuni(int n, float* b){
		if (n > 0 && b!=NULL){
			this->nrLuni = n;
			delete[] this->bonusuriPerLuna;
			this->bonusuriPerLuna = new float[n];
			for (int i = 0; i < n; i++){
				this->bonusuriPerLuna[i] = b[i];
			}
		}
	}

	//get-eri
	int getNrLuni(){
		return this->nrLuni;

	}

	float * getBonusuri(){
		return this->bonusuriPerLuna;
	}

	static string getFarmacie(){
		return Farmacist::denumireFarmacie;
	}

	string getNume(){
		return this->nume;
	}

	void afisare(){
		cout << "ID " <<this->id<< endl;
		cout << "Nume  : " << this->nume << endl;
		cout << "Nr luni  " << this->nrLuni << endl;
		for (int i = 0; i < this->nrLuni; i++){
			cout << "In luna " << i + 1 << " a luat bonus de" << this->bonusuriPerLuna[i] << " RON" << endl;
		}
	}

	~Farmacist(){
		delete[] this->bonusuriPerLuna;
	}

	static void schimbaDenumire(string den){
		Farmacist::denumireFarmacie = den;
	}

	Farmacist() :id(0)
	{
		this->nume = "na";
		this->nrLuni = 0;
		this->bonusuriPerLuna = NULL;

	}


	Farmacist(int idd, string num, int nrl,float* bo ) :id(idd)
	{
		this->nume = num;
		this->nrLuni = nrl;
		this->bonusuriPerLuna = new float[nrl];
		for (int i = 0; i < nrl; i++){
			this->bonusuriPerLuna[i] = bo[i];
		}

	}
	Farmacist(const Farmacist& s) :id(s.id)
	{
		this->nume = s.nume;
		this->nrLuni = s.nrLuni;
		this->bonusuriPerLuna = new float[s.nrLuni];
		for (int i = 0; i < s.nrLuni; i++){
			this->bonusuriPerLuna[i] = s.bonusuriPerLuna[i];
		}

	}


	Farmacist&operator=(const Farmacist& s) 
	{

		delete[] this->bonusuriPerLuna;
		this->nume = s.nume;
		this->nrLuni = s.nrLuni;
		this->bonusuriPerLuna = new float[s.nrLuni];
		for (int i = 0; i < s.nrLuni; i++){
			this->bonusuriPerLuna[i] = s.bonusuriPerLuna[i];
		}
		return *this;
	}


	Farmacist(int idd, string num   ) :id(idd)
	{
		this->nume = num;
		this->nrLuni = 0;
		this->bonusuriPerLuna = NULL;

	}


};
string Farmacist::denumireFarmacie = "FarmaLine";


class Client
{
private:
	const int anNastere;
	static int anCurent;
	char* nume;
	int puncteFidelitate;

public:


	Client& operator -=(int p){
		if (this->puncteFidelitate - p >=0 ){
			this->puncteFidelitate -= p;
		}
		else{
			this->puncteFidelitate = 0;
		}
	}
	
	bool operator>(Client c){
		return this->puncteFidelitate > c.puncteFidelitate;
	}


	bool operator==(const char* numeCautat){
		if (strcmpi(this->nume , numeCautat ) == 0){
			return 1;
		}
		else{
			return false;
		}
	}

	bool operator!(){
		return  (puncteFidelitate > 0);
	}



	//set-eri



	static void setAnCurent(int ac){
		if (ac >= 0){
			Client::anCurent = ac;
		}
	}

	void setNume(const char* n)
	{
		if (strlen(n) > 2){
			delete[] this->nume;
			this->nume = new char[strlen(n) + 1];
			strcpy(this->nume, n);
		}
	}

	void setPuncteFidelitate(int  p){
		if (p >= 0){
			this->puncteFidelitate = p;
		}
	}
	//get-eri

	int getAnNastere(){
		return this->anNastere;
	}
	static int getAnCurent(){
		return Client::anCurent;
	}
	char* getNume(){
		return this->nume;
	}
	int getPuncteFidelitate(){
		return this->puncteFidelitate;
	}

	void afisare(){
		cout << "Varsta " << Client::anCurent- this->anNastere << endl;
		cout << "Nume client: " << this->nume << endl;
		cout << "Pucnte " << this->puncteFidelitate << endl;
	}

	static void incrementareAn(){
		Client::anCurent++;
	}


	~Client(){
		delete[] this->nume;
	}

	Client() :anNastere(1950)
	{
		this->nume = new char[strlen("na") + 1];
		strcpy(this->nume, "na");
		this->puncteFidelitate = 0;

	}

	Client(const char* num,int ann, int p) :anNastere(ann)
	{
		this->nume = new char[strlen(num) + 1];
		strcpy(this->nume, num);
		this->puncteFidelitate = p;

	}

	Client&operator=(const Client& s)  
	{
		delete[] this->nume;
		this->nume = new char[strlen(s.nume) + 1];
		strcpy(this->nume, s.nume);
		this->puncteFidelitate = s.puncteFidelitate;
		return *this;
	}

	Client(const char* num, int ann ) :anNastere(ann)
	{
		this->nume = new char[strlen(num) + 1];
		strcpy(this->nume, num);
		this->puncteFidelitate = 0;

	}
	friend int getVarsta(Client & c);

};
int Client::anCurent = 2023;


int getVarsta(Client & c){
	return Client::anCurent - c.anNastere;
}



int main()  // programul principal 
{



	Medicament m;
	Client c;
	Farmacist f;



	Medicament m1("ceai",23);
	Client c1("Popescu",1992,232);
	Farmacist f1(1, "Ionescu", 2, new float[2]{5.4f,6.6f});

	Medicament m2("aspirina" );
	Client c2("Manolescu", 1992 );
	Farmacist f2(1, "Simion"  );

	 
	cout<<"Nr de medicamente este: "<<	Medicament::getNrMedicamente() << endl;
	Farmacist::schimbaDenumire(" Medicamentul vitamine");
	Client::incrementareAn();


	m.afisare();
	m1.afisare();
	m2.afisare();

	c.afisare();
	c1.afisare();
	c2.afisare();

	f.afisare();
	f1.afisare();
	f2.afisare();

	m.setDenumire("Miere");
	m.setPret(23.3f);
	Medicament::setContor(0);

	cout << m.getDenumire() << endl;
	cout << m.getId() << endl;
	cout << Medicament::getNrMedicamente() << endl;
	cout << m.getPret()<< endl;


	f.setDenumireFarmacie("BioFarma");
	f.setNrLuni(2, new float[2]{4.5f, 3.4f});
	f.setNume("Toma");
	
	cout << f.getNume() << endl;
	cout << Farmacist::getFarmacie() << endl;
	for (int i = 0; i< f.getNrLuni(); i++){
		cout << f.getBonusuri()[i] << endl;
	}



	Client::setAnCurent(2024);
	c.setNume("Mihai");
	c.setPuncteFidelitate(123);
	
	cout << Client::getAnCurent() << endl;
	cout << c.getAnNastere() << endl;
	cout << c.getNume() << endl;
	cout << c.getPuncteFidelitate() << endl;


	//functii globale prietene cu clasele cleint si medicament
	setMarestePret(m, 12.2f);
	cout << "Varsta este " << getVarsta(c) << endl;


	
	f = f1;
	c = c1;
	m = m1;


	
	cout << m;
	m = m + 12;
	++m;
	m(13);

	
	cin >> f;
	f += 12;
	f++;
	cout << f[1] << endl;


	
	if (c  == ("Mircea")){
		cout << "Numele lui f este Mircea" << endl;
	}
	else{
		cout << "Numele lui f nu este Mircea " << endl;
	}

	if (!c){
		cout << "C nu are 0 puncte" << endl;
	}

	if (c > c1){
		cout << "C are mai multe puncte decat c1" << endl;
	}
	else{
		cout << "C nu are mai multe pucnte decat c1" << endl;
	}

	c -= 12;


	return 0;
}

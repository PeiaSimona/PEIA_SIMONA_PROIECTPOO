// PEIA FLORENTINA SIMONA PROIECT POO
#include<iostream>
using namespace std;
#include<fstream>
#include<string>
// domeniul ales este plafar


class InterfataMedicament{

public:
	virtual void afisareTipMedicament() = 0;


};


class Medicament: public InterfataMedicament
{

private:

	static int contor;
	const int id;
	char * denumire;
	float   pret;

public:

	virtual void afisareTipMedicament() {

		cout << "Acesta e un medicament cu pret normal" << endl;

	}

	friend istream& operator>>(istream & in, Medicament & m){
		cout << "Da nume: ";
		 
		char buffer[200];
		in >> buffer;
		m.setDenumire(buffer);

		cout << "Da pret ";
		in >> m.pret;

		return in;
	}

	friend ifstream& operator>>(ifstream & in, Medicament & m){
		 
		char buffer[200];
		in >> buffer;
		m.setDenumire(buffer);

	 
		in >> m.pret;

		return in;
	}

	friend ofstream& operator<<(ofstream& out, Medicament m){

		out   << m.denumire << endl;
	 
		out  << m.pret   << endl;

		return out;
	}



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




	//set 

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


	// get 

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


	Medicament(const char* den) :id(contor++)
	{
		this->denumire = new char[strlen(den) + 1];
		strcpy(this->denumire, den);
		this->pret = 0;

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



// faza 7 
class MedicamentCompensat:public Medicament
{

	float valoareCompensata = 0 ;

public:

	  void afisareTipMedicament() {

		cout << "Acesta e un medicament compensat cu reducere de "<< valoareCompensata<< " ron" << endl;

	}


	void setValCom(float p){
		if (p){
			this->valoareCompensata = p;
		}
	}
	float getValComp(){
		return valoareCompensata;
	}

	MedicamentCompensat(){

	}

	MedicamentCompensat(const char* den, float p, float c) :Medicament(den,p)
	{
		valoareCompensata = c;
	}

	MedicamentCompensat(const MedicamentCompensat & m) :Medicament( m)
	{
		valoareCompensata = m.valoareCompensata;
	}

	MedicamentCompensat & operator= (const MedicamentCompensat & m) 
	{
		Medicament::operator=(m);
		valoareCompensata = m.valoareCompensata;
		return *this;
	}

	~MedicamentCompensat(){

	}


};



// has a faza 5 ... 

class ListaMedicamente
{
	const int id;
	string numePacient;
	int nrMedicamente;
	Medicament * medicamente;

public:
	friend ostream & operator<<(ostream & out, ListaMedicamente & sursa){

		out << "Nume  : " << sursa.numePacient << endl;
		out << "nr mediecamente " << sursa.nrMedicamente << endl;
		for (int i = 0; i < sursa.nrMedicamente; i++){
			out << sursa.medicamente[i] << endl;
		}
		return  out;
	}



 
	ListaMedicamente& operator+=(Medicament  m){

		Medicament * vn = new Medicament[this->nrMedicamente + 1];
		for (int i = 0; i < this->nrMedicamente; i++){
			vn[i] = this->medicamente[i];
		}
		vn[this->nrMedicamente] = m;
		delete[] this->medicamente;
		this->medicamente = vn;
		this->nrMedicamente += 1;

		return *this;
	}
 
	ListaMedicamente operator++(){
		ListaMedicamente copie = *this;

		this->operator+=(Medicament()); 
		return copie;
	}


	friend istream& operator>>(istream& in, ListaMedicamente & sursa){

		cout << "da numele: ";
		in >> sursa.numePacient;
		cout << "Da nr  : ";
		in >> sursa.nrMedicamente;
		delete[] sursa.medicamente;
		sursa.medicamente = new Medicament[sursa.nrMedicamente];
		for (int i = 0; i < sursa.nrMedicamente; i++){
 
			in >> sursa.medicamente[i];
		}


		return in;
	}


	Medicament & operator[](int i){
		if (i >= 0 && i <= this->nrMedicamente){

			return this->medicamente[i];
		}
		else{
			throw new exception();//am lansat o exceptie daca indexul e gresit, practic se intrerupe din executie codul
		}
	}

 

	void setNume(string n){
		if (n.length() > 2){
			this->numePacient = n;
		}
	}

	void setNrLuni(int n, Medicament* b){
		if (n > 0 && b != NULL){
			this->nrMedicamente = n;
			delete[] this->medicamente;
			this->medicamente = new Medicament[n];
			for (int i = 0; i < n; i++){
				this->medicamente[i] = b[i];
			}
		}
	}

	//get
	int getNMedicamente(){
		return this->nrMedicamente;

	}

	Medicament * getMedicamente(){
		return this->medicamente;
	}

 
	string getNume(){
		return this->numePacient;
	}

 

	~ListaMedicamente(){
		delete[] this->medicamente;
	}
 

	ListaMedicamente() :id(0)
	{
		this->numePacient = "na";
		this->nrMedicamente = 0;
		this->medicamente = NULL;

	}


	ListaMedicamente(int idd, string num, int nr , Medicament* bo) :id(idd)
	{
		this->numePacient = num;
		this->nrMedicamente = nr ;
		this->medicamente = new Medicament[nr ];
		for (int i = 0; i < nr ; i++){
			this->medicamente[i] = bo[i];
		}

	}
	ListaMedicamente(const ListaMedicamente& s) :id(s.id)
	{
		this->numePacient = s.numePacient;
		this->nrMedicamente = s.nrMedicamente;
		this->medicamente = new Medicament[s.nrMedicamente];
		for (int i = 0; i < s.nrMedicamente; i++){
			this->medicamente[i] = s.medicamente[i];
		}

	}


	ListaMedicamente& operator= (const ListaMedicamente& s)  
	{
		delete[] this->medicamente;
		this->numePacient = s.numePacient;
		this->nrMedicamente = s.nrMedicamente;
		this->medicamente = new Medicament[s.nrMedicamente];
		for (int i = 0; i < s.nrMedicamente; i++){
			this->medicamente[i] = s.medicamente[i];
		}
		return *this;
	}


	 


};


// Has a cu pointeri ... faza 8 
class ListaMedicamentePointeri
{
	const int id;
	string numePacient;
	int nrMedicamente;
	Medicament ** medicamente;

public:
	friend ostream & operator<<(ostream & out, ListaMedicamentePointeri & sursa){

		out << "Nume  : " << sursa.numePacient << endl;
		out << "nr mediecamente " << sursa.nrMedicamente << endl;
		for (int i = 0; i < sursa.nrMedicamente; i++){
			out << *(sursa.medicamente[i]) << endl;
		}
		return  out;
	}

  
 


	~ListaMedicamentePointeri(){
		delete[] this->medicamente;
	}


	ListaMedicamentePointeri() :id(0)
	{
		this->numePacient = "na";
		this->nrMedicamente = 0;
		this->medicamente = NULL;

	}


	ListaMedicamentePointeri(int idd, string num, int nr, Medicament** bo) :id(idd)
	{
		this->numePacient = num;
		this->nrMedicamente = nr;
		this->medicamente = new Medicament*[nr];
		for (int i = 0; i < nr; i++){
			this->medicamente[i] = bo[i];
		}

	}
	ListaMedicamentePointeri(const ListaMedicamentePointeri& s) :id(s.id)
	{
		this->numePacient = s.numePacient;
		this->nrMedicamente = s.nrMedicamente;
		this->medicamente = new Medicament*[s.nrMedicamente];
		for (int i = 0; i < s.nrMedicamente; i++){
			this->medicamente[i] = s.medicamente[i];
		}

	}


	ListaMedicamentePointeri& operator= (const ListaMedicamentePointeri & s)
	{
		delete[] this->medicamente;
		this->numePacient = s.numePacient;
		this->nrMedicamente = s.nrMedicamente;
		this->medicamente = new Medicament*[s.nrMedicamente];
		for (int i = 0; i < s.nrMedicamente; i++){
			this->medicamente[i] = s.medicamente[i];
		}
		return *this;
	}





};







class Farmacist
{

private:
	const int id;
	string nume;
	static string denumireFarmacie;
	int nrLuni;
	float * bonusuriPerLuna;

public:


	friend ofstream & operator<<(ofstream & out, Farmacist & sursa){

		int l = sursa.nume.length() + 1;
		out.write((char*)&l, 4);
		out.write( sursa.nume.c_str(), l);

		out.write((char*)&sursa.nrLuni, 4);

		for (int i = 0; i < sursa.nrLuni; i++){
			out.write((char*)&sursa.bonusuriPerLuna[i], 4);
		}


		return  out;
	}


	friend ifstream& operator>>(ifstream& in, Farmacist & sursa){

		int l = sursa.nume.length() + 1;
		in.read((char*)&l, 4);
		char buffer[200];
		in.read( buffer, l);

		sursa.setDenumireFarmacie(buffer);

		in.read((char*)&sursa.nrLuni, 4);
		delete[] sursa.bonusuriPerLuna;
		sursa.bonusuriPerLuna = new float[sursa.nrLuni];
		for (int i = 0; i < sursa.nrLuni; i++){
			in.read((char*)&sursa.bonusuriPerLuna[i], 4);
		}


		return in;
	}





	friend ostream & operator<<(ostream & out, Farmacist & sursa){

		out << "Nume client: " << sursa.nume << endl;
		out << "Pucnte " << sursa.nrLuni << endl;
		for (int i = 0; i < sursa.nrLuni; i++){
			out << sursa.bonusuriPerLuna[i] << endl;
		}
		return  out;
	}




	// operatori
	//+= adauga o noua pozitie in vectorul dinamic
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

	// ++ post incrementare  care adauga o luna default de 100 ron bonus 
	Farmacist operator++(){
		Farmacist copie = *this;
		this->operator+=(100);//am folosit operatorul precendent pentru a adaugat noua luna 
		return copie;
	}


	friend istream& operator>>(istream& in, Farmacist & sursa){

		cout << "da numele: ";
		in >> sursa.nume;
		cout << "Da nr luni: ";
		in >> sursa.nrLuni;
		delete[] sursa.bonusuriPerLuna;
		sursa.bonusuriPerLuna = new float[sursa.nrLuni];
		for (int i = 0; i < sursa.nrLuni; i++){
			cout << "Da bonusul din luna " << i + 1 << ": ";
			in >> sursa.bonusuriPerLuna[i];
		}


		return in;
	}


	float & operator[](int i){
		if (i >= 0 && i <= this->nrLuni){

			return this->bonusuriPerLuna[i];
		}
		else{
			throw new exception();//am lansat o exceptie daca indexul e gresit, practic se intrerupe din executie codul
		}
	}



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
		if (n > 0 && b != NULL){
			this->nrLuni = n;
			delete[] this->bonusuriPerLuna;
			this->bonusuriPerLuna = new float[n];
			for (int i = 0; i < n; i++){
				this->bonusuriPerLuna[i] = b[i];
			}
		}
	}

	//get
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
		cout << "ID " << this->id << endl;
		cout << "Nume  : " << this->nume << endl;
		cout << "nr luni  " << this->nrLuni << endl;
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


	Farmacist(int idd, string num, int nrl, float* bo) :id(idd)
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


	Farmacist(int idd, string num) :id(idd)
	{
		this->nume = num;
		this->nrLuni = 0;
		this->bonusuriPerLuna = NULL;

	}


};
string Farmacist::denumireFarmacie = "Dona";



class InterfataClient{
public:
	virtual void afisareTipClient() = 0;
};


class Client:public InterfataClient
{
private:
	const int anNastere;// 1995
	static int anCurent;// 2023
	char* nume;
	int puncteFidelitate;

public:

	friend ifstream& operator>>(ifstream & in, Client & m){

		char buffer[200];
		in >> buffer;
		m.setNume(buffer);


		in >> m.puncteFidelitate;

		return in;
	}

	friend ofstream& operator<<(ofstream& out, Client m){

		out << m.nume << endl;

		out << m.puncteFidelitate << endl;

		return out;
	}




	virtual void afisareTipClient() {
		cout << "Acesta e client normal" << endl;
	}

	friend istream& operator>>(istream & in, Client & m){
		cout << "Da nume: ";

		char buffer[200];
		in >> buffer;
		m.setNume(buffer);

		cout << "Da puncteFidelitate ";
		in >> m.puncteFidelitate;

		return in;
	}


	Client& operator -=(int p){
		if (this->puncteFidelitate - p >= 0){
			this->puncteFidelitate -= p;
		}
		else{
			this->puncteFidelitate = 0;
		}
		return *this; 
	}

	bool operator>(Client c){
		return this->puncteFidelitate > c.puncteFidelitate;
	}


	bool operator==(const char* numeCautat){
		if (strcmpi(this->nume, numeCautat) == 0){
			return 1;
		}
		else{
			return false;
		}
	}

	bool operator!(){
		return  (puncteFidelitate > 0);
	}







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
	//get\

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
		cout << "Varsta " << Client::anCurent - this->anNastere << endl;
		cout << "Nume client: " << this->nume << endl;
		cout << "Pucnte " << this->puncteFidelitate << endl;
	}


	friend ostream & operator<<(ostream & out, Client & sursa){
		 
		 out << "Nume client: " << sursa.nume << endl;
		 out << "Pucnte " << sursa.puncteFidelitate << endl;

		return  out;
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

	Client(const char* num, int ann, int p) :anNastere(ann)
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

	Client(const Client& s) :anNastere(s.anNastere)
	{
	 
		this->nume = new char[strlen(s.nume) + 1];
		strcpy(this->nume, s.nume);
		this->puncteFidelitate = s.puncteFidelitate;
		 
	}


	Client(const char* num, int ann) :anNastere(ann)
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

//faza 7 

class ClientOnline :public Client
{

	int  nrComenziLaAdresa = 0;

public:
	  void afisareTipClient() {
		cout << "Acesta e client normal online" << endl;
	}
	void setnrComenzi(int  p){
		if (p){
			this->nrComenziLaAdresa = p;
		}
	}
	float getnrComenzi(){
		return nrComenziLaAdresa;
	}

	ClientOnline(){

	}

	ClientOnline(const char* den, float p, int  c) :Client(den, p)
	{
		nrComenziLaAdresa = c;
	}

	ClientOnline(const ClientOnline & m) :Client(m)
	{
		nrComenziLaAdresa = m.nrComenziLaAdresa;
	}

	ClientOnline & operator= (const ClientOnline & m)
	{
		Client ::operator=(m);
		nrComenziLaAdresa = m.nrComenziLaAdresa;
		return *this;
	}

	~ClientOnline(){

	}


};







// Has a cu pointeri ... faza 8 
class ListaClientiPointeri
{
	const int id;
	string listaClientiDenumire;
	int nrClienti;
	Client ** clienti;

public:
	friend ostream & operator<<(ostream & out, ListaClientiPointeri & sursa){

		out << "listaClientiDenumire  : " << sursa.listaClientiDenumire << endl;
		out << "nr mediecamente " << sursa.nrClienti << endl;
		for (int i = 0; i < sursa.nrClienti; i++){
			out << *(sursa.clienti[i]) << endl;
		}
		return  out;
	}





	~ListaClientiPointeri(){
		delete[] this->clienti;
	}


	ListaClientiPointeri() :id(0)
	{
		this->listaClientiDenumire = "na";
		this->nrClienti = 0;
		this->clienti = NULL;

	}


	ListaClientiPointeri(int idd, string num, int nr, Client** bo) :id(idd)
	{
		this->listaClientiDenumire = num;
		this->nrClienti = nr;
		this->clienti = new Client*[nr];
		for (int i = 0; i < nr; i++){
			this->clienti[i] = bo[i];
		}

	}
	ListaClientiPointeri(const ListaClientiPointeri& s) :id(s.id)
	{
		this->listaClientiDenumire = s.listaClientiDenumire;
		this->nrClienti = s.nrClienti;
		this->clienti = new Client*[s.nrClienti];
		for (int i = 0; i < s.nrClienti; i++){
			this->clienti[i] = s.clienti[i];
		}

	}


	ListaClientiPointeri& operator= (const ListaClientiPointeri & s)
	{
		delete[] this->clienti;
		this->listaClientiDenumire = s.listaClientiDenumire;
		this->nrClienti = s.nrClienti;
		this->clienti = new Client*[s.nrClienti];
		for (int i = 0; i < s.nrClienti; i++){
			this->clienti[i] = s.clienti[i];
		}
		return *this;
	}





};










int main()// programul principal 
{



	Medicament m;
	Client c;
	Farmacist f;



	Medicament m1("paracetamol", 23);
	Client c1("gigel", 1992, 232);
	Farmacist f1(1, "Costelush", 2, new float[2]{5.4f, 6.6f});

	Medicament m2("aspirina");
	Client c2("titelush", 1992);
	Farmacist f2(1, "Cornelush");


	//cout << "Nr de medicamente este: " << Medicament::getNrMedicamente() << endl;
	Farmacist::schimbaDenumire("la Medicamentul vois");
	Client::incrementareAn();


	//m.afisare();
	//m1.afisare();
	//m2.afisare();

	//c.afisare();
	//c1.afisare();
	//c2.afisare();

	//f.afisare();
	//f1.afisare();
	//f2.afisare();

	m.setDenumire("Penicilina");
	m.setPret(23.3f);
	Medicament::setContor(0);

	//cout << m.getDenumire() << endl;
	//cout << m.getId() << endl;
	//cout << Medicament::getNrMedicamente() << endl;
	//cout << m.getPret() << endl;


	f.setDenumireFarmacie("CostelFarm");
	f.setNrLuni(2, new float[2]{4.5f, 3.4f});
	f.setNume("Toma");

	//cout << f.getNume() << endl;
	//cout << Farmacist::getFarmacie() << endl;
	//for (int i = 0; i< f.getNrLuni(); i++){
	//	cout << f.getBonusuri()[i] << endl;
	//}



	Client::setAnCurent(2025);
	c.setNume("Marinush");
	c.setPuncteFidelitate(123);

	//cout << Client::getAnCurent() << endl;
	//cout << c.getAnNastere() << endl;
	//cout << c.getNume() << endl;
	//cout << c.getPuncteFidelitate() << endl;


	////functii globale prietene cu clasele cleint si medicament
	//setMarestePret(m, 12.2f);
	//cout << "Varsta este " << getVarsta(c) << endl;


	//// op = 
	f = f1;
	c = c1;
	m = m1;


	//// operatoro
	//cout << m;
	m = m + 12;
	++m;
	m(13);

	//// 
	//// cin >> f;//il tinem comentat sa nu ne puna mereu sa bagam de la tastatura cand facem debug
	f += 12;
	f++;
	//cout << f[1] << endl;


	/////
	if (c == ("Gigel")){
		//cout << "Da pe f il cheama gigel" << endl;
	}
	else{
		//cout << "Pe f nu il cheama gigel" << endl;
	}

	if (!c){
		//cout << "C nu are 0 puncte" << endl;
	}

	if (c > c1){
		//cout << "C are mai multe puncte c1" << endl;
	}
	else{
		//cout << "C nu are mai multe pucnte ca c1" << endl;
	}

	  c -= 12;




	  //FAZA 4
	  
	  Medicament vm[3];
	  Client vc[3];
	  Farmacist vf[3];

	  for (int i = 0; i < 3; i++){
		//  cin >> vm[i];
	  }

	  for (int i = 0; i < 3; i++){
		//  cin >> vc[i];
	  }

	  for (int i = 0; i < 3; i++){
		  //cin >> vf[i];
	  }

	  for (int i = 0; i < 3; i++){
		 // cout << vm[i];
	  }

	  for (int i = 0; i < 3; i++){
		 // cout << vc[i];
	  }

	  for (int i = 0; i < 3; i++){
		//  cout << vf[i];
	  }



	  Farmacist matfar[3][3];
	  for (int i = 0; i < 3; i++){
		  for (int j = 0; j < 3; j++){
			 // cin >> matfar[i][j];
		  }
	  }


	  for (int i = 0; i < 3; i++){
		  for (int j = 0; j < 3; j++){
			// cout<< matfar[i][j];
		  }
	  }




	  //faza 5

	  ListaMedicamente lm;
	  
	  ListaMedicamente lm1(121,"titel", 3, vm);
	  ListaMedicamente lm2(lm1);
	  lm = lm1;
	  lm1.getMedicamente();
	  lm1.getNMedicamente();
	  lm1.getNume();
	  lm.setNrLuni(0, NULL);

	  lm1++;
	  lm1[0];
	  lm1 += m;
	 

	  // faza 6

	  // - 
	  ofstream fout;
	  fout.open("fisermedi.txt", ofstream::out);
	  if (fout.is_open()){
		  fout << m1;
		  fout.close();
	  }

	 ifstream fin;
	 fin.open("fisermedi.txt", ifstream::in);
	 if (fin.is_open()){
		 fin >> m1;
		 fin.close();
	  }

	 ofstream fout1;
	 fout1.open("fiserclie.txt", ofstream::out);
	 if (fout1.is_open()){
		 fout1 << c1;
		 fout1.close();
	 }

	 ifstream fin1;
	 fin1.open("fiserclie.txt", ifstream::in);
	 if (fin1.is_open()){
		 fin1 >> c1;
		 fin1.close();
	 }


	 //binar 
	 ofstream fout2;
	 fout2.open("farmacie.bin", ofstream::binary);
	 if (fout2.is_open()){
		 fout2 << f1;
		 fout2.close();
	 }

	 ifstream fin2;
	 fin2.open("farmacie.bin", ifstream::binary);
	 if (fin2.is_open()){
		 fin2 >> f1;
		 fin2.close();
	 }

















	  //faza 7 
	  ClientOnline cf;
	  ClientOnline cf1("tite", 23, 23);
	  ClientOnline cf2(cf1);
	  cf = cf1;
	 // cout << (Client)cf1; // upcasting
	  cf1.getnrComenzi();
	  cf1.setnrComenzi(23);
	  

	  MedicamentCompensat mcp;
	  MedicamentCompensat mcp1("paracetamol",23,12);
	  MedicamentCompensat mcp2(mcp1);
	  mcp = mcp2;
	//   cout << (Medicament)mcp2; // upcasting
	  mcp.setValCom(2);
	  mcp.getValComp();





	  // faza 8 
	  InterfataMedicament * vifm[10]
	  {		new Medicament(),
	  new MedicamentCompensat(),
	  new MedicamentCompensat(),
	  new MedicamentCompensat(),
	  &m,
	  &m1,
	  &m2,
	  &mcp,
	  &mcp1,
	  &mcp2,

	  };


	  for (int i = 0; i < 10; i++){
		  vifm[i]->afisareTipMedicament();
	  }


	  InterfataClient * vcointer[10]
	  {		new Client(),
	  new ClientOnline(),
	  new Client(),
	  new ClientOnline(),
	  &c,
	  &c1,
	  &c1,
	  &c2,
	  &cf,
	  &cf2

	  };


	  for (int i = 0; i < 10; i++){
		  vcointer[i]->afisareTipClient();
	  }



	return 0;
}

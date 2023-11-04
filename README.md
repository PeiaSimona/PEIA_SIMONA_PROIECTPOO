//PEIA_SIMONA_PROIECTPOO
// faza 1 proiect

#include<iostream>
using namespace std;
#include<string>
 // domeniul ales este plafar



class Medicament 
{

public:

	static int contor;
	const int id;
	char * denumire;
	float   pret;



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



	void afisare(){
		cout << "ID " << this->id << endl;
		cout << "Denumire: " << this->denumire << endl;
		cout << "Pret " << this->pret << endl;
	}
 


};
int Medicament::contor = 0;

class Farmacist
{

public:
	const int id;
	string nume;
	static string denumireFarmacie;
	int nrLuni;
	float * bonusuriPerLuna;



	void afisare(){
		cout << "ID " <<this->id<< endl;
		cout << "Nume  : " << this->nume << endl;
		cout << "nr luni  " << this->nrLuni << endl;
		for (int i = 0; i < this->nrLuni; i++){
			cout << "In luna " << i + 1 << " a luat bonus de" << this->bonusuriPerLuna[i] << " RON" << endl;
		}
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
public:
	const int anNastere;
	static int anCurent;
	char* nume;
	int puncteFidelitate;




	void afisare(){
		cout << "Varsta " << Client::anCurent- this->anNastere << endl;
		cout << "Nume client: " << this->nume << endl;
		cout << "Pucnte " << this->puncteFidelitate << endl;
	}

	static void incrementareAn(){
		Client::anCurent++;
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


	Client(const char* num, int ann ) :anNastere(ann)
	{
		this->nume = new char[strlen(num) + 1];
		strcpy(this->nume, num);
		this->puncteFidelitate = 0;

	}


};
int Client::anCurent = 2023;


 



int main()
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
	Farmacist::schimbaDenumire("la Medicamentul vitamine");
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
 
	return 0;
}

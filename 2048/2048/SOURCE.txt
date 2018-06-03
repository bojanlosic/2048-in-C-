#include<iostream>
#include<stdlib.h>
#include<time.h>
#include<cstdlib>
#include<ctype.h>
#include<fstream>
//ubacivanje globalnih promenljivih, datoteka, struktura i prototipa funkcija
extern int matrica[4][4] = { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0 };
extern int provera[4][4] = { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0 };
char kretanje;
int poeni = 0, kaunter = 0;
using namespace std;
void kakoSeIgra();
int main();
char meni();
char mogucaKretanja();
void ispisivanjeMatrice();
void dodajNoviBroj();
void pomeriGore();
void pomeriDole();
void pomeriDesno();
void pomeriLevo();
void proveraUpis();
int proveraCitanje();
void resetujMatricu();
void zapociniIgru();
void nastaviIgru();
void gameOver();
int ispitajGameOver();
int ispitajDHCO();
void najboljiSkor();
void oIgri();
ofstream fout;
ifstream fin;
struct igrac {
	char ime[21];
	int skor;
}s;
//=====================================================================================================
//======================================= GLAVNI PROGRAM ==============================================
//=====================================================================================================
int main() {
	kakoSeIgra();
	char izbor;
	int greska = 0;
	do {
		izbor = meni();
		if (izbor == '?') {//kako se igra 
			kakoSeIgra();
			continue;
		}
		else if (izbor == 'p' || izbor == 'P') {//pocetak igre
			zapociniIgru();
		}
		else if (izbor == 'n' || izbor == 'N') {//nastavak igre
			nastaviIgru();
		}
		else if (izbor == 's' || izbor == 'S') {//pregled najboljeg skora
			najboljiSkor();
		}
		else if (izbor == 'i' || izbor == 'I'){//recite nesto o igri
			oIgri();
		}
		else if (izbor == 'e' || izbor == 'E') {//izlaz iz igre
			system("cls");
			cout << "\n\n\t\t\t\t\t\t\tDovidjenja...\n\n" << endl;
			system("pause");
			exit(1);
		}
		else {
			cout << "Niste uneli nista sto odgovara navedenim funkcijama igrice" << endl;
			system("pause");
		}
	} while (1);
	system("pause");
	return 0;
}
//===============================================================================================================================
//=================================================== FUNKCIJE ==================================================================
//===============================================================================================================================
//glavni meni u kom se odabira sve i moguce mu je pristupiti u svakom trenutku
char meni() {
	char izbor;
	system("cls");
	cout << "\n\n\t\t\t\t\t\t\t   2048" << endl;
	cout << "\t\t\t\t\t\t        Dobrodosli" << endl;
	cout << "\t\t\t\t\t   Igricu napravio Bojan Losic NRT 43 15" << endl << endl << endl;
	cout << "\t\t\t\t       Za pocetak igrice unesite slovo \"p\" (pocetak)" << endl << endl;
	cout << "\t\t\t\t       Za nastavak igre unesite slovo \"n\" (nastavak)" << endl << endl;
	cout << "\t\t\t\t    Za informacije kako se igra igra unesite \"?\" (kako)" << endl << endl;
	cout << "\t\t\t\t    Za pregled najboljeg skora unesite slovo \"s\" (skor)" << endl << endl;
	cout << "\t\t\t\t  Recite nam sta mislite o igri i unesite slovo \"i\" (igra)" << endl << endl;
	cout << "\t\t\t\t       Za izlazak iz igrice unesite slovo \"e\" (exit)" << endl << endl;
	cin >> izbor;
	return izbor;
}
//objasnjenje kontrola i cilja igre
void kakoSeIgra() {
	system("cls");
	cout << "\n\n\n\t\t\t\t\t\t\tKONTROLE:" << endl << endl;
	cout << "\t\tPritiskom dugmica \"w\", \"a\", \"s\", \"d\" pomerate sve brojeve u smerovima odredjenim ovim slovima" << endl;
	cout << "\t\t\t\tPritiskom na slovo \"w\", pomericete sve brojeve ka gore" << endl;
	cout << "\t\t\t\tPritiskom na slovo \"a\", pomericete sve brojeve ka levo" << endl;
	cout << "\t\t\t\tPritiskom na slovo \"s\", pomericete sve brojeve ka dole" << endl;
	cout << "\t\t\t\tPritiskom na slovo \"d\", pomericete sve brojeve ka desno" << endl << endl;
	cout << "\t\t\t\t\t\t\tCILJ IGRE:" << endl << endl;
	cout << "\t     Svaki put kada kliknete na neko od slova, pojavice se novi broj 2 ili 4 na slucajno izabranom mestu" << endl;
	cout << "\t\t\t          Kada se broj poklopi sa istim brojem, oni se sabiraju" << endl;
	cout << "\t\t\t\t\t     Cilj igre je sakupiti broj 2048" << endl;
	cout << "\t\t\t\t         Maksimalan broj na jednom polju je 131072" << endl;
	cout << "\t\t       Igra se zavrsava kada se  popuni citava tabla i brojevi vise ne mogu sabirati" << endl << endl;
	cout << "\t\t\t\tU bilo kom trenutku pritiskom na slovo \"m\" se vracate u meni" << endl;
	cout << "\t\t\t\t\t Pritisnite bilo sta za vracanje u meni" << endl << endl;
	system("pause");
}
//slova koja se mogu kliknuti za pomeranje kao i za meni ========== UZIMA SE SAMO PRVO SLOVO
char mogucaKretanja() {
	char slovo[100] = "";
	do {
		cin >> slovo;
		if (slovo[0] == 'w' || slovo[0] == 'W') {
			slovo[0] = tolower(slovo[0]);
			return slovo[0];
		}
		else if (slovo[0] == 'a' || slovo[0] == 'A') {
			slovo[0] = tolower(slovo[0]);
			return slovo[0];
		}
		else if (slovo[0] == 's' || slovo[0] == 'S') {
			slovo[0] = tolower(slovo[0]);
			return slovo[0];
		}
		else if (slovo[0] == 'd' || slovo[0] == 'D') {
			slovo[0] = tolower(slovo[0]);
			return slovo[0];
		}
		else if (slovo[0] == 'm' || slovo[0] == 'M') {
			slovo[0] = tolower(slovo[0]);
			return slovo[0];
		}
		else {
			cout << "Niste uneli dobro slovo pokusajte ponovo" << endl;
			continue;
		}
	} while (1);
}
//iscrtavanje table kao i brojeva koji se tu nalaze
void ispisivanjeMatrice() {
	system("cls");
	cout << "\n\n\n\t\t     w = /\\ ,\t s = \\/ ,\t d = > ,\t a = < ,\t m = meni\n\n\n";
	for (int i = 0; i < 4; i++) {
		cout << "\t\t\t\t    _________________________________________" << endl << endl << "\t\t\t\t";
		for (int j = 0; j < 4; j++) {
			if (matrica[i][j] < 10) {
				cout << "    |    " << matrica[i][j];
			}
			else if (matrica[i][j] > 10 && matrica[i][j] < 100) {
				cout << "    |   " << matrica[i][j];
			}
			else if (matrica[i][j] > 100 && matrica[i][j] < 1000) {
				cout << "    |  " << matrica[i][j];
			}
			else if (matrica[i][j] > 1000 && matrica[i][j] < 10000) {
				cout << "    | " << matrica[i][j];
			}
			else if (matrica[i][j] > 10000 && matrica[i][j] < 100000) {
				cout << "    |" << matrica[i][j];
			}
		}
		cout << "    |    " << endl;
	}
	cout << "\t\t\t\t    _________________________________________" << endl;
	cout << "\n\n\n\t\t\t\t\t     Vas trenutni skor je: " << poeni << endl;
	fin.open("najboljiSkor.txt", ios::in);
	if (!fin) {
		cerr << "Greska pri otvaranju datoteke" << endl;
		exit(1);
	}
	while (fin >> s.ime >> s.skor) {
		cout << "\n\n\n\t\t\t\t\t       Najbolji skor je: " << s.skor << endl;
	}
	fin.close();
}
//dodavanje novog broja slucajnim izborom na prazno polje dajuci 5% sansi da izadje broj 4 i 95% sansi da izadje broj 2
void dodajNoviBroj() {
	int i, j, brojac = 0, slucajni, dvaCetiri, josGreski = 0;
	for (i = 0; i < 4; i++) {
		for (j = 0; j < 4; j++) {
			if (matrica[i][j] == 0) {
				brojac++;
			}
		}
	}
	srand(time(NULL));
	slucajni = (rand() % brojac) + 1;
	dvaCetiri = rand() % 100;
	brojac = 0;
	for (i = 0; i < 4; i++) {
		for (j = 0; j < 4; j++) {
			if (matrica[i][j] == 0) {
				brojac++;
			}
			if (slucajni == brojac) {
				if (dvaCetiri > 5) {
					matrica[i][j] = 2;
					josGreski = 1;
					break;
				}
				else
				{
					matrica[i][j] = 4;
					josGreski = 1;
					break;
				}
			}
		}
		if (josGreski == 1) {
			break;
		}
	}
	ispisivanjeMatrice();
}
//===============================================================================================================================
//======================================================= LOGIKA POMERANJA ======================================================
//===============================================================================================================================
//^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
//============= GORE ===============
//^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
void pomeriGore() {
	for (int i = 0; i < 4; i++) {
		int popravkaGreske = 0;
		//============= DRUGI RED ===============
		if (matrica[1][i] != 0) {
			if (matrica[0][i] == 0) {
				matrica[0][i] = matrica[1][i];
				matrica[1][i] = 0;
			}
			else if (matrica[0][i] == matrica[1][i]) {
				poeni += matrica[0][i];
				matrica[0][i] = matrica[0][i] + matrica[1][i];
				matrica[1][i] = 0;
				popravkaGreske = 1;
				
			}
		}
		//============= TRECI RED ===============
		if (matrica[2][i] != 0) {
			if (matrica[0][i] == 0 && matrica[1][i] == 0) {
				matrica[0][i] = matrica[2][i];
				matrica[2][i] = 0;
			}
			else if (matrica[0][i] != 0 && matrica[1][i] == 0) {
				if (matrica[0][i] == matrica[2][i] && popravkaGreske == 0) {
					poeni += matrica[0][i];
					matrica[0][i] = matrica[0][i] + matrica[2][i];
					matrica[2][i] = 0;
					popravkaGreske = 1;
				}
				else {
					matrica[1][i] = matrica[2][i];
					matrica[2][i] = 0;
				}
			}
			else if (matrica[0][i] != 0 && matrica[1][i] != 0) {
				if (matrica[1][i] == matrica[2][i] && popravkaGreske == 0) {
					poeni += matrica[1][i];
					matrica[1][i] = matrica[1][i] + matrica[2][i];
					matrica[2][i] = 0;
					popravkaGreske = 1;
				}
			}
		}
		//============= CETVRTI RED ===============
		if (matrica[3][i] != 0) {
			if (matrica[0][i] == 0 && matrica[1][i] == 0 && matrica[2][i] == 0) {
				matrica[0][i] = matrica[3][i];
				matrica[3][i] = 0;
			}
			else if (matrica[0][i] != 0 && matrica[1][i] == 0 && matrica[2][i] == 0) {
				if (matrica[0][i] == matrica[3][i] && popravkaGreske == 0) {
					poeni += matrica[0][i];
					matrica[0][i] = matrica[0][i] + matrica[3][i];
					matrica[3][i] = 0;
				}
				else {
					matrica[1][i] = matrica[3][i];
					matrica[3][i] = 0;
				}
			}
			else if (matrica[0][i] != 0 && matrica[1][i] != 0 && matrica[2][i] == 0) {
				if (matrica[1][i] == matrica[3][i] && popravkaGreske == 0) {
					poeni += matrica[1][i];
					matrica[1][i] = matrica[1][i] + matrica[3][i];
					matrica[3][i] = 0;
				}
				else {
					matrica[2][i] = matrica[3][i];
					matrica[3][i] = 0;
				}
			}
			else if (matrica[0][i] != 0 && matrica[1][i] != 0 && matrica[2][i] != 0) {
				if (matrica[2][i] == matrica[3][i] && popravkaGreske == 0) {
					poeni += matrica[2][i];
					matrica[2][i] = matrica[2][i] + matrica[3][i];
					matrica[3][i] = 0;
				}
			}
		}
	}
	ispisivanjeMatrice();
}
//==================================
//============= DOLE ===============
//==================================
void pomeriDole() {
	for (int i = 0; i < 4; i++) {
		int popravkaGreske = 0;
		//============= TRECI RED ===============
		if (matrica[2][i] != 0) {
			if (matrica[3][i] == 0) {
				matrica[3][i] = matrica[2][i];
				matrica[2][i] = 0;
			}
			else if (matrica[3][i] == matrica[2][i]) {
				poeni += matrica[3][i];
				matrica[3][i] = matrica[3][i] + matrica[2][i];
				matrica[2][i] = 0;
				popravkaGreske = 1;
			}
		}
		//============= DRUGI RED ===============
		if (matrica[1][i] != 0) {
			if (matrica[3][i] == 0 && matrica[2][i] == 0) {
				matrica[3][i] = matrica[1][i];
				matrica[1][i] = 0;
			}
			else if (matrica[3][i] != 0 && matrica[2][i] == 0) {
				if (matrica[3][i] == matrica[1][i] && popravkaGreske == 0) {
					poeni += matrica[3][i];
					matrica[3][i] = matrica[3][i] + matrica[1][i];
					matrica[1][i] = 0;
					popravkaGreske = 1;
				}
				else {
					matrica[2][i] = matrica[1][i];
					matrica[1][i] = 0;
				}
			}
			else if (matrica[3][i] != 0 && matrica[2][i] != 0) {
				if (matrica[2][i] == matrica[1][i] && popravkaGreske == 0) {
					poeni += matrica[2][i];
					matrica[2][i] = matrica[2][i] + matrica[1][i];
					matrica[1][i] = 0;
					popravkaGreske = 1;
				}
			}
		}
		//============= PRVI RED ===============
		if (matrica[0][i] != 0) {
			if (matrica[3][i] == 0 && matrica[2][i] == 0 && matrica[1][i] == 0) {
				matrica[3][i] = matrica[0][i];
				matrica[0][i] = 0;
			}
			else if (matrica[3][i] != 0 && matrica[2][i] == 0 && matrica[1][i] == 0) {
				if (matrica[3][i] == matrica[0][i] && popravkaGreske == 0) {
					poeni += matrica[3][i];
					matrica[3][i] = matrica[3][i] + matrica[0][i];
					matrica[0][i] = 0;
				}
				else {
					matrica[2][i] = matrica[0][i];
					matrica[0][i] = 0;
				}
			}
			else if (matrica[3][i] != 0 && matrica[2][i] != 0 && matrica[1][i] == 0) {
				if (matrica[2][i] == matrica[0][i] && popravkaGreske == 0) {
					poeni += matrica[2][i];
					matrica[2][i] = matrica[2][i] + matrica[0][i];
					matrica[0][i] = 0;
				}
				else {
					matrica[1][i] = matrica[0][i];
					matrica[0][i] = 0;
				}
			}
			else if (matrica[3][i] != 0 && matrica[2][i] != 0 && matrica[1][i] != 0) {
				if (matrica[1][i] == matrica[0][i] && popravkaGreske == 0) {
					poeni += matrica[1][i];
					matrica[1][i] = matrica[1][i] + matrica[0][i];
					matrica[0][i] = 0;
				}
			}
		}
	}
	ispisivanjeMatrice();
}
//>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
//============= DESNO ===============
//>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
void pomeriDesno() {
	for (int i = 0; i < 4; i++) {
		int popravkaGreske = 0;
		//============= TRECA KOLONA ===============
		if (matrica[i][2] != 0) {
			if (matrica[i][3] == 0) {
				matrica[i][3] = matrica[i][2];
				matrica[i][2] = 0;
			}
			else if (matrica[i][3] == matrica[i][2]) {
				poeni += matrica[i][3];
				matrica[i][3] = matrica[i][3] + matrica[i][2];
				matrica[i][2] = 0;
				popravkaGreske = 1;
			}
		}
		//============= DRUGA KOLONA ===============
		if (matrica[i][1] != 0) {
			if (matrica[i][3] == 0 && matrica[i][2] == 0) {
				matrica[i][3] = matrica[i][1];
				matrica[i][1] = 0;
			}
			else if (matrica[i][3] != 0 && matrica[i][2] == 0) {
				if (matrica[i][3] == matrica[i][1] && popravkaGreske == 0) {
					poeni += matrica[i][3];
					matrica[i][3] = matrica[i][3] + matrica[i][1];
					matrica[i][1] = 0;
					popravkaGreske = 1;
				}
				else {
					matrica[i][2] = matrica[i][1];
					matrica[i][1] = 0;
				}
			}
			else if (matrica[i][3] != 0 && matrica[i][2] != 0) {
				if (matrica[i][2] == matrica[i][1] && popravkaGreske == 0) {
					poeni += matrica[i][2];
					matrica[i][2] = matrica[i][2] + matrica[i][1];
					matrica[i][1] = 0;
					popravkaGreske = 1;
				}
			}
		}
		//============= PRVA KOLONA ===============
		if (matrica[i][0] != 0) {
			if (matrica[i][3] == 0 && matrica[i][2] == 0 && matrica[i][1] == 0) {
				matrica[i][3] = matrica[i][0];
				matrica[i][0] = 0;
			}
			else if (matrica[i][3] != 0 && matrica[i][2] == 0 && matrica[i][1] == 0) {
				if (matrica[i][3] == matrica[i][0] && popravkaGreske == 0) {
					poeni += matrica[i][3];
					matrica[i][3] = matrica[i][3] + matrica[i][0];
					matrica[i][0] = 0;
				}
				else {
					matrica[i][2] = matrica[i][0];
					matrica[i][0] = 0;
				}
			}
			else if (matrica[i][3] != 0 && matrica[i][2] != 0 && matrica[i][1] == 0) {
				if (matrica[i][2] == matrica[i][0] && popravkaGreske == 0) {
					poeni += matrica[i][2];
					matrica[i][2] = matrica[i][2] + matrica[i][0];
					matrica[i][0] = 0;
				}
				else {
					matrica[i][1] = matrica[i][0];
					matrica[i][0] = 0;
				}
			}
			else if (matrica[i][3] != 0 && matrica[i][2] != 0 && matrica[i][1] != 0) {
				if (matrica[i][1] == matrica[i][0] && popravkaGreske == 0) {
					poeni += matrica[i][1];
					matrica[i][1] = matrica[i][1] + matrica[i][0];
					matrica[i][0] = 0;
				}
			}
		}
	}
	ispisivanjeMatrice();
}
//<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
//============= LEVO ===============
//<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
void pomeriLevo() {
	for (int i = 0; i < 4; i++) {
		int popravkaGreske = 0;
		//============= DRUGA KOLONA ===============
		if (matrica[i][1] != 0) {
			if (matrica[i][0] == 0) {
				matrica[i][0] = matrica[i][1];
				matrica[i][1] = 0;
			}
			else if (matrica[i][0] == matrica[i][1]) {
				poeni += matrica[i][0];
				matrica[i][0] = matrica[i][0] + matrica[i][1];
				matrica[i][1] = 0;
				popravkaGreske = 1;
			}
		}
		//============= TRECA KOLONA ===============
		if (matrica[i][2] != 0) {
			if (matrica[i][0] == 0 && matrica[i][1] == 0) {
				matrica[i][0] = matrica[i][2];
				matrica[i][2] = 0;
			}
			else if (matrica[i][0] != 0 && matrica[i][1] == 0) {
				if (matrica[i][0] == matrica[i][2] && popravkaGreske == 0) {
					poeni += matrica[i][0];
					matrica[i][0] = matrica[i][0] + matrica[i][2];
					matrica[i][2] = 0;
					popravkaGreske = 1;
				}
				else {
					matrica[i][1] = matrica[i][2];
					matrica[i][2] = 0;
				}
			}
			else if (matrica[i][0] != 0 && matrica[i][1] != 0) {
				if (matrica[i][1] == matrica[i][2] && popravkaGreske == 0) {
					poeni += matrica[i][1];
					matrica[i][1] = matrica[i][1] + matrica[i][2];
					matrica[i][2] = 0;
					popravkaGreske = 1;
				}
			}
		}
		//============= CETVRTA KOLONA ===============
		if (matrica[i][3] != 0) {
			if (matrica[i][0] == 0 && matrica[i][1] == 0 && matrica[i][2] == 0) {
				matrica[i][0] = matrica[i][3];
				matrica[i][3] = 0;
			}
			else if (matrica[i][0] != 0 && matrica[i][1] == 0 && matrica[i][2] == 0) {
				if (matrica[i][0] == matrica[i][3] && popravkaGreske == 0) {
					poeni += matrica[i][0];
					matrica[i][0] = matrica[i][0] + matrica[i][3];
					matrica[i][3] = 0;
				}
				else {
					matrica[i][1] = matrica[i][3];
					matrica[i][3] = 0;
				}
			}
			else if (matrica[i][0] != 0 && matrica[i][1] != 0 && matrica[i][2] == 0) {
				if (matrica[i][1] == matrica[i][3] && popravkaGreske == 0) {
					poeni += matrica[i][1];
					matrica[i][1] = matrica[i][1] + matrica[i][3];
					matrica[i][3] = 0;
				}
				else {
					matrica[i][2] = matrica[i][3];
					matrica[i][3] = 0;
				}
			}
			else if (matrica[i][0] != 0 && matrica[i][1] != 0 && matrica[i][2] != 0) {
				if (matrica[i][2] == matrica[i][3] && popravkaGreske == 0) {
					poeni += matrica[i][2];
					matrica[i][2] = matrica[i][2] + matrica[i][3];
					matrica[i][3] = 0;
				}
			}
		}
	}
	ispisivanjeMatrice();
}
// upis u pomocnu matricu zbog provere da li je moguce nastaviti kretanje u istom pravcu
void proveraUpis() {
	for (int i = 0; i < 4; i++) {
		for (int j = 0; j < 4; j++) {
			provera[i][j] = matrica[i][j];
			
		}
	}

}
// citanje iz pomocne matrice zbog provere da li je moguce nastaviti kretanje u istom pravcu
int proveraCitanje() {
	int greska = 0, ispitivanje;
	for (int i = 0; i < 4; i++) {
		for (int j = 0; j < 4; j++) {
			if (provera[i][j] == matrica[i][j])
				greska ++;
		}
	}
	ispitivanje = ispitajGameOver();
	greska += ispitivanje;
	return greska;
}
//resetovanje matrice za pocetak igre
void resetujMatricu() {
	for (int i = 0; i < 4; i++) {
		for (int j = 0; j < 4; j++) {
			matrica[i][j] = 0;
		}
	}
}
//funkcija samo da bi main izgledao lepse i manje komplikovan
//funkcija za novu igru
void zapociniIgru() {
	poeni = 0;
	kaunter = 0;
	resetujMatricu();
	dodajNoviBroj();
	dodajNoviBroj();
	do
	{
		int greska;
		kretanje = mogucaKretanja();
		switch (kretanje)
		{
		case 'w':
			proveraUpis();
			pomeriGore();
			greska = proveraCitanje();
			if (greska == 16) {
				cout << "Ne mozete vise ici gore!" << endl;
				system("pause");
				break;
			}
			if (greska == 17) {
				gameOver();
				kretanje = 'm';
				break;
			}
			dodajNoviBroj();
			ispitajDHCO();
			if (kaunter > 0) {
				gameOver();
				kretanje = 'm';
				break;
			}
			break;
		case 's':
			proveraUpis();
			pomeriDole();
			greska = proveraCitanje();
			if (greska == 16) {
				cout << "Ne mozete vise ici dole!" << endl;
				system("pause");
				break;
			}
			if (greska == 17) {
				gameOver();
				kretanje = 'm';
				break;
			}
			dodajNoviBroj();
			ispitajDHCO();
			if (kaunter > 0) {
				gameOver();
				kretanje = 'm';
				break;
			}
			break;
		case 'd':
			proveraUpis();
			pomeriDesno();
			greska = proveraCitanje();
			if (greska == 16) {
				cout << "Ne mozete vise ici desno!" << endl;
				system("pause");
				break;
			}
			if (greska == 17) {
				gameOver();
				kretanje = 'm';
				break;
			}
			dodajNoviBroj();
			ispitajDHCO();
			if (kaunter > 0) {
				gameOver();
				kretanje = 'm';
				break;
			}
			break;
		case 'a':
			proveraUpis();
			pomeriLevo();
			greska = proveraCitanje();
			if (greska == 16) {
				cout << "Ne mozete vise ici levo!" << endl;
				system("pause");
				break;
			}
			if (greska == 17) {
				gameOver();
				kretanje = 'm';
				break;
			}
			dodajNoviBroj();
			ispitajDHCO();
			if (kaunter > 0) {
				gameOver();
				kretanje = 'm';
				break;
			}
			break;
		case 'm':
			break;
		default:
			break;
		}
	} while (kretanje != 'm');
}
//funkcija za nastavak igre
void nastaviIgru() {
	if (kretanje != 'm') {
		dodajNoviBroj();
	}
	do
	{
		int greska;
		int proveraZaGameOver;
		ispisivanjeMatrice();
		kretanje = mogucaKretanja();
		switch (kretanje)
		{
		case 'w':
			proveraUpis();
			pomeriGore();
			greska = proveraCitanje();
			if (greska == 16) {
				cout << "Ne mozete vise ici gore!" << endl;
				system("pause");
				break;
			}
			if (greska == 17) {
				gameOver();
				kretanje = 'm';
				break;
			}
			dodajNoviBroj();
			ispitajDHCO();
			if (kaunter > 0) {
				gameOver();
				kretanje = 'm';
				break;
			}
			break;
		case 's':
			proveraUpis();
			pomeriDole();
			greska = proveraCitanje();
			if (greska == 16) {
				cout << "Ne mozete vise ici dole!" << endl;
				system("pause");
				break;
			}
			if (greska == 17) {
				gameOver();
				kretanje = 'm';
				break;
			}
			dodajNoviBroj();
			ispitajDHCO();
			if (kaunter > 0) {
				gameOver();
				kretanje = 'm';
				break;
			}
			break;
		case 'd':
			proveraUpis();
			pomeriDesno();
			greska = proveraCitanje();
			if (greska == 16) {
				cout << "Ne mozete vise ici desno!" << endl;
				system("pause");
				break;
			}
			if (greska == 17) {
				gameOver();
				kretanje = 'm';
				break;
			}
			dodajNoviBroj();
			ispitajDHCO();
			if (kaunter > 0) {
				gameOver();
				kretanje = 'm';
				break;
			}
			break;
		case 'a':
			proveraUpis();
			pomeriLevo();
			greska = proveraCitanje();
			if (greska == 16) {
				cout << "Ne mozete vise ici levo!" << endl;
				system("pause");
				break;
			}
			if (greska == 17) {
				gameOver();
				kretanje = 'm';
				break;
			}
			dodajNoviBroj();
			ispitajDHCO();
			if (kaunter > 0) {
				gameOver();
				kretanje = 'm';
				break;
			}
			break;
		case 'm':
			break;
		default:
			break;
		}
	} while (kretanje != 'm');
}
// game over funkcija za unos imena i skora
void gameOver() {
	system("cls");
	cout << "\n\n\t\t\tZavrsili ste igru!" << endl;
	fin.open("najboljiSkor.txt", ios::in);
	if (!fin) {
		cerr << "Greska pri otvaranju datoteke" << endl;
		exit(1);
	}
	while (fin >> s.ime >> s.skor) {
		if (s.skor < poeni) {
			fout.open("najboljiSkor.txt", ios::out);
			if (!fout) {
				cerr << "Greska pri otvaranju datoteke" << endl;
				exit(1);
			}
			cout << "Unesite vase ime:" << endl;
			cin >> s.ime;
			fout << s.ime << " " << poeni;
			fout.close();
		}
		else {
			cout << "Nazalost niste pobedili najboljeg, najbolji je " << s.ime << ", i njegov skor je " << s.skor << endl;
		}
	}
	fin.close();
	
	system("pause");
}
// ispitivanje da li se barem do nekog broja nalazi isti, sto znaci ako postoji, nije kraj, ako ne postoji, jeste kraj
int ispitajGameOver() {
	int i, j, pomocna[4][4];
	for (i = 0; i < 4; i++) {
		for (j = 0; j < 3; j++) {
			if (matrica[i][j] == matrica[i][j + 1]) {
				return 0;
			}
		}
	}
	for (i = 0; i < 4; i++) {
		for (j = 0; j < 4; j++) {
			pomocna[j][i] = matrica[i][j];
		}
	}
	for (i = 0; i < 4; i++) {
		for (j = 0; j < 3; j++) {
			if (pomocna[i][j] == pomocna[i][j + 1]) {
				return 0;
			}
		}
	}
	return 1;
}
// provera da li ste dosli do broja 2048, ako jeste, pita za nastavak ili kraj
int ispitajDHCO() {
	int i, j;
	if (kaunter == 0) {
		for (i = 0; i < 4; i++) {
			for (j = 0; j < 4; j++) {
				if (matrica[i][j] == 2048) {
					char izbor;
					cout << "Cestitamo, dosli ste do 2048, mozete da nastavite igru, ili da je sada zavrsite" << endl;
					cout << "Sa \"d\" (da) ili \"n\" (ne) odlucite da li zelite da nastavite" << endl;
					do {
						cin >> izbor;
						if (izbor == 'd' || izbor == 'D') {
							izbor = tolower(izbor);
							kaunter -= 1;
							return 1;
						}
						else if (izbor == 'n' || izbor == 'N') {
							izbor = tolower(izbor);
							kaunter += 1;
							return 0;
						}
						else {
							cout << "Niste uneli dobar izbor" << endl;
						}
					} while (1);
				}
			}
		}
	}
}
//ispis najboljeg igraca
void najboljiSkor() {
	system("cls");
	fin.open("najboljiSkor.txt", ios::in);
	if (!fin) {
		cerr << "Greska pri otvaranju datoteke" << endl;
		exit(1);
	}
	while (fin >> s.ime >> s.skor) {
		cout << "\n\n\n\t\t\t\t\tNajbolji igrac je " << s.ime << ", i njegov skor je " << s.skor << endl << endl << endl << endl << endl << endl;
	}
	fin.close();
	system("pause");
}
// dinamicka dodela memorije
void oIgri() {
	system("cls");
	char *recenica;
	recenica = new char[81];
	fout.open("oIgri.txt", ios::out);
	if (!fout) {
		cerr << "Greska pri otvaranju datoteke" << endl;
		exit(1);
	}
	cout << "\n\n\n\t\tRecite nam u jednoj recenici kako Vam se svidela igra, ili sta bismo mogli da dodamo u nju." << endl;
	cin >> recenica;
	fout << recenica;
	cout << "\t\t\t\tHvala Vam na podrsci, ovo cemo proslediti nasim strucnjacima." << endl;
	// slanje na mail, zbog toga sto ovo ide u javnost ne zelim da otkrijem svoj mail
	system("pause");
	delete recenica;
	fout.close();
}
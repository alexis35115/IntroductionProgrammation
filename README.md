# Erreurs fréquentes en C++

## Description

Ce guide ce veut d'être une piste de solutions pour ceux qui commence en C++. Les erreurs les plus fréquentes seront expliquées.

### Il ne faut pas mettre de ";" à la fin d'une instruction if

```c++
bool estMajeur = false;

if(estMajeur == true); // Il ne faut jamais mettre de ";" en fin de ligne sur une instruction "if"
{
    // Code ici
}
```

### Problème lors de la récupération de données avec scanf_s

Il ne faut jamais oublier de préfixer les variables avec "&" dans l'opération **scanf_s**. Il faut également employer la bonne letttre selon le type de données que nous voulons récupérer. Par exemple :

- Pour un float : "%f"
- Pour un int : "%d"
- Pour un char : "%c"

```c++
int main()
{
    int chiffre = 0;

    printf("Veuillez saisir un chiffre.");
    scanf_s("%d", &chiffre); // Le "%d" est pour le type int
    while(getchar() != '\n');

    system("pause>nul"); // requis pour empêcher la fermeture automatique de la fenêtre
    return 0;
}
```

### Ne pas vider le buffer suite l'instruction scanf_s

Il est impératif de toujours vider le buffer suite à une instruction scanf_s de la façon suivante : while(getchar() != '\n');

En cas d'oublie, le programme se fermera automatiquement.

En erreur :

```c++
int main()
{
    int chiffre = 0;

    printf("Veuillez saisir un chiffre.");
    scanf_s("%d", &chiffre);

    system("pause>nul"); // Le programme se fermera vu que le buffer contient encore des valeurs
    return 0;
}
```

Bonne façon :

```c++
int main()
{
    int chiffre = 0;

    printf("Veuillez saisir un chiffre.");
    scanf_s("%d", &chiffre);
    while(getchar() != '\n');

    system("pause>nul"); // requis pour empêcher la fermeture automatique de la fenêtre
    return 0;
}
```

### Nuance entre une affectation et une comparaison

Lors de l'affectation d'une valeur, on utilise un seul égale "=", tandis qu'une comparaison s'effectue avec deux égales "==".

```c++
int main()
{
    // Un seul égale lors d'une affectation
    bool estMajeur = false;

    // Deux égales lors d'une comparaison
    if(estMajeur == true)
    {
        // Insérer code ici
    }

    system("pause>nul");
    return 0;
}
```

### Rappel sur l'utilisation de "*" et du "&"

Tout d'abord, je vous invite à relire vos notes de cours "Suites sur les fonctions - les pointeurs" sur Léa.

Voici un résumé 101 : On met une étoile - "*" à un paramètre d'une fonction lorsque l'on veut donner accès à la fonction de travailler avec le contenu d'une variable et non seulement donner accès à sa valeur. Le fait de mettre une étoile revient à dire que l'on crée une variable de type pointeur qui va contenir l'adresse de la variable. Et lorsque qu'une fonction possède des paramètres avec des étoiles, il faut mettre "&" devant les paramètres lors de l'appelle.

Voici un exemple concret :

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h> // Utilisé pour strcpy_s

void afficherInformation(char[], int*);
void vieillirUnAnUtilisateur(int*);

int main()
{
	char prenom[7];
	strcpy_s(prenom, "Alexis");
	int age = 28;

	afficherInformation(prenom, &age); // <- le "&" est nécessaire

	// Afficher l'âge à la fin du traitement
	printf("L'âge à la fin du traitement est : %d.\n", age);
	// AFFICHE : L'âge à la fin du traitement est : 29.

	// Faire en sorte que la console reste ouverte
	system("pause>nul");

	return 0;
}

void afficherInformation(char prenom[], int* age)
{
	// Afficher les informations de l'utilisateur
	printf("Je suis %s et j'ai %d ans.\n", prenom, *age);
	// AFFICHE : Je suis Alexis et j'ai 28 ans.

	vieillirUnAnUtilisateur(&*age); // <- le "&" est nécessaire vu que le paramètre "age" de la méthode "vieillirUtilisateur" 
	// est préfixé d'un "*"

	// Afficher les informations de l'utilisateur de nouveau après son vieillissement
	printf("Je suis %s et j'ai %d ans après avoir vieillit d'un an.\n", prenom, *age);
	// AFFICHE : Je suis Alexis et j'ai 29 ans après avoir vieillit d'un an.
}

void vieillirUnAnUtilisateur(int* age)
{
	*age = *age + 1;
}

```

### Erreurs fréquentes

#### Les prototypes ne sont pas à jour ou absents

Lors de la création et/ou la mise à jour d'une fonction, il ne faut pas oublier d'ajouter et/ou mettre à jour le prototype de la méthode dans le haut du fichier source.

Voici les erreurs lors de la compilation :

- [C2660](https://docs.microsoft.com/fr-fr/cpp/error-messages/compiler-errors-2/compiler-error-c2660?f1url=https%3A%2F%2Fmsdn.microsoft.com%2Fquery%2Fdev16.query%3FappId%3DDev16IDEF1%26l%3DFR-FR%26k%3Dk(C2660)%26rd%3Dtrue&view=vs-2019) - 'XXX' : la fonction ne prend pas X arguments = La fonction est appelée avec un nombre incorrect de paramètres.
- [C3861](https://docs.microsoft.com/fr-fr/cpp/error-messages/compiler-errors-2/compiler-error-c3861?f1url=https%3A%2F%2Fmsdn.microsoft.com%2Fquery%2Fdev16.query%3FappId%3DDev16IDEF1%26l%3DFR-FR%26k%3Dk(C3861)%26rd%3Dtrue&view=vs-2019) - 'XXX' : identificateur introuvable = Le prototype est absent.

Voici quelques exemples :

```c
#include <stdio.h>
#include <stdlib.h>

void afficherMessageBienvenue();

int main()
{
	afficherMessageBienvenue();

	// Faire en sorte que la console reste ouverte
	system("pause>nul");

	return 0;
}

void afficherMessageBienvenue()
{
	printf_s("Bienvenue dans une méthode sans arguments.\n");
}
```

```c
#include <stdio.h>
#include <stdlib.h>

int calculer(int nombre1, int nombre2);

int main()
{
	int somme = calculer(1, 2);

	printf_s("Voici la valeur de la somme : %d", somme);

	// Faire en sorte que la console reste ouverte
	system("pause>nul");

	return 0;
}

int calculer(int nombre1, int nombre2)
{
	return nombre1 + nombre2;
}
```

Remarque : Il n'est pas nécessaire de mettre [le nom des paramètres dans le prototype](https://stackoverflow.com/a/39224809), se référer à l'exemple ici-bas :

```c
#include <stdio.h>
#include <stdlib.h>

int calculer(int, int);

int main()
{
	int somme = calculer(1, 2);

	printf_s("Voici la valeur de la somme : %d", somme);

	// Faire en sorte que la console reste ouverte
	system("pause>nul");

	return 0;
}

int calculer(int nombre1, int nombre2)
{
	return nombre1 + nombre2;
}
```

#### L'ordre des paramètres est incorrect lors d'un appel d'une fonction

Il est important de __respecter l'ordre des paramètres__ d'une fonction lors de son appel.

Voici un exemple que l'on veut effectuer la soustraction "10 - 5" et que le résultat est "5":

```c
#include <stdio.h>
#include <stdlib.h>

int soustaireDeuxNombres(int nombre1, int nombre2);

int main()
{
	int nombre1 = 10;
	int nombre2 = 5;

    // Il est important de passer les paramètres dans le bon ordre
	int soustraction = soustaireDeuxNombres(nombre1, nombre2);

	printf_s("Voici la valeur de la soustraction : %d", soustraction);

	// Faire en sorte que la console reste ouverte
	system("pause>nul");

	return 0;
}

int soustaireDeuxNombres(int nombre1, int nombre2)
{
	return nombre1 + nombre2;
}
```

#### Le type d'une fonction n'est pas correct

Comme les variables, les fonctions ont un type. Ce type dépend du résultat que la fonction renvoie : si la fonction renvoie un nombre décimal, vous mettrez sûrement double, si elle renvoie un entier vous mettrez "int" ou "long" par exemple. Mais il est aussi possible de créer des fonctions qui ne renvoient rien.

Il y a donc deux sortes de fonctions :

les fonctions qui renvoient une valeur : on leur met un des types que l'on connaît : "char", "int", "double", etc.) ;

les fonctions qui ne renvoient pas de valeur : on leur met un type spécial "void".

Par exemple, la fonction ici-bas est de type "int" :

```c
int soustaireDeuxNombres(int nombre1, int nombre2)
{
	return nombre1 + nombre2;
}
```

#### Mauvaises utilisations avec les structures

Notez que pour éviter des erreurs avec les prototypes, les structures doivent être écrites avant les prototypes.

Il ne faut pas oublier d'initialiser les variables membres avant d’envoyer une structure dans une fonction.

Exemple d'une structure :

```c
struct Etudiant
{
   char nom[35],
        prenom[35],
        adresse[35];

   int note;
}; // <-ne pas oublier le ";" à la fin
```

Voici un exemple complexe :

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h> // Utilisé pour strcpy_s

struct Etudiant
{
	char nom[35],
		 prenom[35],
	 	 adresse[35];

	int note;
}; // <- Ne pas oublier le ";" à la fin

void modifierInformationsEtudiant(struct Etudiant*);
void afficherInformationsEtudiant(struct Etudiant);

int main()
{
	// Déclaration de la structure Étudiant
	struct Etudiant etudiant;

	// Initialiser les propriétés de la structure avec des valeurs par défaut
	strcpy_s(etudiant.nom, "");
	strcpy_s(etudiant.prenom, "");
	strcpy_s(etudiant.adresse, "");
	etudiant.note = 0;

	modifierInformationsEtudiant(&etudiant); // <- Ne pas oublier le "&" pour
	//passer l'adresse de la variable
	afficherInformationsEtudiant(etudiant);

	// Faire en sorte que la console reste ouverte
	system("pause>nul");

	return 0;
}

void modifierInformationsEtudiant(struct Etudiant* etudiant) // <- "*" pour indiquer que 
// l'on veut travailler avec des points pour être en mesure de modifier "l'étudiant"
{
	strcpy_s((*etudiant).adresse, "150 des ursulines");
	strcpy_s((*etudiant).nom, "Bourgoin");
	strcpy_s((*etudiant).prenom, "Ken");

	(*etudiant).note = 100;
}

void afficherInformationsEtudiant(struct Etudiant etudiant)
{
	printf("Bonjour, je m'appelle %s %s, j'habite au %s et j'ai obtenu la note %d",
		etudiant.prenom,
		etudiant.nom,
		etudiant.adresse,
		etudiant.note);
}
```

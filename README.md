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

    getchar(); // requis pour empêcher la fermeture automatique de la fenêtre
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

    getchar(); // Le programme se fermera vu que le buffer contient encore des valeurs
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

    getchar(); // requis pour empêcher la fermeture automatique de la fenêtre
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

    getchar();
    return 0;
}
```

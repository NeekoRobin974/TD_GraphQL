## TD GraphQL
CADET Mattéo - DIEUDONNE Quentin

## Requêtes Query
1) Liste des praticiens en précisant : id, nom, prénom, téléphone, ville  
```graphql
query {
    Praticien {
        id
        nom
        prenom
        telephone
    }
}
```
![Résultat requête 1](./Images_Requetes/requete_1.png)

2) Idem, en ajoutant le libellé de la spécialité du praticien
```graphql
query {
    Praticien {
        id
        nom
        prenom
        telephone
        specialite_id {
            libelle
        }
    }
}
```
![Résultat requête 2](./Images_Requetes/requete_2.png)

3) Idem, en ajoutant un filtre pour sélectionner les praticiens dont la ville  est égale à «Paris» (ou une valeur de votre jeu de données)
```graphql
query {
    Praticien (filter: { ville: {_eq : "Paris"}}) {
        id
        nom
        prenom
        telephone
        specialite_id {
            libelle
        }
    }
}
```
![Résultat requête 3](./Images_Requetes/requete_3.png)

4) Idem, en ajoutant le nom et la ville de la structure d’appartenance du praticien,
```graphql
query {
    Praticien (filter: { ville: {_eq : "Paris"}}) {
        id
        nom
        prenom
        telephone
        specialite_id {
            libelle
        }
        structure_id {
            nom
            ville
        }
    }
}
```
![Résultat requête 4](./Images_Requetes/requete_4.png)

5) Idem, en ajoutant un filtre pour retenir les emails contenant ".fr"
```graphql
query {
    Praticien (filter: {
            ville: { _eq: "Paris" },
            email: { _contains: ".fr" }
        }) {
        id
        nom
        prenom
        telephone
        email
        specialite_id {
            libelle
        }
        structure_id {
            nom
            ville
        }
    }
}
```

7) Requête retournant une liste de praticiens installés à Paris et une liste de praticiens installés à Bourdon-les-Bains ; utiliser des alias

```graphql
query {
PraticiensParis: Praticien(filter: { ville: { _eq: "Paris" } }) {
    id
    nom
    prenom
    ville
    specialite_id {
    libelle
    }
}

    PraticiensBourdon: Praticien(filter: { ville: { _eq: "Bourdon-les-Bains" } }) {
        id
        nom
        prenom
        ville
        specialite_id {
            libelle
        }
    }
}
```
![Résultat requête 7](./Images_Requetes/requete_7-2.png)
![Résultat requête 7](./Images_Requetes/requete_7-1.png)
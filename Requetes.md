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
![Résultat requête 5](./Images_Requetes/requete_5.png)

6) Lister les praticiens rattachés à une structure dont la ville est "Paris".
```graphql
query {
    Praticien (filter: { structure_id: { ville: { _eq: "Paris" } } }) {
        id
        nom
        prenom
        telephone
        structure_id {
            nom
            ville
        }
    }
}
```
![Résultat requête 6](./Images_Requetes/requete_6.png)


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

8)Transformer la requête précédente de façon à utiliser un fragment correspondant aux champs
du résultat.
```graphql
fragment champsPraticien on Praticien {
        id
        nom
        prenom
        ville
        specialite_id {
            libelle
        }
    }

query {
    PraticiensParis: Praticien(filter: { ville: {_eq: "Paris"} }){
    ...champsPraticien
    }
    PraticiensBourdon: Praticien(filter: { ville: {_eq: "Bourdon-les-Bains"} }){
    ...champsPraticien
    }
}
```
![Résultat requête 8](./Images_Requetes/requete_8.png)



9)Transformer la requête 3 pour utiliser une variable de façon à paramétrer la requête par le
nom de la ville souhaitée.
```graphql
query Requete9($ville: String!) {
    Praticien(filter: { ville: { _eq: $ville } }) {
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
![Résultat requête 9](./Images_Requetes/requete_9.png)

10) Liste des structures, en indiquant leur nom et ville,  en ajoutant la liste des praticiens attachés à chaque structure en indiquant leur nom, prénom, email ainsi que le libellé de leur spécialité.

## Autorisations dans Directus
Sur Postman, les tables MoyenPaiement et MotifVisite disparaissent du schema si on met NoAuth.

1) Lister les moyens de paiement (utilisateur utilisant un token statique)
```graphql
query MoyenPaiement {
    MoyenPaiement {
        id
        libelle
    }
}
```
![Résultat requête 2.1](./Images_Requetes/requete_2.1.png)
Si on se met en NoAuth :
![Résultat requête 2.1 NoAuth](./Images_Requetes/requete_2.1_NoAuth.png)


2) Lister les spécialités en indiquant les motifs de visite associés à chacune (utilisateur utilisant un token JWT)

Il faut d'abord se connecter et récupérer le token :
![Récupération du token](./Images_Requetes/recup_token.png)
Ensuite on peut faire la requête en précisant le token dans Authorization.
```graphql
query {
    Specialite {
        id
        libelle
        motifs {
            id
            libelle
        }
    }
}
```
![Résultat requête 2.2 NoAuth](./Images_Requetes/requete_2.2.png)


## Migrations:
1) Créer la specialité «cardiologie»

```
mutation CreateSpecialite {
  create_Specialite_item(data: { libelle: "cardiologie" }) {
    id
    libelle
  }
}
```
![Résultat mutation1](./Images_Requetes/Mutation_1.png)

2) Créer un praticien: nom, prénom, ville, email, téléphone

```
mutation CreatePraticien {
  create_Praticien_item(data: { nom: "cardiologie", prenom: "Jean", ville: "Paris", email: "Jean@test.com", telephone: "0610101010" }) {
    id
    nom
    prenom
    ville
    email
    telephone
  }
}
```
![Résultat mutation2](./Images_Requetes/Mutation_2.png)
3) Modifier le praticien pour le rattacher à la spécialité «cardiologie»

```
mutation ModifierPraticien {
  update_Praticien_item(id:"ddf74994-7606-4de3-8306-4a943f6ba703", data: { specialite_id: {id:"6"}}) {
    id
    nom
    prenom
    ville
    email
    telephone
    specialite_id{
        libelle
    }
  }
}
```
![Résultat mutation3](./Images_Requetes/Mutation_3.png)

4) Créer un praticien en le rattachant à la spécialité «cardiologie»
```
mutation CreatePraticien {
    create_Praticien_item(data: { nom: "Wick", prenom: "John", ville: "Paris", email: "John@test.com", telephone: "0620202020", specialite_id: {id:6} }) {
        id
        nom
        prenom
        ville
        email
        telephone
        specialite_id{
            libelle
        }
    }
}
```
![Résultat mutation4](./Images_Requetes/Mutation_4.png)

5) Créer un praticien et créer en même temps sa spécialité «chirurgie»
```graphql
mutation CreatePraticienEtSpecialite {
    create_Praticien_item(data: {
        nom: "Grey",
        prenom: "Meredith",
        ville: "Seattle",
        email: "meredith@test.com",
        telephone: "0600000001",
        specialite_id: {
            libelle: "chirurgie"
        }
    }) {
        id
        nom
        prenom
        specialite_id {
            id
            libelle
        }
    }
} 
```
![Résultat mutation5](./Images_Requetes/Mutation_5.png)

6) Ajouter un praticien à la spécialité «chirurgie»
```graphql
```
![Résultat mutation6](./Images_Requetes/Mutation_6.png)

7) Modifier le premier praticien créé pour le rattacher à une structure existante
```graphql
```
![Résultat mutation7](./Images_Requetes/Mutation_7.png)

8) Supprimer les deux derniers praticiens créés.
```graphql
```
![Résultat mutation8](./Images_Requetes/Mutation_8.png)
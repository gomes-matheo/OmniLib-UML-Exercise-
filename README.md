# Résumé du Projet OmniLib
OmniLib est un projet visant à créer un portail qui répertorie des livres physiques et virtuels, en les proposant parmi un catalogue. En plus du visiteur qui ne bénéficie d'aucun service, le projet comprends plusieurs offres (adhérent standard qui paie 5€ par mois pour empruter des livres physiques en ayant la possibilité de se les faire livrer, ainsi que l'adhérent premium qui paie 10€ par mois pour pouvoir emprunter des livres physiques, vod, ebooks). L'utilisateur ne peut pas bénéficier des services précédants s'il fait l'objet d'une pénalité active. Les bibliothécaires peuvent altérer le catalogue via leur propre compte.


Conformément à la demande de notre client, Michel L.E, nous avons établi les diagrammes suivants afin de récapituler les étapes principales de l'application et son architecture global. Nous avons aussi inclus un diagramme expliquant globalement le cas de la réservation d'un livre.

## DIAGRAMME DE CAS D'UTILISATION
![use-cases-diagram](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/gomes-matheo/OmniLib-UML-Exercise-/main/Diagramme_Cas_Utilisation.iuml)


## DIAGRAMME D'ACTIVITE (Réservation d'un livre)
```mermaid
flowchart TD
    Start[Entrée du programme]
    Start --> A

    A[Choisir un livre physique]
    A --> B

    B{Réserver ce livre ?}
    B --> | non | A
    B --> | oui | C

    C{Est adhérent ?}
    C1[[Se connecter]]
    C --> | non | D
    C --> | oui | C1
    C1 --> F

    D{Créer un compte ?}
    D --> | non | A
    D --> | oui | E

    E[[création du compte]]
    E --> G

    F{Pénalité en cours ?}
    F1[Refuser l'emprunt car pénalité]
    F --> | non | G
    F --> | oui | F1
    F1 --> END

    G{Se faire livrer ?}
    G1[Le livre est disponible et réservé en librairie]
    G2[Procéder au paiement]
    G --> | non | G1
    G1 --> END
    G --> | oui | G2
    G2 --> H

    H{Paiment accepté ?}
    H1[Afficher un reçu]
    H1_1[Envoyer le bon de livraison]
    H2[Afficher un refus]

    H --> | oui | H1
    H1 --> H1_1 
    H1_1 --> END
    H --> | non | H2
    H2 --> END

    END([Fin du programme])
```


## DIAGRAMME DE CLASSE
```mermaid
classDiagram

        class Bibliotheque {
        - List~Livres_Physiques~ livres
        - List~Ebook~ ebooks
        - List~Vod~ vods

        + seConnecter(String adresseMail, String mdp)

        + chercherDocument()

        + reserverLivrePhysique(Livre livre, Utilisateur utilisateur)
        + livrerADomicile()

        + telechargementEbook(Ebook ebook, Utilisateur utilisateur)
        + locationVod(Vod vod, Utilisateur utilisateur)

        + verifierDureeEmprunt(Livre livre, Utilisateur utilisateur)
        + verifierPenalitesRetard(Utilisateur utilisateur)

    }

    class Document {
        <<abstract>>
        + String auteur
        + String titre
        + int anneeEdition
    }

    class Livres_Physiques {
        + String codeBarre
        + String rayonSpecifique
        + String etatUsure
    }

    class Ebook {
        + int poids
        + String format
    }

    class Vod {
        + int duree
        + String resolution

    }

    class Utilisateur {
        <<abstract>>
        + String nom
        + String prenom
        + String adresseMail
        + String mdp

        + verifierAbonnement() boolean
        + accesLivre() boolean
        + accesEbook() boolean
        + accesVod() boolean
    }


    class AdherentStandard {
        + List~Livres_Physiques~ livres
        + verifierAbonnement() boolean
    }

    class AdherentPremium {
        + List~Ebooks~ ebooks
        + List~Vod~ vods
        + accesEbook() boolean
        + accesVod() boolean

        + verifierAbonnement boolean
    }

    class Bibliothecaire {
        +accesVerification() boolean
    }

    Utilisateur <|-- AdherentStandard : est un
    Utilisateur <|-- AdherentPremium : est un
    Utilisateur <|-- Bibliothecaire : est un
    Document <|-- Livres_Physiques
    Document <|-- Ebook
    Document <|-- Vod
    Bibliotheque <.. Utilisateur : dépendance
    Document *-- Bibliotheque : composition

    AdherentPremium --> Ebook : navigabilité
    AdherentPremium --> Vod : navigabilité
    AdherentStandard --> Livres_Physiques : navigabilité
```


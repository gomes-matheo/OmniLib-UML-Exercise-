# OmniLib-UML-Exercise-

UML exercise (usecase, activity diagram, class diagram)

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

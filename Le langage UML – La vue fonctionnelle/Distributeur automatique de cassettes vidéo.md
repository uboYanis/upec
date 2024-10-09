# TD 1 - Le langage UML – La vue fonctionnelle

## Diagramme de cas d’utilisation

Pour visualiser le diagramme, veuillez utiliser le site 

```puml
@startuml
left to right direction
skinparam packageStyle rect

actor Client
actor Magasin as "Personnel du magasin"

rectangle "Systeme Distributeur" {
usecase "Introduire la carte" as UC_IntroduireCarte
usecase "Vérifier le crédit" as UC_VerifierCredit
usecase "Choisir une cassette" as UC_ChoisirCassette
usecase "Récupérer la cassette" as UC_RecupererCassette
usecase "Rendre la cassette" as UC_RendreCassette
usecase "Débiter le montant" as UC_DebiterMontant
usecase "Recharger la carte" as UC_RechargerCarte

Client --> UC_IntroduireCarte
Client --> UC_VerifierCredit
Client --> UC_ChoisirCassette
Client --> UC_RecupererCassette
Client --> UC_RendreCassette
UC_RendreCassette --> UC_DebiterMontant
UC_DebiterMontant --> UC_VerifierCredit: "Montant > Crédit"
UC_VerifierCredit --> UC_RechargerCarte: "Insuffisant"
}

rectangle "Systeme Gestion" {
usecase "Gérer les comptes débiteurs" as UC_GererComptesDebiteurs
usecase "Gérer les stocks" as UC_GererStocks

Magasin --> UC_GererComptesDebiteurs
Magasin --> UC_GererStocks
}

@enduml
```

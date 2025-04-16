# Projet_Entreprise
Ceci est un projet orienter base de données SQL SERVER, concernant la gestion complète d une Entreprise. On as implémenter des systèmes des triggers, systèmes d authentification …

# Gestion d'entreprise - Projet SQL Server

## 📌 Description

Ce projet est une base de données SQL Server permettant de gérer une entreprise. Il inclut la gestion des employés, des clients, des produits, des ventes, et d'autres opérations essentielles au fonctionnement d'une structure professionnelle.

Le projet a été conçu à des fins pédagogiques et peut servir de point de départ pour des solutions de gestion plus complexes.

## 🧱 Fonctionnalités principales

- Gestion des employés (ajout, modification, suppression)
- Suivi des Employer par Projet
- Gestion des Departements 
- Suivi des Employer par Departement 
- Intégrité des données via des relations entre tables (clés primaires et étrangères)

## 🗂️ Structure du projet

Le dépôt contient :

- `script_creation.sql` : script pour créer toutes les tables de la base de données.
- `insertion_donnees.sql` : script pour insérer des données d'exemple.
- `requêtes.sql` : exemples de requêtes pour exploiter la base de données.

## ▶️ Prérequis

- SQL Server installé (version 2019 ou supérieure recommandée)
- SQL Server Management Studio (SSMS)

## 🚀 Installation

1. Cloner ce dépôt :

```bash
git clone https://github.com/NgomaMarc/Projet_Entreprise.git



scripts :


-- Création de la base de Données
CREATE DATABASE ENTREPRISE;

use ENTREPRISE;

-- Création des Tables

create table Departement (
DepartementID int identity (1,1),
NomDepartement varchar (100) not null,
constraint pkDepartement primary key (DepartementID));

create table Emploiyer (
EmploiyerID int identity (1,1),
Nom varchar (100) not null,
Prenom varchar (100) not null,
Adresse varchar (200),
Telephone varchar (20),
DepartementID int,
constraint pkEmploiyer primary key (EmploiyerID),
constraint fkEmploiyerDepartement foreign key (DepartementID) references Departement (DepartementID));

create table Projet (
ProjetID int identity (1,1),
NomProjet varchar (100) not null,
DateDebut DATE,
DateFin DATE);

Alter table Projet
add constraint pkProjet primary key (ProjetID);

select * from Projet;

create table Emploiyer_Projet (
Emploiyer_Projet_ID int identity (1,1),
EmploiyerID int,
ProjetID int,
DateAffectation DATE,
constraint pkEmploiyer_Projet primary key (Emploiyer_Projet_ID),
constraint fkEmploiyer_Projet_Emploiyer foreign key (EmploiyerID) references Emploiyer (EmploiyerID),
constraint fkEmploiyer_Projet_Projet foreign key (ProjetID) references Projet (ProjetID));

-- Insérer les Données initiales

insert into Departement values 
('Soudeur'),
('Menuisier'),
('Opérateur de Gru'),
('Plombier'),
('Electricien'),
('Vitrier'),
('Charpentier Bois'),
('Ouvrier du Batiment'),
('Peintre'),
('Mécanicien Industriel'),
('Chauffeur de Camion 10 roues'),
('Conducteur d Engin');

-- Procedure stockées , Ajout d un emploiyer

create PROCEDURE AjouterEmploiyer
@Nom varchar (100),
@Prenom varchar (100),
@Adresse varchar (200),
@Telephone varchar (20),
@DepartementID int
AS
BEGIN
INSERT INTO 
Emploiyer (Nom,Prenom, Adresse, Telephone, DepartementID)
values (@Nom,@Prenom,@Adresse,@Telephone,@DepartementID);
END;

-- Utilisation

EXEC AjouterEmploiyer
@Nom = 'VANGUI',
@Prenom = 'Alicia',
@Adresse= '64 Rue BACONGO Plateau',
@Telephone= '068808080',
@DepartementID= 1;

-- Syntaxe pour modifier la table prenom de jaol en Joal avec la Syntaxe UPDATE.

Update Emploiyer
set Prenom='Joal' where EmploiyerID=1;

EXEC AjouterEmploiyer
@Nom = 'Vangui',
@Prenom = 'Yvon',
@Adresse= '40 Rue Alexandrie',
@Telephone= '066301054',
@DepartementID= 9;

INSERT INTO Emploiyer
values 
('Samuel','Jules', '19 rue balo','065141820',1),
('Mila', 'Fila', '17 rue bacongo','065141824',2),
('Megumi', 'Fifi', '28 rue plateau','065551820',3),
('Takeshi', 'jone', '11 rue talangai','065251820',4),
('Rita', 'Alice', '28 rue djouer','065555620',5),
('Tomas','Alfredson','17 rue djouer','065651820',6),
('Paul','Anderson','27 rue DODO','065654820',7),
('Wes','Anderson','57 rue Bax','065651860',8),
('Richard','Ayoade','27 rue baba','065658080',9),
('Luc','Besson','60 rue Kaba','068658088',10);

INSERT INTO Emploiyer
values 
('James','Cameron','66 rue balou','068659088',1),
('Guillermo','del Toro','09 rue Kiba','068658090',2),
('Victor','Fleming','07 rue baka','069998088',3),
('Francis','Ford Coppola','60 rue Kaba','069979889',4),
('Kinji','Fukasaku','24 rue ABAS','067773337',5),
('Florian','Henckel von Donnersmarck','28 rue koka','065555059',6),
('Terry','Jones','02 rue Mbaka','066220221',7),
('Stanley','kubrick','05 rue Mbochis','066445456',8),
('John','Lasseter','32 rue Chinois','068009009',9),
('Ang','Lee','189 rue TOTO','069121321',10);


INSERT INTO Emploiyer
values 
('Bruce','Lee','200 rue Jolie','069806776',10),
('George','Lucas','59 rue Amper','069121120',11),
('Martin','McDonagh','169 rue Berthelos','069121203',12),
('James','McTeigne','190 rue Baja','069017170',11),
('Fernando','Meireilles','202 rue DJABA','069026565',12),
('Hayao','Miyazaki','200 rue GAdjo','069315565',11),
('Paulo','Morelli','24 rue DJOGO','069455456',12),
('Chan-wook','Park','143 rue Paul','069161565',11),
('Sam','Raimi','202 rue DJABO','069021818',12),
('Mark','Romanek','101 rue DJA','069344543',11);

select * from Departement;
select * from Emploiyer;

-- lister tout les employer avec leur departement

Select e.EmploiyerID, e.Nom, e.Prenom, d.NomDepartement
FROM Emploiyer e
INNER JOIN Departement d ON e.DepartementID=d.DepartementID;

--Lister les projets par departements




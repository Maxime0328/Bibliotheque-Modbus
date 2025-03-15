# Modbus-Library
## Bibliothèque Modbus TCP et RTU

Cette bibliothèque fournit une implémentation complète des protocoles Modbus TCP et RTU en C#. Elle permet de communiquer avec des appareils Modbus via TCP/IP ou une communication série (RTU). La bibliothèque prend en charge la lecture et l'écriture de coils, d'entrées discrètes, de registres de maintien (holding registers) et de registres d'entrée (input registers). Elle est conçue pour être simple d'utilisation, robuste et adaptée à des environnements industriels.

## Table des matières
- [Fonctionnalités](#fonctionnalités)
- [Installation](#installation)
- [Utilisation](#utilisation)
  - [Modbus TCP](#modbus-tcp)
  - [Modbus RTU](#modbus-rtu)
- [Exemples](#exemples)
  - [Lire des coils](#lire-des-coils)
  - [Écrire des registres](#écrire-des-registres)
  - [Lecture/Écriture multiple de registres](#lectureécriture-multiple-de-registres)
- [Fonctionnalités Modbus prises en charge](#fonctionnalités-modbus-prises-en-charge)
- [Contribuer](#contribuer)
- [Licence](#licence)

## Fonctionnalités

### Support Modbus TCP :
- Connexion à des appareils Modbus TCP.
- Lecture et écriture de coils, d'entrées discrètes, de registres de maintien et de registres d'entrée.
- Prise en charge des opérations de lecture/écriture simples et multiples.
- Gestion des erreurs et validation des réponses.

### Support Modbus RTU :
- Connexion à des appareils Modbus RTU via une communication série.
- Lecture et écriture de coils, d'entrées discrètes, de registres de maintien et de registres d'entrée.
- Calcul et validation automatiques du CRC pour chaque transaction.
- Gestion des erreurs et validation des réponses.

### Multiplateforme :
- Compatible avec tout environnement .NET (Core, Framework, etc.).
- Facile à intégrer dans des applications existantes.

## Installation

1. Clonez le dépôt :
    ```bash
    git clone https://github.com/Maxime0328/Modbus-Library.git
    ```

2. Ajoutez la bibliothèque à votre projet :
    - Incluez les namespaces `ModbusTCPLibrary` et `ModbusRTULibrary` dans votre projet.

3. Compilez le projet :
    - Utilisez votre IDE préféré (par exemple, Visual Studio) pour compiler la solution.

## Utilisation

### Modbus TCP
Pour utiliser la bibliothèque Modbus TCP, suivez ces étapes :
1. Créez une instance de la classe `ModbusTCP`.
2. Connectez-vous à l'appareil Modbus TCP en spécifiant l'adresse IP et le port.
3. Effectuez des opérations de lecture/écriture, comme la lecture de coils ou l'écriture de registres.
4. Déconnectez-vous une fois terminé.

#### Exemple :
```csharp
using ModbusTCPLibrary;

var modbusTcp = new ModbusTCP();
modbusTcp.Connect("192.168.1.15", 502);

// Lire 10 coils à partir de l'adresse 1
bool[] coils = modbusTcp.ReadCoils(1, 10);
Console.WriteLine("Coils lus : " + string.Join(", ", coils));

// Écrire un seul registre à l'adresse 1
modbusTcp.WriteSingleRegister(1, 1234);

modbusTcp.Disconnect();
```
## Modbus RTU

Pour utiliser la bibliothèque Modbus RTU, suivez ces étapes :

1. Créez une instance de la classe `ModbusRTU`.
2. Connectez-vous à l'appareil Modbus RTU en spécifiant le port série, la vitesse de communication, la parité, les bits de données et les bits de stop.
3. Effectuez des opérations de lecture/écriture, comme la lecture de registres de maintien ou l'écriture de plusieurs registres.
4. Déconnectez-vous une fois terminé.

### Exemple :
```csharp
using ModbusRTULibrary;

var modbusRtu = new ModbusRTU();
modbusRtu.Connect("COM1", 9600, Parity.None, 8, StopBits.One);

// Lire 5 registres de maintien à partir de l'adresse 1
ushort[] holdingRegisters = modbusRtu.ReadHoldingRegisters(1, 1, 5);
Console.WriteLine("Registres de maintien lus : " + string.Join(", ", holdingRegisters));

// Écrire plusieurs registres à partir de l'adresse 5
ushort[] valeurs = { 100, 200, 300 };
modbusRtu.WriteMultipleRegisters(1, 5, valeurs);

modbusRtu.Disconnect();
```
## Exemples

### Lire des coils (Modbus TCP)
```csharp
using ModbusTCPLibrary;

var modbusTcp = new ModbusTCP();
modbusTcp.Connect("192.168.1.15", 502);

bool[] coils = modbusTcp.ReadCoils(1, 10);
Console.WriteLine("Coils lus : " + string.Join(", ", coils));

modbusTcp.Disconnect();
```
### Écrire des registres (Modbus RTU)
```csharp
using ModbusRTULibrary;

var modbusRtu = new ModbusRTU();
modbusRtu.Connect("COM1", 9600, Parity.None, 8, StopBits.One);

ushort[] valeurs = { 100, 200, 300 };
modbusRtu.WriteMultipleRegisters(1, 5, valeurs);

modbusRtu.Disconnect();
```
### Lecture/Écriture multiple de registres (Modbus TCP)
```csharp
using ModbusTCPLibrary;

var modbusTcp = new ModbusTCP();
modbusTcp.Connect("192.168.1.15", 502);

// Lire 5 registres à partir de l'adresse 1 et écrire 3 registres à partir de l'adresse 2
ushort[] valeursAEcrire = { 400, 500, 505 };
ushort[] registresLus = modbusTcp.ReadWriteMultipleRegisters(1, 5, 2, valeursAEcrire);

Console.WriteLine("Registres lus : " + string.Join(", ", registresLus));

modbusTcp.Disconnect();
```
## Fonctionnalités Modbus prises en charge

### Fonctions Modbus TCP et RTU
- **Lecture de coils (FC1)** : `ReadCoils`
- **Lecture d'entrées discrètes (FC2)** : `ReadDiscreteInputs`
- **Lecture de registres de maintien (FC3)** : `ReadHoldingRegisters`
- **Lecture de registres d'entrée (FC4)** : `ReadInputRegisters`
- **Écriture d'un seul coil (FC5)** : `WriteSingleCoil`
- **Écriture d'un seul registre (FC6)** : `WriteSingleRegister`
- **Écriture de plusieurs coils (FC15)** : `WriteMultipleCoils`
- **Écriture de plusieurs registres (FC16)** : `WriteMultipleRegisters`
- **Lecture/Écriture multiple de registres (FC23)** : `ReadWriteMultipleRegisters`

## Contribuer
Ce projet a été créé pour un usage personnel, afin d'être propriétaire du code. Cependant, je ne suis pas fermé aux suggestions de nouvelles fonctionnalités qui pourraient améliorer l'outil. J'ai conçu ce code dans un esprit de simplicité, contrairement à d'autres projets similaires qui sont souvent plus complexes et destinés à un public informaticien plus large. Mon objectif est de rendre l'intégration et l'utilisation de Modbus plus accessibles aux automaticiens, en simplifiant les processus tout en conservant la puissance de ce protocole.

De plus, ce projet est en français pour faciliter son utilisation et son intégration dans des environnements francophones.

## Licence
Ce projet est sous licence MPL-2.0 license. Voir le fichier `LICENSE` pour plus de détails.

## Auteur
Maxime0328 - GitHub

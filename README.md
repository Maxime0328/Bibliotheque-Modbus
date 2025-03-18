# Modbus-Library
## Bibliothèque Modbus TCP et RTU

Cette bibliothèque fournit une implémentation complète des protocoles Modbus TCP et RTU en C#. Elle permet de communiquer avec des appareils Modbus via TCP/IP ou une communication série (RTU). La bibliothèque prend en charge la lecture et l'écriture de coils, d'entrées discrètes, de registres de maintien (holding registers) et de registres d'entrée (input registers). Elle est conçue pour être simple d'utilisation, robuste et adaptée à des environnements industriels.

## Table des matières
- [Fonctionnalités](#fonctionnalités)
- [Installation](#installation)
- [Utilisation](#utilisation)
  - [Modbus TCP](#modbus-tcp)
  - [Modbus RTU](#modbus-rtu)
- [Exemples Modbus TCP](#exemples-modbus-tcp)
  - [Lire des coils](#lire-des-coils)
  - [Lire des registres de maintien](#lire-des-registres-de-maintien)
  - [Lire des registres d'entrée](#lire-des-registres-dentree)
  - [Écrire un seul coil](#ecrire-un-seul-coil)
  - [Écrire plusieurs coils](#ecrire-plusieurs-coils)
  - [Écrire un seul registre de maintien](#ecrire-un-seul-registre-de-maintien)
  - [Écrire plusieurs registres de maintien](#ecrire-plusieurs-registres-de-maintien)
  - [Lecture/Écriture multiple de registres](#lectureécriture-multiple-de-registres)
- [Exemples Modbus RTU](#exemples-modbus-rtu)
  - [Lire des coils](#lire-des-coils)
  - [Lire des registres de maintien](#lire-des-registres-de-maintien)
  - [Lire des registres d'entrée](#lire-des-registres-dentree)
  - [Écrire un seul coil](#ecrire-un-seul-coil)
  - [Écrire plusieurs coils](#ecrire-plusieurs-coils)
  - [Écrire un seul registre de maintien](#ecrire-un-seul-registre-de-maintien)
  - [Écrire plusieurs registres de maintien](#ecrire-plusieurs-registres-de-maintien)
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

1. Telecharger le [release](https://github.com/Maxime0328/Bibliotheque-Modbus/releases) ou clonez le dépôt :
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
> [!NOTE]
> Ce programme a été testé sur Windows 11 Pro x64 et Windows 7 x86, afin de garantir son fonctionnement avec .NET 3.5
```csharp
using System;
using ModbusTCPLibrary;

class Program
{
    static void Main()
    {
        // Créer une instance de la classe ModbusTCP
        var modbusTcp = new ModbusTCP();

        // Définir l'adresse IP et le port du dispositif Modbus TCP
        string ipAddress = "192.168.1.15";
        int port = 502;

        // Connexion initiale
        try
        {
            Console.WriteLine("Connexion au dispositif Modbus TCP...");
            modbusTcp.Connect(ipAddress, port);
            Console.WriteLine("Connexion réussie !");
        }
        catch (Exception ex)
        {
            // Si la connexion échoue, afficher un message d'erreur et quitter le programme
            Console.WriteLine("Erreur de connexion : " + ex.Message);
            Console.WriteLine("Vérifiez l'adresse IP, le port et la connexion réseau.");
            return; // Arrêter l'exécution du programme
        }

        try
        {
            // Lire 10 coils à partir de l'adresse 1
            bool[] coils = modbusTcp.ReadCoils(1, 10);

            // Afficher les valeurs des coils lus
            //conversion de coils a cause de .NET 3.5. ; Si .NET4.0+ Console.WriteLine("Coils lus : " + string.Join(", ", coils));
            Console.WriteLine("Coils lus : " + string.Join(", ", Array.ConvertAll(coils, b => b.ToString())));

            // Écrire un seul registre à l'adresse 1
            modbusTcp.WriteSingleRegister(1, 1234);
        }
        catch (Exception ex)
        {
            // Gestion des erreurs (par exemple, déconnexion)
            Console.WriteLine("Erreur : " + ex.Message);

            // Tentative de reconnexion
            Console.WriteLine("Tentative de reconnexion...");
            try
            {
                modbusTcp.Reconnect(ipAddress, port);
                Console.WriteLine("Reconnexion réussie !");
            }
            catch (Exception reconnectEx)
            {
                Console.WriteLine("Échec de la reconnexion : " + reconnectEx.Message);
            }
        }
        finally
        {
            // Déconnexion garantie
            modbusTcp.Disconnect();
            Console.WriteLine("Déconnexion effectuée.");
        }
    }
}
```
## Exemples Modbus TCP

### Lire des coils
```csharp
using ModbusTCPLibrary;

var modbusTcp = new ModbusTCP();
modbusTcp.Connect("192.168.1.15", 502); // Connexion au dispositif Modbus TCP

// Lire 10 coils à partir de l'adresse 1
bool[] coils = modbusTcp.ReadCoils(1, 10);
Console.WriteLine("Coils lus : " + string.Join(", ", coils)); // Afficher les valeurs des coils

modbusTcp.Disconnect(); // Déconnexion
```
### Lire des registres de maintien
```csharp
using ModbusTCPLibrary;

var modbusTcp = new ModbusTCP();
modbusTcp.Connect("192.168.1.15", 502); // Connexion au dispositif Modbus TCP

// Lire 5 registres de maintien à partir de l'adresse 1
ushort[] holdingRegisters = modbusTcp.ReadHoldingRegisters(1, 5);
Console.WriteLine("Registres de maintien lus : " + string.Join(", ", holdingRegisters)); // Afficher les valeurs

modbusTcp.Disconnect(); // Déconnexion
```
### Lire des registres d'entrée
```csharp
using ModbusTCPLibrary;

var modbusTcp = new ModbusTCP();
modbusTcp.Connect("192.168.1.15", 502); // Connexion au dispositif Modbus TCP

// Lire 3 registres d'entrée à partir de l'adresse 1
ushort[] inputRegisters = modbusTcp.ReadInputRegisters(1, 3);
Console.WriteLine("Registres d'entrée lus : " + string.Join(", ", inputRegisters)); // Afficher les valeurs

modbusTcp.Disconnect(); // Déconnexion
```
### Écrire un seul coil
```csharp
using ModbusTCPLibrary;

var modbusTcp = new ModbusTCP();
modbusTcp.Connect("192.168.1.15", 502); // Connexion au dispositif Modbus TCP

// Écrire true (ON) à l'adresse 1
modbusTcp.WriteSingleCoil(1, true);
Console.WriteLine("Coil à l'adresse 1 mis à ON.");

modbusTcp.Disconnect(); // Déconnexion
```
### Écrire plusieurs coils
```csharp
using ModbusTCPLibrary;

var modbusTcp = new ModbusTCP();
modbusTcp.Connect("192.168.1.15", 502); // Connexion au dispositif Modbus TCP

// Écrire plusieurs coils à partir de l'adresse 5
bool[] coilValues = { true, false, true, false };
modbusTcp.WriteMultipleCoils(5, coilValues);
Console.WriteLine("Coils écrits à partir de l'adresse 5.");

modbusTcp.Disconnect(); // Déconnexion
```
### Écrire un seul registre de maintien
```csharp
using ModbusTCPLibrary;

var modbusTcp = new ModbusTCP();
modbusTcp.Connect("192.168.1.15", 502); // Connexion au dispositif Modbus TCP

// Écrire la valeur 1234 à l'adresse 1
modbusTcp.WriteSingleRegister(1, 1234);
Console.WriteLine("Registre à l'adresse 1 mis à 1234.");

modbusTcp.Disconnect(); // Déconnexion
```
### Écrire plusieurs registres de maintien
```csharp
using ModbusTCPLibrary;

var modbusTcp = new ModbusTCP();
modbusTcp.Connect("192.168.1.15", 502); // Connexion au dispositif Modbus TCP

// Écrire plusieurs registres à partir de l'adresse 5
ushort[] registerValues = { 100, 200, 300 };
modbusTcp.WriteMultipleRegisters(5, registerValues);
Console.WriteLine("Registres écrits à partir de l'adresse 5.");

modbusTcp.Disconnect(); // Déconnexion
```
### Lecture/Écriture multiple de registres
```csharp
using ModbusTCPLibrary;

var modbusTcp = new ModbusTCP();
modbusTcp.Connect("192.168.1.15", 502); // Connexion au dispositif Modbus TCP

// Lire 5 registres à partir de l'adresse 1 et écrire 3 registres à partir de l'adresse 2
ushort[] valeursAEcrire = { 400, 500, 505 };
ushort[] registresLus = modbusTcp.ReadWriteMultipleRegisters(1, 5, 2, valeursAEcrire);

Console.WriteLine("Registres lus : " + string.Join(", ", registresLus)); // Afficher les valeurs lues
Console.WriteLine("Registres écrits : " + string.Join(", ", valeursAEcrire)); // Afficher les valeurs écrites

modbusTcp.Disconnect(); // Déconnexion
```
## Modbus RTU

> [!WARNING]
> La classe ModbusRTU n’a pas encore été testée, contrairement à ModbusTCP. En théorie, le code fonctionne.

Pour utiliser la bibliothèque Modbus RTU, suivez ces étapes :

1. Créez une instance de la classe `ModbusRTU`.
2. Connectez-vous à l'appareil Modbus RTU en spécifiant le port série, la vitesse de communication, la parité, les bits de données et les bits de stop.
3. Effectuez des opérations de lecture/écriture, comme la lecture de registres de maintien ou l'écriture de plusieurs registres.
4. Déconnectez-vous une fois terminé.

### Exemple :
```csharp
using System;
using System.IO.Ports;
using ModbusRTULibrary;

class Program
{
    static void Main()
    {
        // Créer une instance de la classe ModbusRTU
        var modbusRtu = new ModbusRTU();

        // Définir les paramètres de communication série
        string portName = "COM1"; // Port série utilisé (ex: COM1, COM2, etc.)
        int baudRate = 9600; // Vitesse de communication (baud rate)
        Parity parity = Parity.None; // Parité (None, Even, Odd, Mark, Space)
        int dataBits = 8; // Nombre de bits de données
        StopBits stopBits = StopBits.One; // Nombre de bits de stop

        // Connexion initiale
        try
        {
            Console.WriteLine("Connexion au dispositif Modbus RTU...");
            modbusRtu.Connect(portName, baudRate, parity, dataBits, stopBits);
            Console.WriteLine("Connexion réussie !");
        }
        catch (Exception ex)
        {
            // Si la connexion échoue, afficher un message d'erreur et quitter le programme
            Console.WriteLine("Erreur de connexion : " + ex.Message);
            Console.WriteLine("Vérifiez le port série, les paramètres et la connexion.");
            return; // Arrêter l'exécution du programme
        }

        try
        {
            // Lire 10 coils à partir de l'adresse 1 sur l'esclave avec l'ID 1
            bool[] coils = modbusRtu.ReadCoils(1, 1, 10);

            // Afficher les valeurs des coils lus
            Console.WriteLine("Coils lus : " + string.Join(", ", Array.ConvertAll(coils, b => b.ToString()))); // Conversion pour .NET 3.5

            // Écrire un seul registre à l'adresse 1 sur l'esclave avec l'ID 1
            modbusRtu.WriteSingleRegister(1, 1, 1234);
        }
        catch (Exception ex)
        {
            // Gestion des erreurs (par exemple, déconnexion)
            Console.WriteLine("Erreur : " + ex.Message);

            // Tentative de reconnexion
            Console.WriteLine("Tentative de reconnexion...");
            try
            {
                modbusRtu.Reconnect(portName, baudRate, parity, dataBits, stopBits);
                Console.WriteLine("Reconnexion réussie !");
            }
            catch (Exception reconnectEx)
            {
                Console.WriteLine("Échec de la reconnexion : " + reconnectEx.Message);
            }
        }
        finally
        {
            // Déconnexion garantie
            modbusRtu.Disconnect();
            Console.WriteLine("Déconnexion effectuée.");
        }
    }
}
```
## Exemples Modbus RTU
### Lire des coils
```csharp
using ModbusRTULibrary;

var modbusRtu = new ModbusRTU();
modbusRtu.Connect("COM1", 9600, Parity.None, 8, StopBits.One); // Connexion au port série

// Lire 10 coils à partir de l'adresse 1 sur l'esclave avec l'ID 1
bool[] coils = modbusRtu.ReadCoils(1, 1, 10);
Console.WriteLine("Coils lus : " + string.Join(", ", coils)); // Afficher les valeurs des coils

modbusRtu.Disconnect(); // Déconnexion
```
### Lire des entrées discrètes
```csharp
using ModbusRTULibrary;

var modbusRtu = new ModbusRTU();
modbusRtu.Connect("COM1", 9600, Parity.None, 8, StopBits.One); // Connexion au port série

// Lire 5 entrées discrètes à partir de l'adresse 1 sur l'esclave avec l'ID 1
bool[] discreteInputs = modbusRtu.ReadDiscreteInputs(1, 1, 5);
Console.WriteLine("Entrées discrètes lues : " + string.Join(", ", discreteInputs)); // Afficher les valeurs

modbusRtu.Disconnect(); // Déconnexion
```
### Lire des registres de maintien
```csharp
using ModbusRTULibrary;

var modbusRtu = new ModbusRTU();
modbusRtu.Connect("COM1", 9600, Parity.None, 8, StopBits.One); // Connexion au port série

// Lire 5 registres de maintien à partir de l'adresse 1 sur l'esclave avec l'ID 1
ushort[] holdingRegisters = modbusRtu.ReadHoldingRegisters(1, 1, 5);
Console.WriteLine("Registres de maintien lus : " + string.Join(", ", holdingRegisters)); // Afficher les valeurs

modbusRtu.Disconnect(); // Déconnexion
```
### Lire des registres d'entrée
```csharp
using ModbusRTULibrary;

var modbusRtu = new ModbusRTU();
modbusRtu.Connect("COM1", 9600, Parity.None, 8, StopBits.One); // Connexion au port série

// Lire 3 registres d'entrée à partir de l'adresse 1 sur l'esclave avec l'ID 1
ushort[] inputRegisters = modbusRtu.ReadInputRegisters(1, 1, 3);
Console.WriteLine("Registres d'entrée lus : " + string.Join(", ", inputRegisters)); // Afficher les valeurs

modbusRtu.Disconnect(); // Déconnexion
```
### Écrire un seul coil
```csharp
using ModbusRTULibrary;

var modbusRtu = new ModbusRTU();
modbusRtu.Connect("COM1", 9600, Parity.None, 8, StopBits.One); // Connexion au port série

// Écrire true (ON) à l'adresse 1 sur l'esclave avec l'ID 1
modbusRtu.WriteSingleCoil(1, 1, true);
Console.WriteLine("Coil à l'adresse 1 mis à ON.");

modbusRtu.Disconnect(); // Déconnexion
```
### Écrire plusieurs coils
```csharp
using ModbusRTULibrary;

var modbusRtu = new ModbusRTU();
modbusRtu.Connect("COM1", 9600, Parity.None, 8, StopBits.One); // Connexion au port série

// Écrire plusieurs coils à partir de l'adresse 5 sur l'esclave avec l'ID 1
bool[] coilValues = { true, false, true, false };
modbusRtu.WriteMultipleCoils(1, 5, coilValues);
Console.WriteLine("Coils écrits à partir de l'adresse 5.");

modbusRtu.Disconnect(); // Déconnexion
```
### Écrire un seul registre de maintien
```csharp
using ModbusRTULibrary;

var modbusRtu = new ModbusRTU();
modbusRtu.Connect("COM1", 9600, Parity.None, 8, StopBits.One); // Connexion au port série

// Écrire la valeur 1234 à l'adresse 1 sur l'esclave avec l'ID 1
modbusRtu.WriteSingleRegister(1, 1, 1234);
Console.WriteLine("Registre à l'adresse 1 mis à 1234.");

modbusRtu.Disconnect(); // Déconnexion
```
### Écrire plusieurs registres de maintien
```csharp
using ModbusRTULibrary;

var modbusRtu = new ModbusRTU();
modbusRtu.Connect("COM1", 9600, Parity.None, 8, StopBits.One); // Connexion au port série

// Écrire plusieurs registres à partir de l'adresse 5 sur l'esclave avec l'ID 1
ushort[] registerValues = { 100, 200, 300 };
modbusRtu.WriteMultipleRegisters(1, 5, registerValues);
Console.WriteLine("Registres écrits à partir de l'adresse 5.");

modbusRtu.Disconnect(); // Déconnexion
```
### Lecture/Écriture multiple de registres
```csharp
using ModbusRTULibrary;

var modbusRtu = new ModbusRTU();
modbusRtu.Connect("COM1", 9600, Parity.None, 8, StopBits.One); // Connexion au port série

// Lire 5 registres à partir de l'adresse 1 et écrire 3 registres à partir de l'adresse 2 sur l'esclave avec l'ID 1
ushort[] valeursAEcrire = { 400, 500, 505 };
ushort[] registresLus = modbusRtu.ReadWriteMultipleRegisters(1, 1, 5, 2, valeursAEcrire);

Console.WriteLine("Registres lus : " + string.Join(", ", registresLus)); // Afficher les valeurs lues
Console.WriteLine("Registres écrits : " + string.Join(", ", valeursAEcrire)); // Afficher les valeurs écrites

modbusRtu.Disconnect(); // Déconnexion
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

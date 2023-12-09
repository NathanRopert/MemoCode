# Introduction à MySQL avec Python

MySQL Connector/Python est un connecteur Python pour la base de données MySQL. Il permet de se connecter, de manipuler et d'interagir avec des bases de données MySQL à partir d'applications Python.

## Installation de MySQL Connector/Python

Pour installer MySQL Connector/Python, utilisez la commande suivante :
```bash
pip install mysql-connector-python
```

## Exemple d'utilisation de MySQL Connector/Python

Pour se connecter à une base de données MySQL et effectuer une requête simple :

```python
import mysql.connector

# Établir la connexion à la base de données
mydb = mysql.connector.connect(
  host="localhost",
  user="utilisateur",
  password="motdepasse",
  database="nom_de_la_base"
)

# Créer un curseur pour exécuter des requêtes SQL
mycursor = mydb.cursor()

# Exemple d'exécution d'une requête SQL SELECT
mycursor.execute("SELECT * FROM ma_table")

# Récupérer les résultats de la requête
resultats = mycursor.fetchall()

# Afficher les résultats
for row in resultats:
  print(row)

# Fermer le curseur et la connexion à la base de données
mycursor.close()
mydb.close()
```

## Exemple de requête SQL securisée

Pour éviter les injections SQL, il est recommandé d'utiliser des requêtes SQL sécurisées. Pour cela, on utilise des paramètres dans la requête SQL. Les paramètres sont représentés par des `%s` dans la requête SQL. Les valeurs des paramètres sont passées dans un tuple en deuxième paramètre de la fonction `execute()`.

Il y a plusieurs type de paramètres :
- `%s` : paramètre générique
- `%d` : paramètre pour les nombres entiers
- `%f` : paramètre pour les nombres à virgule flottante
- `%b` : paramètre pour les données binaires
- `%B` : paramètre pour les données binaires

Voici un exemple d'utilisation de paramètres dans une requête SQL sécurisée :

```python
# Créer un curseur pour exécuter des requêtes SQL
mycursor = mydb.cursor()

# Exemple d'exécution d'une requête sécurisée avec des paramètres
sql = "SELECT * FROM ma_table WHERE nom = %s AND age = %s"
valeurs = ("John Doe", 30)  # Exemple de valeurs à passer comme paramètres

mycursor.execute(sql, valeurs)

# Récupérer les résultats de la requête
resultats = mycursor.fetchall()

# Afficher les résultats
for row in resultats:
    valeur1 = row[0] # Exemple de récupération d'une valeur dans la première colonne
    valeur2 = row[1]
    print("Valeur 1 : " + valeur1 + " Valeur 2 : " + valeur2)

# Fermer le curseur et la connexion à la base de données
mycursor.close()
mydb.close()
```

## Gestions des erreurs

Pour gérer les erreurs, on utilise un bloc `try...except` :

```python
try:
    # Tentative d'exécution de la requête ou de la connexion à la base de données
except mysql.connector.Error as error:
    # En cas d'erreur, affichage du message d'erreur
    print("Erreur MySQL:", error)
```
Il existe plusieurs types d'erreurs :
- `mysql.connector.Error` : erreur générique
- `mysql.connector.errors.DatabaseError` : erreur de base de données
- `mysql.connector.errors.DataError` : erreur de données
- `mysql.connector.errors.IntegrityError` : erreur d'intégrité
- `mysql.connector.errors.InterfaceError` : erreur d'interface
- `mysql.connector.errors.InternalError` : erreur interne
- `mysql.connector.errors.NotSupportedError` : erreur non supportée
- `mysql.connector.errors.OperationalError` : erreur opérationnelle
- `mysql.connector.errors.PoolError` : erreur de pool
- `mysql.connector.errors.ProgrammingError` : erreur de programmation
- `mysql.connector.errors.Warning` : avertissement

Avec le `mysql.connector.Error`, on peut récupérer le code de l'erreur avec : 

```python
except mysql.connector.Error as error:
    print("Erreur MySQL:", error)
    print("Code de l'erreur :", error.errno)
```

Voici une liste des codes d'erreurs les plus courants :
- 1062 : `Duplicate entry` Ce code d'erreur indique qu'une tentative d'insertion d'une valeur qui existe déjà dans une colonne avec une contrainte d'unicité a été effectuée.
- 1045 : `Access denied` Ce code d'erreur indique un problème d'authentification, généralement des informations d'identification incorrectes lors de la connexion à la base de données.
- 1048 : `Column cannot be null` Ce code d'erreur indique qu'une tentative a été faite pour insérer ou mettre à jour une colonne définie comme non nullable avec une valeur NULL.
- 1054 : `Unknown column` Ce code d'erreur survient lorsque tu essaies d'accéder à une colonne qui n'existe pas dans une table spécifiée.
- 1216 : `Cannot add or update a child row` Ce code d'erreur se produit lorsqu'une tentative est faite pour créer ou mettre à jour une clé étrangère, mais que la valeur parente n'existe pas dans la table parente.
- 1064 : `Syntax error` Ce code d'erreur indique qu'il y a une erreur de syntaxe dans la requête SQL.
- 1452 : `Cannot add or update a child row a foreign key constraint fails` Ce code d'erreur survient lorsqu'une contrainte de clé étrangère est violée.
- 2002 : `Can't connect to MySQL server` Ce code d'erreur indique qu'il y a un problème pour se connecter au serveur MySQL, comme une mauvaise adresse IP ou un port incorrect.

## Exemple de gestion d'erreur

Voici un exemple de gestion d'erreur avec MySQL Connector/Python pour gérer spécifiquement l'erreur de duplication :

```python
import mysql.connector

try:
    # Le code avec la connexion et l'exécution des requêtes ici...

except mysql.connector.Error as error:
    # Capturer les erreurs MySQL
    if "Duplicate entry" in str(error):
        # Gérer spécifiquement l'erreur de duplication
        print("Erreur: Entrée en double détectée.")
    else:
        # Gérer d'autres erreurs
        print("Erreur MySQL:", error)
```     

## Exemple de gestion d'erreur avec rollback

Voici un exemple de gestion d'erreur avec MySQL Connector/Python pour gérer spécifiquement l'erreur de duplication et effectuer un rollback :

```python
import mysql.connector

try:
    # Le code avec la connexion et l'exécution des requêtes ici...

except mysql.connector.Error as error:
    # Capturer les erreurs MySQL
    if "Duplicate entry" in str(error):
        # Gérer spécifiquement l'erreur de duplication
        print("Erreur: Entrée en double détectée.")
        # Effectuer un rollback
        mydb.rollback()
    else:
        # Gérer d'autres erreurs
        print("Erreur MySQL:", error)
```


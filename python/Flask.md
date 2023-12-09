# Introduction à Flask : Un framework web minimaliste pour Python

Flask est un framework web pour Python. Il offre une approche simple et flexible pour créer des applications web. Il permet de rapidement mettre en place des applications web, des APIs.

## Liens vers mes projets Git
- [Projet Flask 1 - Application mobile OneShoot](lien_vers_ton_projet_git_1)

## Installation de flask 
pip install flask

## Les base d'un fichier 
Importation de la librairie flask et création d'une instance de l'application. Ce code établit une application Flask basique avec une seule route '/' qui renvoie "Hello, World!" lorsque que l'on accède à l'URL de l'application.

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(debug=True)
```
L'instruction if __name__ == '__main__': est utilisée pour exécuter le serveur de développement uniquement lorsque le script est exécuté directement par Python. Lorsque le fichier est importé, il ne sera pas exécuté.

## Les routes
Les routes sont les URL de votre application. Elles permettent de définir les actions à effectuer lorsque l'utilisateur accède à l'URL de votre application dans un navigateur web. Les routes sont définies à l'aide de l'annotation @app.route(). On peut ajouter des parametre dans l'URL. 

```python
@app.route('/', methods=['GET', 'POST'])
def index():
    return 'Index Page'
```
le paramètre methods permet de définir les méthodes HTTP autorisées. Il existe plusieurs méthodes HTTP, mais les plus courantes sont GET et POST. 

- GET : Les requêtes GET doivent uniquement récupérer des données.
- POST : Les requêtes POST doivent avoir pour effet de modifier les données.
- DELETE : Les requêtes DELETE doivent supprimer les données.
- PUT : Les requêtes PUT doivent remplacer les données.
- PATCH : Les requêtes PATCH doivent modifier les données.
- HEAD : Les requêtes HEAD doivent uniquement récupérer des en-têtes.
- OPTIONS : Les requêtes OPTIONS doivent uniquement récupérer les méthodes HTTP autorisées.

## Recuperer les parametres d'une URL
Pour récupérer les paramètres d'une URL, on utilise la fonction `request.args.get()`.

```python
from flask import request

@app.route('/login', methods=['GET', 'POST'])
def login():
        nomUtilisateur = request.args.get('nomutilisateur')
        motDePasse = request.args.get('motdepasse')
        return "Nom d'utilisateur : " + nomUtilisateur + " Mot de passe : " + motDePasse
```
Voici un exemple d'URL : 
```http
http://localhost:5000/login?nomutilisateur=Jean&motdepasse=1234
```






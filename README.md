# TP Pokemon

Voici la suite du TP Pokemon-SQL, vous avez maintenant une base de donnée prête et vous savez désormais manipuler les données de cette BDD grâce aux commandes SQL.

Vous allez maintenant devoir mettre en place un site web en HTML, CSS, JS et créer des cards pour accueillir les données de vos Pokemons. Pour ce faire nous allons utiliser `Nodejs`et `Express`. Pour le style des cards vous êtes libres d’utiliser Bootstrap, tailwindcss ou de faire du css natif.

**Étape 1 : Préparation de votre serveur.**

→ Lancer MAMP et accéder à votre base de donnée sur Phpmyadmin. 

→ Vérifier bien votre base de donnée ‘Pokemon’ est toujours remplie.

**Étape 2 : Configuration de Node.js pour se connecter à MySQL**

1. **Installation des dépendances nécessaires :**
    - Dans le terminal, installez les modules **`mysql`** et **`express`** :
        
        ```bash
        npm install mysql express
        ```
        
2. **Création d'un fichier `server.js` :**
    - Créez un fichier **`server.js`** à la racine de votre projet et ajoutez le code suivant pour configurer le serveur Express et se connecter à MySQL :

```jsx
const express = require('express');
const mysql = require('mysql');
const app = express();
const port = 3000;

// Configuration de la connexion à MySQL
const db = mysql.createConnection({
  host: 'localhost',
  user: 'root', // Remplacez par votre nom d'utilisateur MySQL
  password: '', // Remplacez par votre mot de passe MySQL
  database: 'pokemon'
});

db.connect((err) => {
  if (err) {
    throw err;
  }
  console.log('MySQL Connected...');
});

// Route pour récupérer tous les Pokémon
app.get('/pokemons', (req, res) => {
  let sql = // Commande SQL pour récupérer tous les pokemons de la BDD ;
  db.query(sql, (err, results) => {
    if (err) {
      throw err;
    }
    res.json(results);
  });
});

app.listen(port, () => {
  console.log(`Server started on port ${port}`);
});

```

1. **Démarrage du serveur Node.js :**
- Dans le terminal, démarrez le serveur :
    
    ```
    node server.js
    
    ```
    

### **Étape 3 : Intégration de l'API avec le site web**

1. **Modification du site web pour utiliser l'API Node.js :**
    - Dans le fichier HTML de votre projet, ajoutez un script JavaScript pour récupérer les données des Pokémon depuis l'API et les afficher dans les cards.
2. **Exemple de code JavaScript pour récupérer et afficher les Pokémon : (vous avez un exemple avec le nom du pokemon, à vous de faire pareil pour les autre données).**

**TIPS : Attention aux noms des ID et des classes.**

```jsx
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pokémon</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <div id="pokemon-container"></div>

  <script>
    fetch('http://localhost:3000/pokemons')
      .then(response => response.json())
      .then(pokemons => {
        const container = document.getElementById('');
        pokemons.forEach(pokemon => {
          const card = document.createElement('div');
          card.className = '';
          card.innerHTML = `
            <h2>${pokemon.name}</h2>
            <p>Type:  </p>
            <p>HP:  </p>
            <p>Attack: </p>
            <p>Defense: </p>
            <p>Speed: </p>
          `;
          container.appendChild(card);
        });
      })
      .catch(error => console.error('Error:', error));
  </script>
</body>
</html>

```

### **Étape 5 : Vérification et test**

1. **Lancement de MySQL et du serveur Node.js :**
    - Assurez-vous que votre serveur MySQL est en cours d'exécution.
    - Démarrez le serveur Node.js (**`node server.js`**).
2. **Test du site web :**
    - Ouvrez votre navigateur et accédez à votre fichier HTML.
    - Vérifiez que les cards des Pokémon s'affichent correctement avec les informations récupérées depuis la base de données.
    

### **Étape 6 : Création d'un formulaire pour ajouter un nouveau Pokémon à la base de données**

Dans cette étape, vous devez créer un formulaire HTML pour ajouter un nouveau Pokémon dans la base de données, et configurer le serveur Node.js pour traiter ces ajouts.

### **Étape 6.1 : Création du formulaire HTML**

1. **Ajout du formulaire dans le fichier HTML existant :**
    
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Pokémon</title>
      <link rel="stylesheet" href="styles.css">
    </head>
    <body>
      <div id="pokemon-container"></div>
    
      <!-- Formulaire pour ajouter un nouveau Pokémon -->
      <form id="add-pokemon-form">
        <h2>Ajouter un nouveau Pokémon</h2>
        <label for="name">Nom:</label>
        <input type="text" id="name" name="name" required><br>
        <label for="type">Type:</label>
        <input type="text" id="type" name="type" required><br>
        
        
        // à vous de remplir correctement le formulaire.
        
        
        <button type="submit">Ajouter</button>
      </form>
    
      <script>
    
        // Code JavaScript pour ajouter un nouveau Pokémon
        const form = document.getElementById('');
        form.addEventListener('submit', function (event) {
          event.preventDefault();
          const formData = new FormData(form);
          const data = {
            name: formData.get('name'),
            type: formData.get('type'),
            
            
            // Etc ...
          };
    
          fetch('http://localhost:3000/pokemons', {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify(data)
          })
          .then(response => response.json())
          .then(pokemon => {
            alert('Nouveau Pokémon ajouté avec succès !');
            // Ajouter le nouveau Pokémon à la liste sans recharger la page
            const container = document.getElementById('');
            const card = document.createElement('div');
            card.className = 'pokemon-card';
            card.innerHTML = `
              <h2>${pokemon.name}</h2>
              <p>Type: ${pokemon.type}</p>
              
              
             // etc ...
            `;
            container.appendChild(card);
          })
          .catch(error => console.error('Error:', error));
        });
      </script>
    </body>
    </html>
    
    ```
    
    ### **tape 6.2 : Configuration du serveur Node.js pour accepter les ajouts**
    
    1. **Modification du fichier `server.js` pour gérer les requêtes POST :**

```jsx
// ... Votre ancien code 

// Route pour ajouter un nouveau Pokémon
app.post('/pokemons', (req, res) => {
  const newPokemon = req.body;
  let sql = // requête SQL pour insérer un pokemon. 
  db.query(sql, newPokemon, (err, result) => {
    if (err) {
      throw err;
    }
    res.json({ id: result.insertId, ...newPokemon });
  });
});

app.listen(port, () => {
  console.log(`Server started on port ${port}`);
});
```

1. **Redémarrage du serveur Node.js :**
    - Après avoir enregistré les modifications, redémarrez le serveur :
        
        ```bash
        bashCopier le code
        node server.js
        
        ```
        

### **Étape 6.3 : Test du formulaire**

1. **Ajout de nouveaux Pokémon via le formulaire :**
    - Ouvrez votre navigateur et accédez à votre fichier HTML.
    - Remplissez le formulaire pour ajouter un nouveau Pokémon et soumettez-le.
    - Vérifiez que le nouveau Pokémon apparaît dans la liste des Pokémon.
2. **Vérification dans la base de données :**
    - Ouvrez PHPMyAdmin et assurez-vous que le nouveau Pokémon a été ajouté à la table **`pokemons`**.
    

**Conclusion**

Bravo, chers dresseurs de pokemons ! Vous pouvez maintenant ajouter de nouveaux pokemons et les afficher dans vos cards dynamiquement ! 

**Pour aller plus loin**, vous pouvez créer un autre formulaire pour cette fois-ci supprimer un pokemon de votre base de donnée.

Faites de même pour la modification des stats d’un pokemon…

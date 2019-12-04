# fiche_socket-io
Une fiche sur la technologie socket.io

# Socket io qu'est ce que c'est ?

Socket.io est une librairie qui permet une communication en temps réel, qui est bidirectionelle et basée sur des événements entre le navigateyr et le serveur.
Sont utilisés :
- un serveur Node.js
- une librairie cliente javascript pour le navigateur 

Les fonctionnalités principales sont :

# La fiabilité

Les connexions sont établies même en présence de

-procurations et équilibreurs de charge.
-pare-feu personnel et logiciel antivirus.

A cette fin, il s'appuie sur Engine.IO, qui établit d'abord une connexion à interrogation longue, puis tente de procéder à une mise  niveau vers de meilleurs transports "testés" sur le côté, comme WebSocket.

# Prise en charge de la reconnexion automatique

Sauf indication contraire, un client déconnecté essaiera de se reconnecter indéfiniment jusqu'à ce que le serveur soit à nouveau disponible. 

# Détection de déconnexion

Un mécanisme de pulsation est implémenté au niveau de Engine.IO, permettant ainsi au serveur et au client de savoir quand l'autre ne répond plus.

Cette fonctionnalité est obtenue avec des minuteries définies à la fois sur le serveur et sur le client, avec des valeurs  de délai d'attente (les paramètres pinginterval et pingTimeout) partagées lors de l'établissement de la connexion.
Ces minuteries exigent que tous les appels clients ultérieurs soient dirigés vers le même serveur, d'où la condition de blocage de session lors de l'utilisation de plusieurs noeuds.

# Support binaire

Toutes les structures de données sérialisables peuvent être émises, notamment :
-ArrayBuffer et Blob dans le navigateur
-ArrayBuffer et Buffer dans Node.js

# Prise en charge du multiplexage

Afin de séparer les préoccupations de votre application (par exemple, par module ou en fonction d'autorisations), Socket.IO vous permet de créer plusieurs espaces de noms, qui agiront comme des canaux de communication distincts tout en partageant la même connexion sous-jacente.

# Soutien de la salle

Dans chaque espace de noms , vous pouvez définir des canaux arbitraires, appelés pièces , que les sockets peuvent rejoindre et quitter. Vous pouvez ensuite diffuser dans n’importe quelle pièce et atteindre chaque prise qui l’a rejoint.

Cette fonctionnalité est utile pour envoyer des notifications à un groupe d'utilisateurs ou à un utilisateur donné connecté à plusieurs appareils, par exemple.

Ces fonctionnalités sont fournies avec une API simple et pratique, qui ressemble à ce qui suit:

io.on ( 'connexion' , fonction ( socket ) { 
  socket.emit ( 'demande' , / * * / ); // émet un événement sur la socket
   io.emit ( 'broadcast' , / * * / ); / / emit un événement à tous les sockets connectés
   socket.on ( 'reply' , function () { / * * / }); // écoute l'événement
 });
 
 # Ce que Socket.IO n'est pas
 
 Socket.IO n'est PAS une implémentation WebSocket. Bien que Socket.IO utilise effectivement WebSocket comme transport lorsque cela est possible, il ajoute des métadonnées à chaque paquet: le type de paquet, l'espace de nom et l'identifiant du paquet lorsqu'un accusé de réception de message est requis. C'est pourquoi un client WebSocket ne pourra pas se connecter correctement à un serveur Socket.IO et un client Socket.IO ne pourra pas non plus se connecter à un serveur WebSocket. 

# L'installation
## Serveur
npm install --save socket.io

## Client javascript

Une version autonome du client est exposée par défaut par le serveur à l'adresse /socket.io/socket.io.js.

Il peut aussi être servi depuis un CDN, comme cdnjs .

Pour l'utiliser depuis Node.js, ou avec un bundle tel que webpack ou browserify , vous pouvez également installer le paquet à partir de npm:

npm install --save socket.io-client

#Utilisation avec un serveur http Node
##Serveur (app.js)

var app = require ( 'http' ) .createServer (gestionnaire) 
var io = require ( 'socket.io' ) (app); 
var fs = require ( 'fs' );

app.listen ( 80 );

 gestionnaire de fonctions ( req, res ) {
  fs.readFile (__ dirname + '/index.html' , function ( err, données ) { if (err) {      res.writeHead ( 500 ); renvoie res.end ( 'Erreur de chargement de l'index. html ' );    }
  
    

      


    res.writeHead ( 200 ); 
    res.end (données); 
  }); 
}

io.on ( 'connexion' , fonction ( socket ) { 
  socket.emit ( 'news' , { hello : 'world' }); 
  socket.on ( 'mon autre événement' , fonction ( données ) { console .log (données );   }); });

## Client (index.html)

< script  src = "/socket.io/socket.io.js" ></ script > 
< script >
  var socket = io ( 'http: // localhost' ); 
  socket.on ( 'news' , fonction ( données ) { console .log (données);     socket.emit ( 'mon autre événement' , { my : 'données' });   });
    


</ script >

#Utilisation avec Express
## Serveur (app.js)

var app = require ( 'express' ) (); 
var server = require ( 'http' ) .Server (app); 
var io = require ( 'socket.io' ) (serveur);

server.listen ( 80 ); 
// ATTENTION: app.listen (80) ne fonctionnera PAS ici!

app.get ( '/' , fonction ( req, res ) { 
  res.sendFile (__ dirname + '/index.html' ); 
});

io.on ( 'connexion' , fonction ( socket ) { 
  socket.emit ( 'news' , { hello : 'world' }); 
  socket.on ( 'mon autre événement' , fonction ( données ) { console .log (données );   }); });
    
##Client (index.html)

< script  src = "/socket.io/socket.io.js" ></ script > 
< script >
  var socket = io.connect ( 'http: // localhost' ); 
  socket.on ( 'news' , fonction ( données ) { console .log (données);     socket.emit ( 'mon autre événement' , { my : 'données' });   });
    


</ script >

#Envoi et réception d'événements

Socket.IO vous permet d'émettre et de recevoir des événements personnalisés. En outre connect, messageet disconnect, vous pouvez émettre des événements personnalisés:

##Serveur

// remarque, io (<port>) créera un serveur http pour vous 
var io = require ( 'socket.io' ) ( 80 );

io.on ( 'connexion' , fonction ( socket ) { 
  io.emit ( 'ceci' , { sera : 'sera reçu par tout le monde' });

  socket.on ( 'message privé' , fonction ( de, msg ) { console .log ( 'j'ai reçu un message privé de' , de , 'disant' , msg);   });
    


  socket.on ( 'déconnecter' , fonction () { 
    io.emit ( 'utilisateur déconnecté' ); 
  }); 
});



LAB — Rooting Android & Burp Suite Proxy Analysis
1. Objectif du laboratoire

L’objectif de ce laboratoire est de comprendre comment utiliser Burp Suite comme proxy afin d’observer et d’analyser le trafic HTTP généré par un appareil Android (émulateur).

Ce laboratoire permet de :

Configurer un proxy HTTP avec Burp Suite

Connecter un Android Emulator au proxy

Observer les requêtes HTTP

Comprendre la structure des requêtes et réponses HTTP

Tester l’interception des requêtes

2. Configuration du proxy Burp Suite

Dans cette étape, nous configurons Burp Suite pour écouter les requêtes HTTP provenant de l’émulateur Android.

Le proxy listener est configuré sur le port 8080.

Capture : configuration du Proxy Listener

<img width="782" height="502" alt="img1" src="https://github.com/user-attachments/assets/5920d34f-2c3f-4fe1-8ca8-073107a1e671" />


3. Identification de l’adresse IP de la machine hôte

Pour permettre à l’émulateur Android de communiquer avec Burp Suite, nous devons identifier l’adresse IP locale de la machine.

Commande utilisée :

ipconfig

Résultat obtenu :

IPv4 Address : 192.168.100.208
Subnet Mask  : 255.255.255.0
Default Gateway : 192.168.100.1

Cette adresse sera utilisée comme Proxy Host dans la configuration du réseau Android.

Capture : résultat de la commande ipconfig

<img width="438" height="129" alt="img2" src="https://github.com/user-attachments/assets/f6d56659-e974-4cbf-8635-62535f05dc54" />

4. Configuration du proxy dans l’émulateur Android

Dans l’émulateur Android, nous configurons le réseau Wi-Fi afin d’utiliser le proxy Burp.

Paramètres utilisés :

Proxy : Manual
Proxy Host : 192.168.100.208
Proxy Port : 8080

Capture : configuration du proxy dans Android

<img width="223" height="380" alt="img3" src="https://github.com/user-attachments/assets/3b84cba7-5a23-4584-b522-1caf51fcb11d" />

5. Test de capture HTTP

Après la configuration du proxy, nous ouvrons un navigateur dans l’émulateur Android et accédons au site :

http://example.com

La page se charge correctement, ce qui indique que la connexion réseau fonctionne à travers le proxy.

Capture : navigation sur example.com

<img width="225" height="371" alt="img4" src="https://github.com/user-attachments/assets/6ca2862a-a0e2-482d-bb71-0d843e2227d8" />

6. Capture du trafic HTTP avec Burp Suite

Après l’accès au site web depuis l’émulateur Android, Burp Suite capture les requêtes HTTP envoyées.

Dans l’onglet HTTP history, nous pouvons observer plusieurs requêtes générées par l’appareil Android et par le navigateur.

Exemples de domaines observés :

connectivitycheck.gstatic.com
play.googleapis.com
example.com

Capture : historique HTTP

<img width="953" height="503" alt="img5" src="https://github.com/user-attachments/assets/42e6383b-114e-44ae-a3d5-8d30d2a31d20" />

7. Analyse d’une requête HTTP

Nous sélectionnons une requête dans l’historique et analysons ses détails.

Exemple :

GET /generate_204 HTTP/1.1
Host: connectivitycheck.gstatic.com
User-Agent: Mozilla/5.0 (Linux; Android 10)
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

Informations observées :

Méthode HTTP
GET
Chemin
/generate_204
Headers principaux
Host
User-Agent
Accept-Encoding
Connection

Ces informations permettent d’identifier le client, le type de contenu attendu et la destination de la requête.

Capture : analyse de la requête et de la réponse

<img width="955" height="502" alt="img6" src="https://github.com/user-attachments/assets/9367cbc5-498a-486a-9863-07817f5a035c" />

8. Interception des requêtes

Burp Suite permet également d’intercepter les requêtes avant qu’elles ne soient envoyées au serveur.

Pour cela :

Proxy → Intercept → Intercept ON

Lorsqu’une requête est interceptée, elle apparaît dans Burp Suite et peut être analysée avant d’être transmise.

La requête peut ensuite être envoyée vers le serveur en cliquant sur :

Forward

Capture : interception d’une requête

<img width="959" height="503" alt="img7" src="https://github.com/user-attachments/assets/00abb7f9-dd9b-42f6-95ea-e6dec57f6ef7" />

9. Principe du certificat CA pour HTTPS

Le protocole HTTPS chiffre les communications entre le client et le serveur.

Pour analyser le trafic HTTPS avec un proxy comme Burp Suite, il est nécessaire d’installer un certificat CA de confiance dans l’environnement de test.

Cela permet au proxy de :

Déchiffrer les communications HTTPS

Observer les requêtes et réponses sécurisées

Ce certificat doit être installé uniquement dans un environnement de laboratoire (émulateur) et jamais sur un appareil personnel.

10. Conclusion

Ce laboratoire nous a permis de comprendre :

Le fonctionnement d’un proxy HTTP

La capture et l’analyse du trafic réseau

La structure des requêtes HTTP

Le principe d’interception des communications

Le rôle des certificats CA dans HTTPS

L’utilisation de Burp Suite permet aux analystes de sécurité d’observer et de comprendre les échanges entre une application mobile et les serveurs distants, ce qui est essentiel pour l’analyse et les tests de sécurité.

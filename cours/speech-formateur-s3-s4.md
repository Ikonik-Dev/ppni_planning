# Speech Formateur — S3-S4 · Réseaux & Web Front-End

## PPNI 2026 — Phase 2 (25 mai → 5 juin)

---

## ✅ Checklist formateur — Avant chaque séance

- [ ] Ordinateur branché, navigateur ouvert sur `cours/s3-s4.html`
- [ ] Terminal accessible (PowerShell ou cmd Windows)
- [ ] Tableau blanc disponible (pour les schémas réseau)
- [ ] Connexion réseau stable (exercices en ligne : whatismyip.com, example.com)
- [ ] Fichier `quiz-reseaux.html` testé et fonctionnel
- [ ] Éditeur VS Code installé sur toutes les machines
- [ ] GitHub Desktop installé (pour déploiement GitHub Pages vendredi S4)

---

## S3 · Lundi 25 mai — 09h00 → 17h00

### ⏰ 09h00 – 09h20 · Accueil + retour S1-S2

⚙️ **ATELIER — Retour rapide sur S1-S2**

Questions flash à poser au groupe :

- "Qui a encore son README GitHub ?"
- "Qui s'en sert depuis la formation ?"
- "Une chose que vous avez retenue de la semaine Git ?"

💬 **FORMATEUR :**

> "Bonjour à tous. Avant de démarrer cette nouvelle phase, je veux qu'on revienne
> deux minutes sur S1-S2. Git, c'est un outil qu'on a mis en place pour vous —
> pas juste pour cet exercice, mais pour tout ce que vous allez construire désormais.
> Aujourd'hui, on entre dans la Phase 2 : comprendre Internet de l'intérieur.
> À la fin de ces deux semaines, vous aurez une page web en ligne — visible par
> n'importe qui dans le monde, avec une URL à envoyer à vos contacts.
> On commence par les fondations : comment Internet fonctionne vraiment."

📌 **TECHNIQUE :**
Écrire au tableau : `S3 = le réseau · S4 = le Web` et les deux dates clés.

---

### ⏰ 09h20 – 10h30 · Introduction au réseau — Séquence 3.1

⚙️ **ATELIER — Schéma réseau + observer sa propre IP**

Dessiner progressivement au tableau :

```
[Ton ordi] ──── [Box Internet] ──── [Routeur FAI] ──── [Internet] ──── [Serveur Google]
192.168.1.42     192.168.1.1          (masqué)          142.250.x.x      142.250.75.78
```

💬 **FORMATEUR :**

> "Quand tu tapes 'google.com' dans ton navigateur et que tu appuies sur Entrée,
> que se passe-t-il ? La réponse naïve c'est 'ça se connecte à Google'.
> La vraie réponse, c'est 6 choses qui se passent en moins de 200 millisecondes.
> On va les voir une par une. Mais d'abord — chacun tape dans son terminal :
> `ipconfig` sous Windows."

📌 **TECHNIQUE — Schéma des 6 étapes à construire ensemble :**

1. Tu tapes `google.com` → ton ordi demande au DNS : "C'est quoi l'IP de google.com ?"
2. DNS répond : `142.250.75.78`
3. Ton ordi ouvre une connexion TCP vers ce serveur (port 443)
4. HTTPS : négociation TLS (poignée de mains chiffrée)
5. Requête HTTP GET envoyée dans le tunnel chiffré
6. Serveur répond avec le HTML de la page

> "Votre IP locale (192.168.x.x) : c'est votre adresse dans votre réseau domestique.
> Votre IP publique (sur whatismyip.com) : c'est l'adresse que le monde voit.
> La différence ? Le NAT — votre box fait la traduction. C'est comme un immeuble :
> l'adresse de l'immeuble, c'est l'IP publique. Votre numéro d'appartement, c'est l'IP privée."

---

### ⏰ 10h30 – 12h30 · Protocoles HTTP/HTTPS, DNS, TCP/IP — Séquence 3.2

⚙️ **ATELIER — DevTools Réseau (F12 → onglet Réseau)**

Étapes guidées pour tout le groupe :

1. Ouvrir Chrome → F12 → "Réseau" → cocher "Préserver le journal"
2. Aller sur `http://neverssl.com` → Observer les requêtes
3. Aller sur `https://github.com` → Comparer
4. Cliquer sur la première requête → Headers → voir : méthode, statut, serveur

💬 **FORMATEUR :**

> "HTTP, c'est la langue que votre navigateur et le serveur utilisent pour se parler.
> 'GET /index.html HTTP/1.1' ça veut dire : 'Donne-moi le fichier index.html, s'il te plaît.'
> Le serveur répond avec un code. 200, ça veut dire OK, j'ai trouvé.
> 404 : non trouvé. 500 : le serveur a planté.
> Maintenant, HTTPS c'est HTTP + TLS. La différence ? Tout ce qui passe entre votre navigateur
> et le serveur est chiffré. Sur un site HTTP, n'importe qui sur votre Wi-Fi peut lire
> ce que vous envoyez et recevez — y compris vos mots de passe."

💬 **FORMATEUR — Sur le DNS :**

> "DNS, c'est l'annuaire d'Internet. Votre ordi ne sait pas que google.com c'est
> 142.250.75.78. Il demande à un serveur DNS — souvent celui de votre FAI ou de Google (8.8.8.8).
> Ce serveur répond avec l'adresse. Vous pouvez faire ça vous-mêmes dans le terminal.
> Tapez : `nslookup google.com` — vous allez voir l'IP répondre."

📌 **TECHNIQUE — Faire ensemble dans le terminal :**

```
nslookup google.com
nslookup github.com
nslookup exemple-bidon-qui-nexiste-pas.fr
```

→ Observer : le 3e échoue car le domaine n'existe pas dans le DNS.

---

### ⏰ 13h20 – 15h00 · Ports, pare-feu, Wi-Fi public — Séquence 3.3

⚙️ **ATELIER — Décomposer 5 URL + table des ports**

Écrire au tableau les URL à décomposer :

```
https://github.com/Ikonik-Dev/ppni.git
ftp://files.example.com:21/backup.zip
http://192.168.1.1:8080/admin
ssh://user@server.io:22
https://api.service.com:443/v2/users?limit=10
```

💬 **FORMATEUR :**

> "Un port, c'est comme le numéro de porte dans un immeuble.
> L'adresse IP, c'est l'immeuble. Le port, c'est l'appartement.
> Si tu arrives à l'immeuble et que tu cherches le service HTTP,
> tu sonnes à la porte 80. Pour HTTPS, c'est la 443.
> Pour SSH — accès distant sécurisé — c'est la 22.
> Un pare-feu, c'est le vigile à l'entrée : il regarde le port de destination
> et la source, et il décide de laisser passer ou pas."

💬 **FORMATEUR — Sur le Wi-Fi public :**

> "Scénario réel : vous êtes dans un café, Wi-Fi ouvert, pas de mot de passe.
> Quelqu'un dans le même café peut lancer un logiciel d'écoute réseau
> et voir TOUT ce qui passe en clair. Mots de passe, messages, identifiants.
> Deux règles simples : 1/ n'utilisez que des sites HTTPS sur un Wi-Fi public.
> 2/ Utilisez un VPN si vous devez faire quelque chose de sensible.
> Un VPN crée un tunnel chiffré de votre machine jusqu'au serveur VPN —
> même sur un Wi-Fi compromis, l'attaquant ne voit qu'un flux chiffré."

---

### ⏰ 15h00 – 17h00 · TP Terminal Réseau — Séquence 3.4

⚙️ **ATELIER — 4 commandes à maîtriser**

Faire chaque commande ensemble, puis en binôme :

```powershell
# 1. Tester la connectivité
ping google.com
ping 8.8.8.8

# 2. Voir le chemin des paquets
tracert google.com
tracert github.com

# 3. Résolution DNS manuelle
nslookup google.com
nslookup 8.8.8.8

# 4. Configuration réseau complète
ipconfig /all
```

💬 **FORMATEUR :**

> "Ping envoie 4 paquets et mesure le temps de réponse.
> 'TTL=113' ça veut dire que le paquet a traversé au moins 15 nœuds
> (128 - 113 = 15 sauts depuis un serveur Windows).
> Si vous voyez 'Délai d'attente de la demande dépassé', c'est que le serveur
> bloque les pings — pas forcément qu'il est éteint.
> Tracert, c'est ping sur chaque nœud du chemin. Vous allez voir
> chaque routeur par lequel votre donnée passe, avec sa latence.
> C'est fascinant — parfois votre data traverse 3 continents pour aller à un serveur en France."

📌 **TECHNIQUE — Exercice binôme :**
Chaque binôme trace la route vers un site différent (netflix.com, amazon.fr, wikipedia.org).
Compter le nombre de sauts, noter la latence max. Présentation de 2 min chacun.

---

## S3 · Vendredi 29 mai — 09h00 → 15h00

### ⏰ 09h00 – 10h30 · Quiz réseaux — Séquence 3.5

⚙️ **ATELIER — Quiz réseaux interactif**

Ouvrir le fichier : `exercices/quiz-reseaux.html`
→ 20 QCM en 3 sections (IP, DNS/HTTP, Ports & Sécurité)
→ Laisser 15-20 minutes pour faire le quiz individuellement
→ Pas d'aide pendant le quiz — noter les questions qui posent problème

💬 **FORMATEUR — Avant le quiz :**

> "Ce quiz n'est pas une évaluation notée. C'est un outil pour vous.
> Il va vous montrer exactement où vous en êtes sur les notions de S3.
> Faites-le honnêtement, sans aide. Le score s'affiche à la fin avec la correction.
> Ce qui vous a posé problème, c'est ce qu'on va retravailler ensemble juste après."

💬 **FORMATEUR — Après le quiz (correction collective) :**

> "Qui a eu 18 ou plus ? Qui a eu moins de 10 ? Quelle question a posé le plus de problème ?
> On va reprendre les 3 questions les plus ratées dans le groupe — je veux que
> ce soit vous qui expliquez la réponse correcte, pas moi."

📌 **TECHNIQUE — Identifier les questions difficiles :**
Faire un tour de table rapide : chacun cite sa question la plus difficile.
Regrouper les difficultés communes. Reprendre les 2-3 notions clés.

---

### ⏰ 10h30 – 12h30 · Du réseau au Web — Séquence 3.6

⚙️ **ATELIER — "Afficher la source" et DevTools**

```
Ctrl + U  →  afficher la source HTML de n'importe quel site
F12  →  DevTools → onglet Éléments → survoler les éléments
```

💬 **FORMATEUR :**

> "On vient de passer une semaine sur les tuyaux : comment les données voyagent.
> Maintenant, on va parler de ce qui voyage. Le Web, c'est des fichiers.
> Des fichiers HTML, CSS et JavaScript que ton navigateur télécharge puis affiche.
> On connaît déjà le réseau. On connaît déjà GitHub. Maintenant, on va apprendre
> à créer ces fichiers. Et lundi, on commence à coder."

💬 **FORMATEUR — Sur les 3 langages :**

> "HTML c'est le squelette : il dit ce qu'il y a sur la page et dans quel ordre.
> CSS c'est l'habit : il dit comment ça doit avoir l'air.
> JavaScript c'est les muscles : il dit ce qui se passe quand on interagit.
> Sans HTML, pas de page. Sans CSS, la page est moche mais fonctionnelle.
> Sans JS, la page est statique. Les trois ensemble : l'expérience web moderne."

📌 **TECHNIQUE — Manipulation guidée :**

1. Ouvrir DevTools → onglet Éléments sur n'importe quel site
2. Cliquer sur un texte → modifier le contenu dans le panneau (non permanent)
3. Modifier une couleur en cliquant sur la petite boîte colorée dans les styles CSS
4. Montrer que le changement disparaît au rechargement → "C'est uniquement dans votre navigateur, pas sur le serveur"

---

### ⏰ 13h20 – 15h00 · Débriefing S3 + preview S4 — Séquence 3.7

💬 **FORMATEUR :**

> "On a une semaine derrière nous. Je veux qu'on fasse le point ensemble.
> Chacun dit une chose : ce que vous avez appris, et une chose que vous ne
> comprenez pas encore bien. Personne ne sera jugé — c'est une matière nouvelle
> pour tout le monde."

Tour de table (1 min par stagiaire) :

- "Ce que j'ai appris cette semaine : ..."
- "Ce qui m'a le plus surpris : ..."
- "Ce que je veux mieux comprendre : ..."

💬 **FORMATEUR — Preview S4 :**

> "Lundi prochain, on passe à la construction. Vous allez créer une vraie page web.
> Pas un exercice bidon — une landing page à votre nom, avec votre style,
> publiée sur GitHub Pages avec une URL que vous pouvez partager.
> Défi optionnel pour le week-end : lisez la fiche HTML/CSS dans vos ressources.
> 10 minutes suffisent. Ça vous donnera une longueur d'avance lundi matin."

📌 **RAPPEL :** Envoyer le lien `ressources/fiche-html-css.html` dans le chat de groupe.

---

## S4 · Lundi 1 juin — 09h00 → 17h00

### ⏰ 09h00 – 10h30 · HTML sémantique — Séquence 4.1

⚙️ **ATELIER — Première page dans VS Code**

```
1. VS Code → Fichier → Nouveau dossier : ppni-landing-page
2. Nouveau fichier : index.html
3. Taper ! puis Tab (Emmet) → structure HTML minimale générée
4. Ajouter entre <body> et </body> :
   - <h1>Mon titre</h1>
   - <p>Mon premier paragraphe.</p>
   - <ul><li>Point 1</li><li>Point 2</li></ul>
   - <a href="https://github.com">Mon GitHub</a>
5. Double-clic sur le fichier → s'ouvre dans le navigateur
```

💬 **FORMATEUR :**

> "HTML, ça veut dire HyperText Markup Language. 'Markup' = balisage.
> On entoure le contenu avec des balises pour dire au navigateur ce que c'est.
> h1 = titre de niveau 1. p = paragraphe. a = lien.
> Ce sont des instructions pour le navigateur — pas du code qui s'exécute,
> juste de la description de contenu.
> La balise DOCTYPE en haut : c'est juste pour dire au navigateur
> 'ce fichier suit la norme HTML5'. Sans ça, certains navigateurs anciens
> affichent les pages différemment."

💬 **FORMATEUR — Sur la sémantique :**

> "HTML5 a introduit des balises sémantiques : nav, main, article, section, footer.
> Pourquoi ? Parce qu'un screen reader (pour malvoyants) ou un moteur de recherche
> lit le HTML. Si tout est dans des div, il ne peut pas savoir si c'est un menu
> ou du contenu. Avec nav, il sait que c'est une navigation. Avec footer, un pied de page.
> C'est l'accessibilité et le SEO — deux raisons concrètes d'utiliser les bonnes balises."

---

### ⏰ 10h30 – 12h30 · CSS — Sélecteurs, box model — Séquence 4.2

⚙️ **ATELIER — Éditeur HTML live + page locale**

Ouvrir `exercices/editeur-html.html`
→ Exercice 1 : modifier couleurs et tailles
→ Exercice 2 : créer une carte avec box-shadow
→ Appliquer ensuite sur `index.html` local

💬 **FORMATEUR :**

> "CSS, c'est une règle en deux parties : un sélecteur, et des déclarations.
> Le sélecteur dit 'à qui ça s'applique'. Les déclarations disent 'quoi changer'.
> `h1 { color: red; font-size: 32px; }` :
> Tous les h1 de ma page seront rouges et auront une taille de 32 pixels.
>
> Le box model, c'est essentiel. Chaque élément HTML est une boîte.
> Du centre vers l'extérieur : le contenu, le padding (espace intérieur),
> la bordure, le margin (espace extérieur).
> Si votre élément prend trop de place, c'est souvent une question de margin ou padding."

💬 **FORMATEUR — Sur les couleurs :**

> "Allez sur coolors.co. Choisissez une palette de 2 couleurs pour votre page.
> Une couleur principale pour les fonds et accents, une couleur de texte.
> Notez les codes hexadécimaux (#RRGGBB) ou les valeurs RGB.
> Ce sera la palette de votre landing page. Cohérence = professionnalisme."

---

### ⏰ 13h20 – 15h00 · Flexbox + responsive — Séquence 4.3

⚙️ **ATELIER — Navbar responsive dans l'éditeur live**

Exercice 4 dans l'éditeur :

```css
/* Desktop : liens côte à côte */
nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px;
}
nav a {
  color: #fff;
  text-decoration: none;
  margin: 0 12px;
}

/* Mobile : liens en colonne */
@media (max-width: 768px) {
  nav {
    flex-direction: column;
    gap: 8px;
  }
}
```

💬 **FORMATEUR :**

> "Flexbox, c'est le système de mise en page moderne du CSS.
> Vous avez un conteneur — vous mettez `display: flex` dessus —
> et tous ses enfants directs deviennent des 'flex items'.
> Vous pouvez les aligner, les espacer, les faire changer de direction.
> C'est la fin de l'ère des tableaux HTML pour la mise en page.
>
> Les media queries, c'est : 'applique ces règles CSS seulement si...'
> le plus courant : seulement si l'écran fait moins de 768px de large.
> C'est comme ça qu'un site devient 'responsive' — il s'adapte à la taille de l'écran."

📌 **TECHNIQUE — Tester en mobile :**
DevTools → Ctrl+Shift+M → choisir "iPhone SE" (375px) → voir les breakpoints en action.

---

### ⏰ 15h00 – 17h00 · JavaScript intro — Séquence 4.4

⚙️ **ATELIER — Bouton interactif dans l'éditeur**

Exercice 5 dans l'éditeur — code de départ à expliquer ligne par ligne :

```html
<button id="monBouton">Cliquez-moi</button>
<p id="texteReponse">Rien pour l'instant.</p>

<script>
  const bouton = document.getElementById("monBouton");
  const texte = document.getElementById("texteReponse");
  let compteur = 0;

  bouton.addEventListener("click", function () {
    compteur++;
    texte.textContent = "Vous avez cliqué " + compteur + " fois.";
  });
</script>
```

💬 **FORMATEUR :**

> "JavaScript, c'est le seul langage de programmation qui s'exécute nativement
> dans le navigateur. HTML et CSS sont des langages de description.
> JS, c'est du vrai code : variables, fonctions, boucles, conditions.
>
> La ligne la plus importante à retenir pour aujourd'hui :
> `document.getElementById('id')` — ça sélectionne un élément HTML par son ID.
> Une fois qu'on l'a, on peut modifier son texte, ses styles, ses classes —
> tout ça sans recharger la page.
> C'est ça, l'interactivité web."

💬 **FORMATEUR — Pour les plus avancés :**

> "Essayez d'ajouter une condition : si le compteur dépasse 5,
> changez la couleur du paragraphe en rouge.
> En JS : `if (compteur > 5) { texte.style.color = 'red'; }`
> C'est votre premier `if` en JavaScript."

---

## S4 · Vendredi 5 juin — 09h00 → 15h00 · JOUR DU LIVRABLE

### ⏰ 09h00 – 10h30 · Construction landing page — Séquence 4.5

⚙️ **ATELIER — Structure guidée collective**

Plan de la landing page à écrire au tableau :

```
┌─────────────────────────────────────┐
│  <nav>  Logo     Liens navigation   │
├─────────────────────────────────────┤
│  <section id="hero">                │
│    <h1>Titre principal</h1>         │
│    <p>Sous-titre accrocheur</p>     │
│    <a>Bouton CTA</a>                │
├─────────────────────────────────────┤
│  <section id="atouts">              │
│  [Card 1]  [Card 2]  [Card 3]       │
├─────────────────────────────────────┤
│  <footer>  Nom | GitHub | Contact   │
└─────────────────────────────────────┘
```

💬 **FORMATEUR :**

> "Aujourd'hui, pas de nouvel apprentissage théorique. Aujourd'hui, on construit.
> Vous avez tout ce qu'il faut. Vous savez faire du HTML sémantique,
> du CSS avec Flexbox et responsive, et du JS interactif.
> L'objectif : envoyer une URL GitHub Pages au formateur avant 12h30.
> Pas besoin que ce soit parfait — besoin que ce soit en ligne.
> Commencez par l'HTML pur, sans CSS. Une structure propre d'abord."

📌 **TECHNIQUE — Minima obligatoires :**

- [ ] Nav + Hero + 1 section + Footer
- [ ] Un fichier CSS séparé (`style.css`) lié avec `<link>`
- [ ] Au moins 1 media query `@media (max-width: 768px)`
- [ ] Au moins 1 bloc JavaScript (bouton, animation, menu mobile…)

---

### ⏰ 10h30 – 12h30 · GitHub Pages — Séquence 4.6

⚙️ **ATELIER — Déploiement pas à pas**

```
1. GitHub.com → "New repository"
   Nom : ppni-landing-page
   ✅ Public  ✅ Add a README  → Create repository

2. GitHub Desktop → Clone le repo → mettre les fichiers HTML/CSS/JS dedans

3. GitHub Desktop :
   - Summary : "Initial commit : landing page HTML/CSS/JS"
   - Commit to main → Push origin

4. Sur GitHub.com → Settings → Pages
   Source : Deploy from a branch → Branch : main → / (root) → Save

5. Attendre ~2 min → l'URL apparaît :
   https://[username].github.io/ppni-landing-page/

6. Envoyer cette URL dans le chat de groupe
```

💬 **FORMATEUR :**

> "GitHub Pages, c'est un hébergement gratuit pour les sites statiques.
> Statique = HTML, CSS, JS — pas de base de données, pas de PHP, pas de backend.
> Ce que vous venez de faire, c'est exactement ce que font des centaines de milliers
> de développeurs pour déployer des portfolios, des documentations, des landing pages.
> À chaque fois que vous pusherez sur la branche main, votre site se mettra à jour
> automatiquement en moins de 2 minutes. C'est du déploiement continu."

💬 **FORMATEUR — Sur HTTPS :**

> "Remarquez l'URL : elle commence par HTTPS. GitHub vous offre automatiquement
> un certificat TLS — ce dont on a parlé lundi dernier en S3.
> La boucle est bouclée : vous savez ce que ça veut dire techniquement,
> et vous l'avez maintenant sur votre propre site."

📌 **TECHNIQUE :** S'assurer que chaque stagiaire a son URL `github.io` avant 12h00.

---

### ⏰ 13h20 – 15h00 · Présentations + bilan — Séquence 4.7

⚙️ **FORMAT — Présentation flash (2 min par stagiaire)**

Déroulé par stagiaire :

1. Ouvrir sa landing page à l'écran + montrer l'URL GitHub Pages
2. Dire : "J'ai choisi ce thème parce que…"
3. Montrer une chose dont il/elle est fier(ère)
4. Montrer le repo GitHub : nombre de commits, messages
5. Dire : "La chose la plus difficile a été…"

Formateur : 1 retour positif + 1 suggestion d'amélioration par stagiaire.

💬 **FORMATEUR — Bilan collectif :**

> "En deux semaines, vous avez :
> — compris comment Internet fonctionne (IP, DNS, HTTPS, ports)
> — créé une vraie page web avec HTML, CSS et JavaScript
> — déployé en ligne avec Git + GitHub Pages
> C'est le socle du numérique. Tout le reste — cybersécurité avancée,
> bases de données, frameworks — s'appuie sur ce que vous savez maintenant.
>
> La semaine prochaine, on rentre dans la phase d'orientation.
> Certains vont approfondir la cybersécurité, d'autres le développement web.
> Mais tous, vous repartez avec quelque chose de concret : une URL publique
> et un commit sur GitHub. C'est votre première trace numérique professionnelle."

📌 **DERNIÈRE ACTION :** Collecter toutes les URLs GitHub Pages dans un document partagé.

---

## 📎 Annexes

### Réponses correctes — Quiz Réseaux (corrigé formateur)

| Q   | Réponse | Explication clé                                                |
| --- | ------- | -------------------------------------------------------------- |
| Q1  | b       | IP = identifiant unique sur un réseau                          |
| Q2  | b       | 192.168.1.1 — seule adresse valide (4 octets 0-255)            |
| Q3  | b       | IPv4 = 32 bits, IPv6 = 128 bits                                |
| Q4  | b       | TCP garantit la livraison (accusé de réception)                |
| Q5  | b       | IP privée = réseau local seulement (non routable sur Internet) |
| Q6  | b       | ICMP = protocole de ping                                       |
| Q7  | b       | Masque = définit la plage réseau                               |
| Q8  | b       | UDP = rapide mais sans garantie (streaming, DNS)               |
| Q9  | b       | DNS = Domain Name System                                       |
| Q10 | b       | DNS traduit nom → IP                                           |
| Q11 | c       | HTTP 200 = succès                                              |
| Q12 | c       | HTTP 404 = ressource non trouvée                               |
| Q13 | c       | HTTP 500 = erreur interne du serveur                           |
| Q14 | b       | HTTPS = HTTP + TLS (chiffrement)                               |
| Q15 | c       | Port 80 = HTTP                                                 |
| Q16 | b       | Port 443 = HTTPS                                               |
| Q17 | c       | Port 22 = SSH                                                  |
| Q18 | b       | Pare-feu = filtre le trafic réseau                             |
| Q19 | b       | VPN = tunnel chiffré                                           |
| Q20 | c       | Wi-Fi public = risque man-in-the-middle                        |

### Commandes réseau de référence (Windows)

```powershell
ipconfig /all              # Configuration réseau complète
ping google.com            # Test connectivité
ping 8.8.8.8               # Test connectivité IP directe
tracert google.com         # Tracer le chemin des paquets
nslookup google.com        # Résolution DNS
nslookup -type=MX gmail.com  # Enregistrements mail DNS
netstat -an | findstr :80  # Services écoutant sur le port 80
```

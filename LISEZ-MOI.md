# Alveare Architecture — site statique (charte Aqua Design)

Archive prête à déposer chez un hébergeur web classique. Aucune base de données, aucun PHP requis : uniquement des fichiers HTML, CSS, SVG et JPEG.

## Contenu

| Fichier | Rôle |
|---|---|
| `index.html` | Accueil |
| `patrimoine.html`, `moex.html`, `environnement.html` | Les trois pôles |
| `journal.html` | Liste des articles (liens vers les articles encore hébergés sur Wix) |
| `contact.html` | Coordonnées et formulaire (ouvre un courriel pré-rempli) |
| `mentions-legales.html` | Mentions légales — rubrique Hébergement à compléter |
| `404.html` | Page introuvable |
| `style.css` | Feuille de style unique |
| `assets/img/` | Photos (19 fichiers, 4,3 Mo) ; `assets/favicon.svg` |
| `logo/` | Logo vectoriel (SVG encre, blanc, fond grège) et PNG haute définition, favicon 256 px — hors site, pour vos documents |
| `.htaccess` | Apache (OVHcloud, o2switch, Infomaniak) : HTTPS forcé, redirections 301 des anciennes adresses Wix, page 404, cache |
| `_redirects` | Même chose pour Netlify / Cloudflare Pages |
| `robots.txt`, `sitemap.xml` | Référencement |

Les polices (Italiana, Inter, DM Mono) sont chargées depuis Google Fonts ; pour un hébergement 100 % local, télécharger les fichiers WOFF2 et remplacer la ligne `@import` en tête de `style.css`.

## Mise en ligne chez OVHcloud (recommandé : le domaine y est déjà)

1. Commander un hébergement web **Perso** (ou Kimsufi/Starter si une seule adresse suffit) dans l'espace client OVHcloud, en l'associant au domaine `alveare-architecture.fr`.
2. Dans *Hébergements › Multisite*, vérifier que `alveare-architecture.fr` et `www.alveare-architecture.fr` sont déclarés, dossier racine `www`, SSL Let's Encrypt activé.
3. Déposer **tout le contenu** de cette archive (y compris `.htaccess`) dans le dossier `www` via FTP (identifiants dans *Hébergements › FTP-SSH*), avec FileZilla ou l'explorateur de fichiers OVH.
4. Dans *Domaines › alveare-architecture.fr › Zone DNS*, appliquer les enregistrements ci-dessous.
5. Attendre la propagation (quelques minutes à 24 h), vérifier `https://www.alveare-architecture.fr`, puis résilier l'abonnement Wix **après** avoir migré les articles du Journal.

## Enregistrements DNS

La zone DNS est gérée chez OVH (serveurs `dns108.ovh.net` / `ns108.ovh.net`). On modifie uniquement les enregistrements **A** et **CNAME** du site ; on ne touche pas aux **MX**, **TXT/SPF/DKIM** (courrier) ni aux serveurs de noms.

### À supprimer (pointage actuel vers Wix)

| Type | Sous-domaine | Cible actuelle |
|---|---|---|
| A | `@` (vide) | 185.230.63.107 |
| A | `@` (vide) | 185.230.63.171 |
| A | `@` (vide) | 185.230.63.186 |
| A ou CNAME | `www` | mêmes IP Wix / `*.wixdns.net` |

### À créer selon l'hébergeur retenu

**OVHcloud (hébergement web dans le même compte)** — l'IP est fournie dans *Hébergements › Informations générales › Adresse IPv4* ; OVH propose souvent de la configurer automatiquement lors de la commande.

| Type | Sous-domaine | Cible |
|---|---|---|
| A | `@` | IPv4 de l'hébergement (ex. 213.186.33.x, à lire dans l'espace client) |
| CNAME | `www` | `alveare-architecture.fr.` |

**Netlify**

| Type | Sous-domaine | Cible |
|---|---|---|
| A | `@` | `75.2.60.5` |
| CNAME | `www` | `<nom-du-site>.netlify.app.` |

**Cloudflare Pages**

| Type | Sous-domaine | Cible |
|---|---|---|
| CNAME | `www` | `<projet>.pages.dev.` |
| @ | — | Cloudflare demande en pratique de déléguer la zone à ses serveurs de noms pour l'apex ; sinon, rediriger `alveare-architecture.fr` vers `www` depuis OVH (redirection visible). |

**GitHub Pages**

| Type | Sous-domaine | Cible |
|---|---|---|
| A | `@` | `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153` (4 enregistrements) |
| CNAME | `www` | `<compte>.github.io.` |

**Vercel**

| Type | Sous-domaine | Cible |
|---|---|---|
| A | `@` | `76.76.21.21` (vérifier la valeur affichée dans le projet) |
| CNAME | `www` | `cname.vercel-dns.com.` |

Dans tous les cas : TTL par défaut, et déclarer le domaine dans l'interface de l'hébergeur pour que le certificat HTTPS soit émis.

## Points restant à traiter

- **Courrier** : si `dguido@alveare-architecture.fr` est géré via Wix (Google Workspace souscrit chez Wix), le service continue tant que l'abonnement Google reste actif, indépendamment du site ; vérifier dans la zone DNS que les MX sont bien présents avant de toucher quoi que ce soit.
- **Journal** : les cinq articles sont des pages Wix (`/journal/...`). Les récupérer (texte + images) avant de résilier Wix ; je peux les intégrer au gabarit du site.
- **Formulaire** : actuellement, l'envoi ouvre la messagerie du visiteur. Sur OVH (PHP disponible) ou avec un service comme Formspree / Web3Forms, un envoi direct est possible.
- **Mentions légales** : compléter la rubrique Hébergement (raison sociale et adresse de l'hébergeur).
- **Réseau d'échange** (groupe Wix) et **liens LinkedIn / Instagram** : à décider / à fournir.

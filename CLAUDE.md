# CLAUDE.md — Défi Winner 30J

## Démarrage rapide

```bash
# Charger le token Circle avant tout appel API
export $(cat .env | xargs)

# Vérifier
curl -s https://app.circle.so/api/admin/v2/spaces \
  -H "Authorization: Token $CIRCLE_API_TOKEN" | python3 -m json.tool | head -20
```

> Pas de build system — HTML/CSS/JS pur. Ouvrir les fichiers directement dans le navigateur ou avec n'importe quel serveur statique.

---

## Contexte projet

**Projet :** Le Défi Winner 30J — challenge e-commerce 4 semaines pour la communauté Tariqa PRO / CTP
**Niche imposée :** Produits pour enfants (sommeil bébé, éveil/éducatif, sécurité, confort)
**Plateforme e-commerce :** Shopify
**Communauté :** Circle
**GitHub Pages :** https://shortcut-click.github.io/defi-winner-30j/
**Repo GitHub :** https://github.com/shortcut-click/defi-winner-30j (branche `master`)

---

## Credentials & accès

| Service | Info | Emplacement |
|---|---|---|
| Circle API token | Dans `.env` à la racine du projet | `CIRCLE_API_TOKEN=...` |
| Circle space challenge | ID `2307817` — "Le Défi Winner 30J" | |
| Google Drive livrables | ID `13uhqNKYS0twk9c8SpS7rEbaBG9ddX166` | |
| GitHub | Branche `master` → Pages automatique | |

**Posts Circle (IDs) :**
- Bienvenue : `31304201`
- Semaine 1 : `31304203`
- Semaine 2 : `31304205`
- Semaine 3 : `31304206`
- Semaine 4 : `31304207`

**Circle API — format PATCH posts :**
```
PATCH https://app.circle.so/api/admin/v2/posts/{id}
body: { "tiptap_body": { "body": { "type": "doc", "content": [...] }, ... } }
```

---

## Quirks connus

- **Circle API succès** : un PATCH réussi retourne `{"message": "Post updated."}` — pas un objet post standard. Traiter `message == "Post updated."` comme succès.
- **Encodage Windows** : les scripts Python manipulant des emojis doivent passer par `sys.stdout.buffer.write(result.encode('utf-8'))` — le codec cp1252 de Windows plante sinon.
- **`.env` protégé** : `.gitignore` inclut déjà `.env`. Ne jamais committer le token Circle.

---

## Inventaire fichiers HTML

| Fichier | Rôle |
|---|---|
| `index.html` | Hub central de navigation |
| `challenge-deck.html` | Slides présentation du challenge |
| `challenge-scorecard.html` | Tableau scores live (localStorage) |
| `s1-framework-produit.html` | Framework Produit Winner (P1 qualifier, P2 valider, P3 agent sourcing, persona) |
| `s1-guide-methode.html` | Guide pas à pas S1 |
| `s1-template-fiche-produit.html` | Template fiche produit (marge ×4 auto, localStorage) |
| `s1-template-persona.html` | Template persona (3 onglets, scoring, localStorage) |
| `s1-ads-library-keywords.html` | 150 mots-clés META Ads Library (8 clusters, recherche, copier) |
| `s2-template-page-produit.html` | Template page produit Shopify (bridge Persona→Copy, localStorage) |
| `s2-guide-methode.html` | Guide Shopify (compte, produit, légal, pixel, test) |
| `s3-framework-creas.html` | Framework créas pub (angles, hooks, formats) |
| `s3-guide-methode.html` | Guide Canva + CapCut + Meta Ads Manager |
| `s4-guide-lancement-meta.html` | Guide lancement META (étape 0 pixel, 3 scénarios post-lancement) |

---

## Règles techniques impératives

### DOM-safe (hook sécurité actif)
**Ne JAMAIS utiliser `innerHTML` avec des données dynamiques.**
Toujours construire le DOM avec :
```javascript
const el = document.createElement('div');
el.textContent = userValue; // jamais innerHTML = userValue
parent.appendChild(el);
```
Pour les `<select>` : `new Option(text, value)`.

### localStorage
- Clé par fichier : `defi_fiche_produit`, `defi_fiche_persona`, `defi_template_s2`, `defi_guide_s4`
- Pattern : IIFE auto-exécutée à `DOMContentLoaded`
- Restore des boutons de sélection par matching `textContent` ou attribut `onclick`

### Design system
- Background : `#0f0f0f`
- Surface : `#1a1a1a`
- Vert principal : `#1D9E75`
- Police : `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif`
- Tous les fichiers utilisent le même dark theme cohérent

### Navigation
- Logo header = `<a href="index.html" class="logo">← Tariqa PRO</a>`
- CTA inter-semaines = liens `<a href="sX-fichier.html">` (jamais des `<span>` non cliquables)

---

## Décisions clés à ne pas réintroduire

| Décision | Règle |
|---|---|
| Stock | Pas de stock en France. Agent en Chine → expédie directement au client final |
| MOQ | Idéal = aucun. Si imposé : < 10 unités, 20 max. Jamais 50-100 avant premières ventes |
| Phase 3 S1 | Agent sourcing (trouver / brief+MOQ / process / test unitaire) — PAS un test pub META |
| Persona | C'est l'angle de la créa, pas un filtre dans le gestionnaire de pubs |
| Framework | P1 : 5 filtres + douleur 1/2/3 → P2 : marge ×4 + demande + compétition → P3 : agent |

---

## Workflow Git

```bash
cd "C:\Users\hamza\Desktop\Travail\Tariqa Pro\Cours Ecommerce CTP\challenge-winner-30j"
git add fichier.html
git commit -m "description"
git push origin master
# → GitHub Pages se met à jour automatiquement (~1 min)
```

**Ne jamais committer `.env`** (contient le token Circle).

---

## Google Drive — structure livrables

```
📁 Défi Winner 30J — Livrables  (ID: 13uhqNKYS0twk9c8SpS7rEbaBG9ddX166)
   └── 📋 DOSSIER DE BASE — À COPIER  (ID: 1_Tc_guoXHdIcE2WYCKfNJ9V7ZyV4hpkP)
         ├── S1 — Sourcing & Persona
         ├── S2 — Site Shopify
         ├── S3 — Créas Pub
         └── S4 — Lancement META
```

Permissions : "Toute personne disposant du lien" → Éditeur
Les participants dupliquent "DOSSIER DE BASE" et le renomment à leur nom.

---

## Ressources externes

- **Google Doc suivi des tâches :** https://docs.google.com/document/d/1j9R-ExAuXR1wgXDFNfMgzn-oR7PiLVVWyRBmB45aiqg/edit
- **GitHub Pages :** https://shortcut-click.github.io/defi-winner-30j/
- **Google Drive livrables :** https://drive.google.com/drive/folders/13uhqNKYS0twk9c8SpS7rEbaBG9ddX166

## Agents disponibles

- `Pedagogue` — expert pédagogie & formation en ligne → `~/.claude/agents/pedagogue.md`

## Tâches restantes

- [ ] Google Sheets scoring (Participants / S1→S4 / Classement =RANK())
- [ ] Exemple fil rouge produit fictif traversant les 4 semaines (rédactionnel)
- [ ] Restructuration spaces Circle (décisions Hamza nécessaires avant action)
- [ ] Hébergement Netlify pour embeds iframe Circle (scorecard, deck) — optionnel

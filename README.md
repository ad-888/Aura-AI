# Aura-AI

Assistant personnel IA en français pour Adrien, propulsé par Claude (Anthropic). PWA installable, fonctionne directement dans le navigateur sans backend.

**URL de l'app :** `https://ad-888.github.io/Aura-AI/`

---

## Configuration rapide

1. Obtenez une clé API sur [platform.claude.com](https://platform.claude.com) → **Clés API** → créer une clé
2. Copiez la clé **immédiatement** (elle ne s'affiche qu'une seule fois)
3. Dans Aura, cliquez sur **⚙** en haut à droite → collez la clé → **Enregistrer**

La clé est stockée uniquement dans le `localStorage` du navigateur.

---

## Architecture

```
Aura-AI/
├── index.html      # Application complète (HTML + CSS + JS inline)
├── Sw.js           # Service Worker (cache PWA hors-ligne)
├── manifest.json   # Manifeste PWA (icônes, nom, couleurs)
└── icons/          # Icônes PWA (192px, 512px) — à créer
```

**Stack :** HTML/CSS/JS vanilla, zéro dépendance, zéro build.  
**Déploiement :** GitHub Pages sur la branche `claude/fix-api-key-issue-B5n2Y`.  
**Modèle IA :** `claude-sonnet-4-6` via appel direct navigateur (header `anthropic-dangerous-direct-browser-access: true`).

---

## Fonctionnalités actuelles

| Fonctionnalité | Détail |
|---|---|
| 4 modes | Général, Professionnel, Créatif, Planification |
| Actions rapides | Email, planification, résumé, brainstorming, correction |
| Contexte personnel | École Black Jack, Loge Parfaite Amitié |
| Historique de session | Conservé en mémoire (perdu au rechargement) |
| PWA installable | Icône sur l'écran d'accueil iOS/Android |
| Offline | Assets mis en cache par le Service Worker |

---

## Pistes d'amélioration

### Expérience utilisateur
- [ ] **Historique persistant** — sauvegarder les conversations dans `localStorage`
- [ ] **Bouton nouvelle conversation** — réinitialiser `hist` et afficher le welcome screen
- [ ] **Streaming des réponses** — afficher le texte mot par mot (API SSE)
- [ ] **Markdown complet** — rendre les listes, titres, code (ex : bibliothèque `marked.js`)
- [ ] **Copier une réponse** — bouton copier sur chaque bulle IA
- [ ] **Icônes PWA** — créer `icons/icon-192.png` et `icons/icon-512.png`

### Modes et personnalisation
- [ ] **Nouveaux modes** — Juridique, Santé, Langue étrangère…
- [ ] **Éditer les system prompts** — interface pour personnaliser chaque mode
- [ ] **Température / max_tokens** — curseurs dans les paramètres
- [ ] **Choisir le modèle** — Haiku (rapide/économique) vs Sonnet vs Opus

### Technique
- [ ] **Mémoire longue durée** — résumé automatique des anciennes conversations
- [ ] **Export conversation** — télécharger en `.txt` ou `.md`
- [ ] **Raccourcis clavier** — `Ctrl+K` nouvelle conversation, `Ctrl+M` changer de mode
- [ ] **Multi-langue** — basculer FR/EN dans les paramètres

---

## Développement

Aucun build nécessaire — modifier `index.html` directement.

```bash
# Lancer un serveur local
python3 -m http.server 8080
# puis ouvrir http://localhost:8080
```

**Branche de travail :** `claude/fix-api-key-issue-B5n2Y`  
Toutes les modifications passent par cette branche, GitHub Pages la sert directement.

### Points techniques importants

- **Appel API direct** : utilise `anthropic-dangerous-direct-browser-access: true` pour éviter tout proxy CORS (ne jamais supprimer ce header)
- **Clé API** : jamais dans le code source — toujours via `localStorage.getItem('aura_api_key')`
- **Service Worker** : le fichier s'appelle `Sw.js` (S majuscule) — la registration dans `index.html` doit rester `'Sw.js'` (sensible à la casse sur Linux/GitHub Pages)
- **Historique** : tableau `hist` en mémoire, format `[{role, content}]` — envoyé intégralement à chaque requête

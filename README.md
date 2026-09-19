<<<<<<< HEAD
# 📘 Guide Complet des Commandes Git

> Référence rapide — de l'installation au déploiement

---

## 1. Installation et Vérification

```bash
git --version                  # Vérifier la version installée
sudo apt install git           # Linux (Debian/Ubuntu)
brew install git               # macOS
```

---

## 2. Configuration (à faire une seule fois par machine)

```bash
# Identité (obligatoire pour les commits)
git config --global user.name "Votre Nom"
git config --global user.email "votre@email.com"

# Éditeur par défaut
git config --global core.editor "code --wait"   # VS Code
git config --global core.editor "nano"

# Voir toute la configuration
git config --list
git config user.name           # Voir une valeur précise

# Raccourci pratique : alias
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.lg "log --oneline --graph --decorate --all"
```

---

## 3. Créer / Cloner un Dépôt

```bash
# Démarrer un nouveau projet
git init                       # Crée un dépôt dans le dossier courant
git init mon-projet            # Crée le dossier + dépôt

# Récupérer un projet existant
git clone https://github.com/user/repo.git
git clone git@github.com:user/repo.git        # via SSH
git clone <url> mon-dossier                   # dans un dossier nommé différemment
git clone --depth 1 <url>                     # clone léger (sans l'historique complet)
```

---

## 4. Commandes de Base du Quotidien

```bash
git status                     # État des fichiers (modifiés, ajoutés, ignorés)
git add fichier.txt            # Ajouter un fichier à la zone de staging
git add .                      # Ajouter TOUT les modifications
git add -p                     # Ajouter interactivement (par morceaux)

git commit -m "message"        # Enregistrer les changements
git commit -am "message"       # add + commit (fichiers déjà suivis uniquement)
git commit --amend -m "nouveau message"   # Corriger le DERNIER commit
```

### Écrire de bons messages de commit
```
type(scope): description courte

- feat    : nouvelle fonctionnalité
- fix     : correction de bug
- docs    : documentation
- style   : formatage, sans changement de code
- refactor: restructuration sans changement de comportement
- test    : ajout/modification de tests
- chore   : tâches diverses (config, dépendances...)
```

---

## 5. Consulter l'Historique

```bash
git log                        # Historique complet
git log --oneline              # Historique condensé (1 ligne par commit)
git log --oneline --graph --all  # Vue graphique de toutes les branches
git log -n 5                   # Les 5 derniers commits
git log --stat                 # Avec fichiers modifiés
git log --author="Nom"         # Filtrer par auteur
git log --since="2026-01-01"   # Filtrer par date
git log -- fichier.txt         # Historique d'un fichier précis
git log -p                     # Avec les différences (diffs)

git show <commit>              # Détails d'un commit précis
git show HEAD                  # Détails du dernier commit
git diff                       # Modifications NON encore stagées
git diff --staged              # Modifications stagées (prêtes au commit)
git diff main..develop         # Différence entre deux branches
```

---

## 6. Les Branches (le cœur de Git)

```bash
git branch                     # Lister les branches locales
git branch -a                  # Lister toutes les branches (locales + distantes)
git branch ma-branche          # Créer une branche
git branch -d ma-branche       # Supprimer une branche (fusionnée)
git branch -D ma-branche       # Forcer la suppression
git branch -m ancien nouveau   # Renommer une branche

git switch ma-branche          # Changer de branche (moderne)
git switch -c ma-branche       # Créer + changer de branche
git checkout ma-branche        # Changer de branche (ancienne syntaxe)
git checkout -b ma-branche     # Créer + changer (ancienne syntaxe)

# Naviguer vers un commit précis (état "detached HEAD")
git switch --detach <commit>

# Suivre une branche distante
git switch -c ma-branche origin/ma-branche
```

---

## 7. Fusionner (Merge) et Rebaser

```bash
# Pour fusionner ma-branche DANS la branche courante :
git merge ma-branche

git merge --no-ff ma-branche   # Conserver l'historique de la branche
git merge --abort              # Annuler une fusion en conflit

# Rebase : replacer sa branche sur une autre (historique linéaire)
git rebase main                # depuis sa branche de travail
git rebase --continue          # après résolution de conflit
git rebase --abort             # annuler le rebase
git rebase -i HEAD~3           # Rebase interactif (modifier les 3 derniers commits)

# Stratégie courante : garder sa branche à jour avec main
git switch ma-branche
git rebase main
```

### Résoudre des conflits
```bash
# 1. Git signale les fichiers en conflit → les ouvrir
# 2. Chercher les marqueurs :  <<<<<<< HEAD / ======= / >>>>>>>
# 3. Modifier le fichier, garder ce qu'il faut
# 4. Puis :
git add fichier-conflit.txt
git commit -m "resolve conflict"      # (après un merge)
# ou : git rebase --continue          # (après un rebase)
```

---

## 8. Annuler / Corriger des Erreurs

```bash
# Déplacer un fichier oublié du staging vers les modifications
git restore --staged fichier.txt

# Annuler les modifications NON commises d'un fichier (⚠️ irréversible)
git restore fichier.txt

# Revenir au dernier commit propre
git reset --hard HEAD          # ⚠️ perd toutes les modifications non commitées

# Annuler le dernier commit mais GARDER les modifications en staging
git reset --soft HEAD~1

# Annuler le dernier commit et les modifications (garder untracked)
git reset --mixed HEAD~1

# Annuler un commit déjà PARTAGÉ (crée un commit inverse)
git revert <commit>

# Désindexer TOUT ce qui a été ajouté avec `git add .`
git reset
```

---

## 9. Stash (mettre de côté temporairement)

```bash
git stash                      # Mettre de côté les modifications en cours
git stash save "message"       # Avec un message descriptif
git stash list                 # Lister les stashs
git stash pop                  # Récupérer ET supprimer le dernier stash
git stash apply                # Récupérer SANS supprimer
git stash apply stash@{2}      # Récupérer un stash précis
git stash drop stash@{0}       # Supprimer un stash
git stash clear                # Vider tous les stashs
git stash show -p              # Voir le contenu d'un stash
```

---

## 10. Dépôts Distants (Remotes)

```bash
git remote -v                  # Voir les remotes configurés
git remote add origin <url>    # Ajouter un remote
git remote set-url origin <url>  # Modifier l'URL d'un remote
git remote remove origin       # Supprimer un remote
git remote rename old new      # Renommer un remote

git fetch                      # Récupérer les changements distants (sans toucher au code)
git fetch --all                # Depuis tous les remotes

git pull                       # fetch + merge (récupérer et fusionner)
git pull --rebase              # fetch + rebase (historique linéaire)

git push origin main           # Envoyer ses commits vers le remote
git push -u origin main        # Premier push : lie la branche locale au remote
git push --force origin main   # ⚠️ Forcer (à éviter si déjà partagé)
git push --force-with-lease    # Plus sûr que --force

git push origin --delete ma-branche    # Supprimer une branche distante
git push --tags                # Envoyer les tags
```

### Vérifier un remote avant de cloner
```bash
git ls-remote origin           # Tester la connexion à un remote
```

---

## 11. Tags (versions)

```bash
git tag                        # Lister les tags
git tag v1.0.0                 # Créer un tag léger
git tag -a v1.0.0 -m "Version 1.0"   # Créer un tag annoté (recommandé)
git tag -a v1.0.0 <commit>     # Tager un ancien commit
git show v1.0.0                # Voir les détails d'un tag
git push origin v1.0.0         # Envoyer un tag précis
git push origin --tags         # Envoyer tous les tags
git tag -d v1.0.0              # Supprimer un tag local
git push origin --delete v1.0.0  # Supprimer un tag distant
```

---

## 12. Fichiers : Ignorer, Suivre, Supprimer

```bash
# .gitignore → fichiers à ignorer (exemples : node_modules/, *.log, .env)
# Le créer à la racine du projet

echo "*.log" >> .gitignore
echo "node_modules/" >> .gitignore
echo ".env" >> .gitignore

git rm fichier.txt             # Supprimer un fichier ET le commit de suivi
git rm --cached fichier.txt    # Arrêter de suivre un fichier (garde le fichier local)
git mv ancien.txt nouveau.txt  # Renommer/déplacer

# Si un fichier suivi doit être ignoré APRÈS coup :
git rm --cached fichier.txt
echo "fichier.txt" >> .gitignore
```

### Modèle de `.gitignore` typique
```
# Dépendances
node_modules/
vendor/

# Fichiers d'environnement
.env
.env.local

# Logs
*.log

# Système / Éditeurs
.DS_Store
.idea/
.vscode/
__pycache__/
```

---

## 13. Commandes Avancées Utiles

```bash
git cherry-pick <commit>       # Appliquer un commit précis sur la branche courante

git clean -n                   # Voir les fichiers non suivis qui seraient supprimés
git clean -fd                  # Supprimer les fichiers/dossiers non suivis (⚠️)

git blame fichier.txt          # Qui a écrit chaque ligne, et quand

git reflog                     # Journal de TOUTES les actions (sauvetage !)
                               # Permet de récupérer un commit "perdu"

git shortlog -sn               # Nombre de commits par auteur

git archive --format=zip HEAD -o projet.zip   # Exporter le projet en zip

git gc                         # Optimiser la base de données du dépôt

git count-objects -vH          # Taille du dépôt

git bisect start               # Trouver le commit qui a introduit un bug (recherche binaire)
git bisect bad                 # Marquer le commit actuel comme "mauvais"
git bisect good <commit>       # Marquer un ancien commit comme "bon"
# → Git se place sur un commit à tester. Répéter good/bad jusqu'au coupable.
git bisect reset               # Sortir du mode bisect
```

---

## 14. Flux de Travail Typique (Workflow)

### Démarrer un projet
```bash
git init
git add .
git commit -m "chore: initial commit"
git remote add origin git@github.com:user/repo.git
git push -u origin main
```

### Travailler au quotidien
```bash
git switch -c feature/login      # 1. Créer une branche de travail
# ... écrire du code ...
git add .                        # 2. Stager
git commit -m "feat: add login form"  # 3. Committer
git push -u origin feature/login # 4. Pousser
```

### Intégrer sa branche dans main
```bash
git switch main
git pull                          # Synchroniser main
git merge feature/login           # Fusionner (ou : git rebase)
git push origin main
git branch -d feature/login       # Nettoyer
```

### 🌿 GitFlow (modèle professionnel classique)
- `main`      → code en production
- `develop`   → code de développement
- `feature/*` → nouvelles fonctionnalités (partent de `develop`)
- `release/*` → préparation d'une version
- `hotfix/*`  → correctifs urgents en production (partent de `main`)

---

## 15. Gros Fichiers : Git LFS

```bash
git lfs install                          # Activer LFS (une fois par machine)
git lfs track "*.psd"                    # Suivre les gros fichiers
git add .gitattributes
git commit -m "chore: track large files with LFS"
```

---

## 16. Aide Mémoire : Danger vs Sans Danger

| ✅ Sans danger (local)         | ⚠️ Dangereux (perte possible)      |
|-------------------------------|-------------------------------------|
| `git status`                  | `git reset --hard`                  |
| `git diff`                    | `git checkout -- fichier`           |
| `git log`                     | `git push --force`                  |
| `git fetch`                   | `git clean -fd`                     |
| `git stash`                   | `git branch -D`                     |
| `git switch`                  | Réécrire l'historique déjà poussé   |

---

## 17. Sauvetage : Récupérer un Commit "Perdu"

```bash
git reflog                     # Trouver le hash du commit perdu
git switch -c rescue <hash>    # Le récupérer dans une nouvelle branche
```

---

*Document généré le 19 septembre 2026 — Git v2.x*
=======
le site exemple du cours
>>>>>>> d0509f2528efe31330c0209ab1c89e7a1f9ca4e6

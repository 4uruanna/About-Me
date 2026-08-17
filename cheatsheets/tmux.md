# 🧩 tmux Cheatsheet — Commandes de base

---

## 🔑 **Préfixe par défaut**
`Ctrl+b` → Préfixe par défaut pour toutes les commandes tmux.
*Exemple :* `Ctrl+b` + `c` = nouvelle fenêtre.

---

---

## 🖥️ **Sessions**
   Commande | Action |
 |----------|--------|
 | `tmux` | Démarrer une nouvelle session |
 | `tmux new -s <nom>` | Créer une session nommée |
 | `Ctrl+b` + `d` | Détacher de la session (la laisser tourner en arrière-plan) |
 | `tmux ls` | Lister toutes les sessions |
 | `tmux attach -t <nom>` | Rejoindre une session existante |
 | `tmux attach` | Rejoindre la dernière session |
 | `Ctrl+b` + `$` | Renommer la session courante |
 | `tmux kill-session -t <nom>` | Tuer une session |

---

## 🪟 **Fenêtres (Tabs)**
 | Commande | Action |
 |----------|--------|
 | `Ctrl+b` + `c` | Créer une nouvelle fenêtre |
 | `Ctrl+b` + `n` | Aller à la fenêtre suivante |
 | `Ctrl+b` + `p` | Aller à la fenêtre précédente |
 | `Ctrl+b` + `0-9` | Aller à la fenêtre numéro *N* |
 | `Ctrl+b` + `,` | Renommer la fenêtre courante |
 | `Ctrl+b` + `&` | Fermer la fenêtre courante |
 | `Ctrl+b` + `w` | Menu interactif pour choisir une fenêtre |

---

## ✨ **Panneaux (Splits)**
 | Commande | Action |
 |----------|--------|
 | `Ctrl+b` + `%` | Diviser verticalement |
 | `Ctrl+b` + `"` | Diviser horizontalement |
 | `Ctrl+b` + `↑↓←→` | Changer de panneau |
 | `Ctrl+b` + `o` | Cycler à travers les panneaux |
 | `Ctrl+b` + `x` | Fermer le panneau courant |
 | `Ctrl+b` + `:` + `swap-pane -s <source> -t <target>` | Échanger deux panneaux |
 | `Ctrl+b` + `Alt+↑↓←→` | Redimensionner un panneau (maintenir `Alt`) |
 | `Ctrl+b` + `Space` | Changer de disposition (layout) |
 | `Ctrl+b` + `!` | Convertir un panneau en fenêtre |
 | `Ctrl+b` + `{` | Déplacer un panneau vers la gauche |
 | `Ctrl+b` + `}` | Déplacer un panneau vers la droite |

---

## 🔍 **Autres commandes utiles**
 | Commande | Action |
 |----------|--------|
 | `Ctrl+b` + `:` | Entrer le mode commande (ex: `list-sessions`) |
 | `Ctrl+b` + `[` | Mode copie (déplacer avec `↑↓←→`, copier avec `Space`, coller avec `Ctrl+b` + `]`) |
 | `Ctrl+b` + `?` | Afficher toutes les raccourcis |
 | `Ctrl+b` + `t` | Afficher l'heure actuelle |
 | `Ctrl+b` + `:` + `set -g mouse on` | Activer la souris (pour redimensionner, changer de panneau) |
 | `Ctrl+b` + `:` + `set -g prefix C-a` | Changer le préfixe en `Ctrl+a` |
 | `Ctrl+b` + `:` + `set -g base-index 1` | Commencer la numérotation des fenêtres à 1 |
 | `Ctrl+b` + `:` + `set -g pane-base-index 1` | Commencer la numérotation des panneaux à 1 |
 | `tmux source-file ~/.tmux.conf` | Recharger la configuration |

---
---
## 💡 **Astuces**
- **Copier/coller** : Sélectionner avec `Ctrl+b` + `[`, `Space` pour copier, `Ctrl+b` + `]` pour coller.
- **Rechercher dans le buffer** : `Ctrl+b` + `[` puis `/` (comme dans `vim`).
- **Synchroniser les panneaux** : `Ctrl+b` + `:` + `setw synchronize-panes on` (utile pour envoyer la même commande à tous les panneaux).
# RIFTBORN — Curriculum d'Apprentissage UE5

## Vue d'ensemble

Ce curriculum est conçu pour un développeur **débutant absolu** en code et en game dev.
Durée totale : **10 semaines** (temps plein, 30–40h/semaine).
Objectif final : être capable de commencer à implémenter le MVP RIFTBORN en UE5.

> **Principe directeur** : Ne jamais toucher au projet RIFTBORN tant que les exercices de base ne sont pas maîtrisés.

---

## Semaine 1 — Installation et Prise en Main UE5

**Objectifs** :
- Installer Unreal Engine 5 via Epic Games Launcher
- Naviguer dans l'interface (Viewport, Outliner, Details, Content Browser)
- Comprendre les concepts : Actor, Component, Level, Blueprint

**Ressources** :
- [Formation officielle UE5 débutant](https://dev.epicgames.com/community/learning) — "Your First Hour in Unreal Engine 5"
- YouTube : "Unreal Engine 5 Beginner Tutorial" par Unreal Sensei

**Exercice pratique** :
1. Créer un projet "Third Person" de base
2. Placer 10 objets différents dans une scène
3. Changer les matériaux (couleurs) de ces objets
4. Prendre une capture d'écran et la sauvegarder

**Livrable** : Une scène simple avec des objets colorés et organisés

---

## Semaine 2 — Blueprints Fondamentaux

**Objectifs** :
- Comprendre les nœuds Blueprint (Event, Function, Variable)
- Créer son premier Blueprint interactif
- Utiliser les Events : `BeginPlay`, `Tick`, `OnComponentHit`

**Ressources** :
- Epic : "Introduction to Blueprints" (gratuit sur dev.epicgames.com)
- YouTube : "UE5 Blueprint Beginner Tutorial" par Matt Aspland

**Exercice pratique** :
1. Créer une porte qui s'ouvre quand le joueur s'approche
2. Créer une lumière qui change de couleur au fil du temps
3. Ajouter un compteur affiché à l'écran (Print String)

**Livrable** : Projet avec 3 Blueprints interactifs fonctionnels

---

## Semaine 3 — Variables, Fonctions et Composants

**Objectifs** :
- Maîtriser les types de variables (Float, Integer, Boolean, Vector, String)
- Créer des fonctions réutilisables
- Comprendre les Components et leur attachement

**Exercice pratique** :
1. Créer un système de compteur de score (variable Integer)
2. Créer une barre de vie simple pour le joueur (Float 0–100)
3. Faire diminuer la vie avec le temps (TimerByEvent)

**Livrable** : Joueur avec une barre de vie fonctionnelle affichée à l'écran

---

## Semaine 4 — UMG / Interface Utilisateur

**Objectifs** :
- Créer des Widgets Blueprint (UI)
- Afficher des variables en temps réel à l'écran
- Comprendre le Canvas Panel, les Progress Bars, les Text Blocks

**Exercice pratique** :
1. Créer un HUD avec : barre de vie, compteur de pièces, texte de bienvenue
2. Connecter la barre de vie à la variable `Health` du personnage
3. Animer l'apparition du HUD au démarrage (fade in)

**Livrable** : HUD jouable affichant les données du personnage en temps réel

---

## Semaine 5 — IA de Base et Behavior Trees

**Objectifs** :
- Configurer le NavMesh (navigation automatique)
- Créer un Behavior Tree simple (patrouille + attaque)
- Utiliser les AI Controllers et les Blackboards

**Exercice pratique** :
1. Créer un ennemi qui patrouille entre 3 points
2. L'ennemi détecte le joueur dans un rayon de 10m et le poursuit
3. L'ennemi attaque le joueur quand il est à portée (réduit la vie)

**Livrable** : Ennemi IA fonctionnel avec patrouille, détection et attaque

---

## Semaine 6 — Système de Collision et Interaction

**Objectifs** :
- Maîtriser les Collision Channels et les Event Overlaps
- Créer des objets ramassables (pickup items)
- Implémenter un système d'interaction par touche (E)

**Exercice pratique** :
1. Créer 5 objets ramassables (cristaux colorés)
2. Afficher un message "Appuyez sur E" quand le joueur s'approche
3. L'objet disparaît au ramassage et incrémente le compteur

**Livrable** : Système de pickup avec prompt d'interaction et compteur

---

## Semaine 7 — Mini-Jeu Complet (Projet de Consolidation)

**Description du mini-jeu** :
> **"Survivor"** : Le joueur doit collecter 10 cristaux avant que sa vie n'atteigne zéro. Des ennemis patrouillent la zone. Chaque cristal collecté +10 vie. Timer 3 minutes. Écran de victoire / défaite.

**Livrables** :
- Mini-jeu avec menu principal, gameplay, et écran fin
- Au moins 1 type d'ennemi IA
- HUD avec vie, timer, compteur de cristaux
- ≥ 5 minutes de jeu possible sans crash

---

## Semaine 8 — Terrain et Environnement UE5

**Objectifs** :
- Maîtriser le Landscape Tool (sculpture de terrain)
- Utiliser Quixel Bridge pour importer des assets gratuits
- Créer un biome de base (herbe, arbres, rochers)

**Exercice pratique** :
1. Sculpter un terrain 1km × 1km avec collines et vallées
2. Appliquer 3 matériaux de terrain (herbe, terre, roche)
3. Placer 200+ arbres avec le Foliage Tool
4. Ajouter des rochers et buissons via Quixel Bridge

**Livrable** : Un biome jouable de base, visuellement agréable

---

## Semaine 9 — Git, Sauvegarde et Organisation

**Objectifs** :
- Configurer Git LFS pour les projets UE5
- Maîtriser les commandes Git essentielles
- Organiser la structure de dossiers du projet
- Faire son premier push vers GitHub

**Exercice pratique** :
1. Configurer Git LFS sur le projet de la semaine 8
2. Créer un .gitignore pour UE5
3. Premier commit + push vers le repo GitHub RIFTBORN
4. Vérifier que les assets sont bien versionnés

**Livrable** : Projet envoyé sur GitHub avec historique de commits propre

---

## Semaine 10 — Début du Projet RIFTBORN

**Objectifs** :
- Créer le projet UE5 officiel RIFTBORN
- Mettre en place l'architecture Blueprint de base
- Implémenter les étapes 1 et 2 de `IMPLEMENTATION_ORDER.md`

**Tâches** :
1. Créer projet UE5 "Third Person" → renommer en `RIFTBORN`
2. Créer la structure de dossiers (`Assets/`, `Blueprints/`, `Maps/`, `UI/`, `Audio/`)
3. Lier le projet au repo GitHub
4. Créer `BP_RiftbornCharacter` héritant de ACharacter
5. Sculpter le terrain du biome Plaines Fertiles (2km × 2km)

**Livrable** : Premier commit du vrai projet RIFTBORN sur GitHub

---

## Blender — En Parallèle (semaines 1–10)

À pratiquer **30 min/jour** pendant les 10 semaines, en parallèle du curriculum UE5.

| Semaines | Contenu Blender |
|---|---|
| 1–2 | Interface, navigation, sélection, vues |
| 3–4 | Modélisation de base : cube, cylindre, sphère (Donut Tutorial de Blender Guru) |
| 5–6 | Modélisation d'une maison simple et d'un rocher |
| 7–8 | UV Unwrapping et application de textures basiques |
| 9–10 | Modélisation d'une créature simple (Glisseur des Plaines) |

**Objectif à 10 semaines** : Être capable de créer un asset 3D low-poly exportable en UE5.

---

## Récapitulatif des Livrables

| Semaine | Livrable |
|---|---|
| 1 | Scène UE5 avec objets colorés |
| 2 | 3 Blueprints interactifs |
| 3 | Barre de vie fonctionnelle |
| 4 | HUD complet temps réel |
| 5 | Ennemi IA patrouille/attaque |
| 6 | Système de pickup interactif |
| 7 | Mini-jeu "Survivor" complet |
| 8 | Biome visuellement agréable |
| 9 | Projet sur GitHub avec Git LFS |
| 10 | Premier commit RIFTBORN officiel |

---

> **Rappel** : Il est normal de bloquer, de recommencer, de passer 2h sur un bug simple. C'est le game dev. Chaque problème résolu est une compétence acquise pour toujours.

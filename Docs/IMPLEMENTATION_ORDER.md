# RIFTBORN MVP — Ordre d'Implémentation (14 Étapes)

## Principe

Chaque étape doit être **testable et fonctionnelle** avant de passer à la suivante.
Ne jamais sauter d'étape. Ne jamais commencer une étape si la précédente crashe.

**Règle d'or** : *Un système qui marche vaut mieux que deux systèmes à moitié faits.*

---

## Étape 1 — Projet UE5 + Configuration Git

**Durée estimée** : 1–2 jours

- [ ] Créer projet UE5 "Third Person" → nommer `RIFTBORN_MVP`
- [ ] Configurer la structure de dossiers : `Assets/`, `Blueprints/`, `Maps/`, `UI/`, `Audio/`
- [ ] Initialiser Git avec `.gitignore` UnrealEngine
- [ ] Configurer Git LFS pour les fichiers `.uasset`, `.umap`
- [ ] Premier commit sur GitHub repo `RIFTBORN`

**Critère de réussite** : Projet UE5 qui se lance, repo GitHub à jour.

---

## Étape 2 — Terrain du Biome (Plaines Fertiles)

**Durée estimée** : 1–2 semaines

- [ ] Créer `Map_FertilePlains` (2km × 2km)
- [ ] Sculpter terrain avec collines douces (hauteur max 50m)
- [ ] Appliquer matériaux : herbe, terre, roche
- [ ] Placer végétation via Foliage Tool (arbres, buissons)
- [ ] Importer assets Quixel Bridge (rochers, plantes)
- [ ] Configurer lumière directionnelle + ciel (Sky Atmosphere)
- [ ] Configurer cycle jour/nuit (20 min : 10 jour / 10 nuit)

**Critère de réussite** : Map belle, fluide à 60 FPS, jouable à pied.

---

## Étape 3 — SurvivalComponent

**Durée estimée** : 2–3 semaines

- [ ] Créer `BP_SurvivalComponent` (Actor Component)
- [ ] Variables : `Health`, `Hunger`, `Thirst`, `Stamina` (Float, 0–100)
- [ ] Timer de déclin : Hunger -0.05/sec, Thirst -0.08/sec
- [ ] Logique : si Hunger ≤ 20 → Health -0.02/sec
- [ ] Logique : si Thirst ≤ 10 → Health -0.05/sec
- [ ] Stamina : -10/sec au sprint, +5/sec au repos
- [ ] Attacher à `BP_RiftbornCharacter`

**Critère de réussite** : Toutes les jauges déclinent correctement sans erreur Blueprint.

---

## Étape 4 — Mort et Respawn

**Durée estimée** : 1 semaine

- [ ] Créer fonction `TriggerDeath()` dans `BP_RiftbornCharacter`
- [ ] Animation de mort (ragdoll ou animation montage)
- [ ] Écran noir (fade out)
- [ ] Sauvegarder position de mort (variable Vector)
- [ ] Respawn après 3 secondes à la base
- [ ] Reset Hunger/Thirst à 50 au respawn

**Critère de réussite** : Mourir et respawner sans crash, jauges remises à niveau.

---

## Étape 5 — BP_Resource_Base + Première Ressource

**Durée estimée** : 1–2 semaines

- [ ] Créer classe mère `BP_Resource_Base` (AActor)
- [ ] Variables : `ItemName`, `ItemIcon`, `Quantity`, `MaxStack`, `ResourceType`
- [ ] Event `OnInteract(Player)` — fonction de ramassage
- [ ] Créer `BP_Resource_Terrafera` héritant de `BP_Resource_Base`
- [ ] Modèle 3D de placeholder (cube gris acceptable au début)
- [ ] Placer 10 nœuds de Terrafera dans `Map_FertilePlains`

**Critère de réussite** : Ramasser du Terrafera, quantité affichée dans le log.

---

## Étape 6 — InventoryComponent (Grille)

**Durée estimée** : 2–4 semaines

- [ ] Créer struct `FInventorySlot` (ItemClass, Quantity, MaxStack, bIsEquipped, ItemName, ItemIcon)
- [ ] Créer `BP_InventoryComponent` (Array de 30 FInventorySlot)
- [ ] Fonction `AddItem(ItemClass, Quantity)` — avec stacking
- [ ] Fonction `RemoveItem(ItemClass, Quantity)` — pour la construction
- [ ] Fonction `HasItem(ItemClass, Quantity)` — vérification ressources
- [ ] Connecter `OnInteract` de BP_Resource_Base à `AddItem`
- [ ] Créer Widget inventaire (grille 6×5 en UMG)
- [ ] Afficher/masquer avec touche I

**Critère de réussite** : Ramasser items, les voir dans l'inventaire, stack fonctionnel.

---

## Étape 7 — BP_Structure_Base + 3 Structures

**Durée estimée** : 2–3 semaines

- [ ] Créer classe mère `BP_Structure_Base` (AActor)
- [ ] Variables : `HP`, `MaxHP`, `ConstructionCost`
- [ ] Créer `BP_Structure_Habitat` (coût : 20 Terrafera, 15 Etherwood, 5 Aetherium)
- [ ] Créer `BP_Structure_Wall` (coût : 10 Terrafera)
- [ ] Créer `BP_Structure_WatchTower` (coût : 15 Terrafera, 20 Etherwood, 3 Aetherium)
- [ ] Système de destruction (prise de dégâts, effondrement à HP=0)

**Critère de réussite** : 3 structures placées manuellement dans la map, destructibles.

---

## Étape 8 — BuildingComponent

**Durée estimée** : 2–4 semaines

- [ ] Créer `BP_BuildingComponent` (Actor Component)
- [ ] Ghost preview de la structure (matériau semi-transparent)
- [ ] Couleur verte = placement valide, rouge = invalide
- [ ] Validation : vérifier collisions + `HasItem()` dans l'inventaire
- [ ] Confirmation du placement : `RemoveItem()` + spawner la structure
- [ ] Mode construction activé avec touche B

**Critère de réussite** : Construire un Habitat avec les ressources requises, ressources déduites.

---

## Étape 9 — BP_Creature_Base + Glisseur des Plaines

**Durée estimée** : 2–3 semaines

- [ ] Configurer NavMesh sur `Map_FertilePlains`
- [ ] Créer `BP_Creature_Base` (ACharacter) avec HP, vitesse, dégâts d'attaque
- [ ] Créer `BT_Creature_Easy` (Behavior Tree) : Patrouille → Détection → Poursuite → Attaque
- [ ] Créer `BB_Creature` (Blackboard) : clés `TargetPlayer`, `PatrolPoint`, `bIsAlert`
- [ ] Créer `BP_Creature_Glisseur` héritant de `BP_Creature_Base`
- [ ] Stats : HP 80, Vitesse 300, Dégâts 10, Détection 15m
- [ ] Drop au mort : Terrafera ×2–5
- [ ] Spawner 15–20 Glisseurs dans la map

**Critère de réussite** : Glisseurs patrouillent, attaquent, meurent et droppent des ressources.

---

## Étape 10 — HUD Complet

**Durée estimée** : 1–2 semaines

- [ ] Créer `WBP_HUD` (Widget Blueprint)
- [ ] Barres de survie : Santé (rouge), Faim (orange), Soif (bleu), Endurance (jaune)
- [ ] Hotbar : 6 slots en bas au centre
- [ ] Minimap circulaire (haut-droite, rayon 50m)
- [ ] Système de notifications (haut-centre, fade 3s)
- [ ] Connecter toutes les barres aux variables de `SurvivalComponent`
- [ ] Tester sur résolution 1920×1080

**Critère de réussite** : HUD affiche toutes les données en temps réel, aucune valeur fixe.

---

## Étape 11 — Audio Ambiant + SFX

**Durée estimée** : 1–2 semaines (en parallèle)

- [ ] Importer ambiance sonore des Plaines (vent, oiseaux) — source : freesound.org
- [ ] Son de ramassage de ressource
- [ ] Son de construction de structure
- [ ] Son d'attaque et de mort du Glisseur
- [ ] Sons de pas du joueur (herbe, terre, roche)
- [ ] Configurer MetaSound pour les sons procéduraux

**Critère de réussite** : Gameplay sonore, aucune action silencieuse.

---

## Étape 12 — Ombrefang (Créature Medium)

**Durée estimée** : 1–2 semaines

- [ ] Créer `BT_Creature_Medium` avec logique de fuite à < 30% HP
- [ ] Créer `BP_Creature_Ombrefang`
- [ ] Stats : HP 200, Vitesse 400, Dégâts 25, Détection 20m
- [ ] Drop : Aetherium ×1–3, Aquaflora ×1–2
- [ ] Spawner 5–8 Ombrefangs dans zones éloignées

**Critère de réussite** : Ombrefangs plus dangereux, fuient quand blessés.

---

## Étape 13 — Gardien du Rift (Boss)

**Durée estimée** : 2–3 semaines

- [ ] Créer `BT_Creature_Boss` avec phases (normale et enragée)
- [ ] Créer `BP_Creature_GuardianRift`
- [ ] Stats : HP 1500, Phase 2 à 50% HP (vitesse +50%, dégâts +100%)
- [ ] Attaque mêlée (dégâts 60) + attaque à distance (projectile, dégâts 40, cd 3s)
- [ ] Drops : Luminite ×3–5, Aetherium ×10+
- [ ] Emplacement fixe : centre-nord de la map
- [ ] Musique de boss (track dédiée)

**Critère de réussite** : Boss difficile, deux phases, drops de valeur.

---

## Étape 14 — Map de Test Assemblée

**Durée estimée** : 1 semaine

- [ ] Assembler tous les systèmes dans `Map_FertilePlains`
- [ ] Vérifier l'absence de bugs critiques (crash, boucle infinie, NullReference)
- [ ] Tester une session complète de 30 minutes
- [ ] Documenter les bugs dans Linear (créer issues)
- [ ] Corriger les bugs critiques avant de continuer
- [ ] Prendre des captures d'écran pour les devlogs

**Critère de réussite** : Jouer 30 minutes sans crash. Toutes les jauges fonctionnent. Ressources ramassables. Constructions possibles. Créatures actives.

---

## Récapitulatif des Durées

| Étape | Description | Durée estimée |
|---|---|---|
| 1 | Projet + Git | 1–2 jours |
| 2 | Terrain biome | 1–2 semaines |
| 3 | SurvivalComponent | 2–3 semaines |
| 4 | Mort & Respawn | 1 semaine |
| 5 | Ressource de base | 1–2 semaines |
| 6 | Inventaire | 2–4 semaines |
| 7 | Structures de base | 2–3 semaines |
| 8 | BuildingComponent | 2–4 semaines |
| 9 | Première créature IA | 2–3 semaines |
| 10 | HUD complet | 1–2 semaines |
| 11 | Audio | 1–2 semaines |
| 12 | Créature medium | 1–2 semaines |
| 13 | Boss | 2–3 semaines |
| 14 | Map de test | 1 semaine |
| **TOTAL** | **MVP Jouable** | **~6 à 12 mois** |

---

> *Ce document est la feuille de route technique. Chaque ticket Linear de la Phase 2 correspond à une de ces 14 étapes.*

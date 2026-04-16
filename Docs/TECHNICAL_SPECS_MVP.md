# RIFTBORN: Chronicles of Terra — Spécifications Techniques MVP

## Architecture Blueprint UE5

### Personnage Principal : `BP_RiftbornCharacter`

Hérite de `ACharacter`. Composants attachés :

- **`SurvivalComponent`** — gestion santé/faim/soif/endurance
- **`InventoryComponent`** — grille d'items, stacking, sérialisation
- **`BuildingComponent`** — placement de structures, validation ressources

### Classes de Base

| Blueprint | Hérite de | Rôle |
|---|---|---|
| `BP_Resource_Base` | `AActor` | Classe mère de toutes les ressources ramassables |
| `BP_Creature_Base` | `ACharacter` | Classe mère de toutes les créatures |
| `BP_Structure_Base` | `AActor` | Classe mère de toutes les constructions |
| `BP_RiftbornCharacter` | `ACharacter` | Personnage jouable |
| `BP_GameMode_Riftborn` | `AGameMode` | Règles du jeu, spawn, win/lose |

---

## Système de Survie — Variables et Taux

| Variable | Valeur Initiale | Taux de déclin (/sec) | Seuil critique |
|---|---|---|---|
| `Health` | 100.0 | 0 (dégâts uniquement) | ≤ 20 → alerte rouge |
| `Hunger` | 100.0 | 0.05 | ≤ 20 → perte santé |
| `Thirst` | 100.0 | 0.08 | ≤ 10 → perte santé rapide |
| `Stamina` | 100.0 | 10.0 (sprint) / +5.0 (repos) | ≤ 0 → sprint impossible |

### Logique de Santé

```
Si Hunger <= 20 → Health -= 0.02/sec
Si Thirst <= 10 → Health -= 0.05/sec
Si Health <= 0  → TriggerDeath()
```

### Mort et Respawn

1. `TriggerDeath()` → jouer animation mort + écran noir
2. Sauvegarder position de mort (marqueur sur la carte)
3. Respawn à la base ou au dernier campement sauvegardé
4. Conserver 50% de l'inventaire (items communs perdus, rares conservés)
5. Hunger/Thirst reset à 50.0

---

## Ressources MVP — Spécifications de Spawn

| Ressource | Rareté | Biome | Quantité par nœud | Cooldown respawn |
|---|---|---|---|---|
| Aetherium Crystal | ★★★★ | Tous biomes | 3–8 unités | 10 min |
| Terrafera Stone | ★★ | Plaines, Déserts | 10–20 unités | 5 min |
| Etherwood Log | ★★ | Forêts, Plaines | 5–15 unités | 8 min |
| Aquaflora Herb | ★★★ | Zones humides, Grottes | 2–6 unités | 12 min |
| Luminite Shard | ★★★★★ | Grottes profondes uniquement | 1–3 unités | 30 min |

---

## Inventaire — Struct de Données

```
Struct FInventorySlot {
    TSubclassOf<BP_Resource_Base>  ItemClass
    int32                          Quantity
    int32                          MaxStack     // défaut: 50
    bool                           bIsEquipped
    FText                          ItemName
    UTexture2D*                    ItemIcon
}
```

Grille : **6 colonnes × 5 lignes = 30 slots**

---

## Système de Construction — Coûts MVP

| Structure | Terrafera | Etherwood | Aetherium | HP |
|---|---|---|---|---|
| Habitat Modulaire | 20 | 15 | 5 | 500 |
| Mur Défensif | 10 | 0 | 0 | 300 |
| Tour de Guet | 15 | 20 | 3 | 400 |
| Centrale Aetherium | 30 | 10 | 20 | 600 |

**Placement** : Grille snapping 1m × 1m. Couleur verte = valide, rouge = invalide.

---

## IA des Créatures MVP — Behavior Trees

### `BT_Creature_Easy` (Glisseur des Plaines)
**Stats** : HP 80 | Vitesse 300 | Détection 15m | Drop : Terrafera ×2–5

### `BT_Creature_Medium` (Ombrefang)
**Stats** : HP 200 | Vitesse 400 | Détection 20m | Drop : Aetherium ×1–3, Aquaflora ×1–2

### `BT_Creature_Boss` (Gardien du Rift)
**Stats** : HP 1500 | Phase 2 à 50% HP (vitesse +50%, dégâts +100%) | Drop : Luminite ×3–5, Aetherium ×10+

---

## Interface HUD

| Élément | Position | Description |
|---|---|---|
| Jauge Santé | Bas-gauche | Barre rouge, icône cœur |
| Jauge Faim | Bas-gauche | Barre orange, icône fourchette |
| Jauge Soif | Bas-gauche | Barre bleue, icône goutte |
| Jauge Endurance | Bas-centre | Barre jaune, visible seulement au sprint |
| Inventaire rapide (hotbar) | Bas-centre | 6 slots horizontaux |
| Minimap | Haut-droit | Rayon 50m, nord en haut |
| Notifications | Haut-centre | Fade in/out, durée 3s |

---

## Paramètres de Génération du Biome Initial

**Biome** : Plaines Fertiles (`Biome_FertilePlains`)

| Paramètre | Valeur |
|---|---|
| Taille de la map | 2km × 2km |
| Hauteur max terrain | 50m |
| Cycle jour/nuit | 20 min (10 jour / 10 nuit) |
| Spawns créatures (facile) | 15–20 actifs simultanément |
| Spawns créatures (medium) | 5–8 actifs simultanément |
| Boss de zone | 1 emplacement fixe (centre-nord) |

---

## Ordre d'Implémentation

Voir `IMPLEMENTATION_ORDER.md` pour les 14 étapes détaillées.

```
1. Projet UE5 + Git
2. Terrain du biome
3. SurvivalComponent
4. Mort & Respawn
5. BP_Resource_Base + 1 ressource
6. InventoryComponent (grille)
7. BP_Structure_Base + 3 structures
8. BuildingComponent
9. BP_Creature_Base + Glisseur (easy)
10. HUD complet
11. Audio ambiant + SFX
12. Ombrefang (medium)
13. Gardien du Rift (boss)
14. Première Map de test assemblée
```

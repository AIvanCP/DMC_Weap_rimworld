# RimWorld: Devil May Cry Weapons

[![RimWorld](https://img.shields.io/badge/RimWorld-1.5%20%7C%201.6-brightgreen.svg)](https://rimworldgame.com/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

Twenty-one devil arms from Devil May Cry 3, 4 and 5. No assembly, no Harmony — every
effect described below is stock RimWorld XML, verified against `Assembly-CSharp.dll`.

## The arsenal

### Devil swords

| Weapon | Game | Tier | What it does |
|---|---|---|---|
| **Rebellion** | 3 / 4 / 5 | 1 | Plain, reliable broadsword. Its Stinger thrust hits far harder against a target that has not noticed you |
| **Force Edge** | 3 | 2 | The Sparda sealed. A little holy light leaks out when it connects |
| **Sparda** | 3SE / 5 | 3 | Unsealed. Holy light on the blade, demonic fire on the scythe sweep, the best armour penetration of any sword here |
| **Yamato** | 3 / 5 | 2 | Dimensional cut: bypasses armour outright and opens a wound that will not clot. Judgement Cut on an unaware target |
| **Mirage Edge** | 5 | 2 | Fast, light, high dodge, +move speed. Built around the surprise attack |
| **Devil Sword Dante** | 5 | 3 | Heavy fire procs on both the edge and the thrust |
| **Devil Sword Vergil** | 5 | 3 | Yamato + Beowulf + Mirage Edge. Dimensional cut on two tools, holy light on the pommel |
| **Red Queen** | 4 / 5 | 2 | Exceed. The highest fire proc chance in the mod (60% on the exceed slash) |
| **Agni & Rudra** | 3 / 5 | 2 | Two blades, two elements — Agni burns, Rudra sweeps wide and catches extra targets |
| **Nevan** | 3 | 2 | Lightning on the scythe and the distortion; the bat swarm drains the target instead of wounding it |

### Gauntlets

| Weapon | Game | Tier | What it does |
|---|---|---|---|
| **Beowulf** | 3 / 5 | 2 | Holy light on every tool. Blinds — the target cannot aim |
| **Balrog** | 5 | 3 | Fast jabs, heavy kicks, and an Ignition tool with a 60% fire proc |
| **Gilgamesh** | 4 | 3 | No element. Highest blunt armour penetration in the mod, plus concussive stagger |
| **King Cerberus** | 5 | 2 | Three heads, three elements — fire, ice, lightning — plus a plain chain whip |

### Heavy

| Weapon | Game | Tier | What it does |
|---|---|---|---|
| **Cavaliere** | 5 | 3 | 12 kg and −0.45 move speed. Cleaves several targets and leaves them bleeding badly |

### Firearms

| Weapon | Game | Tier | What it does |
|---|---|---|---|
| **Ebony & Ivory** | 3 / 4 / 5 | 2 | 6-round burst, fastest cooldown here. Sustained fire occasionally staggers |
| **Blue Rose** | 4 / 5 | 2 | Both barrels at once — a small charged detonation that staggers the target |
| **Coyote-A** | 4 / 5 | 1 | Sawed-off. 16 tiles of range, enormous stopping power, knocks targets down |
| **Kalina Ann** | 3 / 5 | 2 | Missile launcher, 2.6 blast radius, with a grappling bayonet for melee |
| **Artemis** | 3 | 2 | Five-bolt multi-lock burst; each bolt can sear the target's sight |
| **Spiral** | 3 | 2 | 45-tile rifle whose round rebounds for a second hit 45% of the time |

## How the effects work

There is no C# here. Each elemental hit is a real second damage instance attached to a
tool through `<extraMeleeDamages>` (or to a projectile through
`<projectile><extraDamages>`), each with its own `<chance>`. The damage type then applies
its debuff through `<additionalHediffs>` on the `DamageDef`.

| Damage type | Debuff | Effect |
|---|---|---|
| `DMC_DemonicFire` | `DMC_DemonicBurn` | Pain, rest drain, movement loss at higher severity. Tendable. Can ignite the ground |
| `DMC_DemonicIce` | `DMC_DemonicFrost` | Movement and manipulation loss |
| `DMC_DemonicLightning` | `DMC_ElectricShock` | Real stun via `causeStun` (45 ticks, with `stunAdaptationTicks`), then lingering accuracy loss |
| `DMC_HolyLight` | `DMC_LightBlind` | Sight capacity and shooting accuracy loss |
| `DMC_DimensionalCut` | `DMC_DimensionalWound` | Armour penetration 2.0 (bypasses armour), raised bleed rate, reduced natural healing |
| `DMC_DemonicWind` | — | Widened `cutExtraTargetsCurve` + `cutCleaveBonus` 2.0 |
| `DMC_SoulDrain` | `DMC_Enervated` | Consciousness, manipulation and melee DPS loss; heavy rest drain |
| `DMC_Shred` | `DMC_Shredded` | Heavy bleeding, tendable |
| `DMC_Stagger` | — | Pure stun, no wound (inherits vanilla `Stun`) |
| `DMC_ChargedRound` | — | Explosive, staggers on hit |

## Acquisition

| Route | Detail |
|---|---|
| Research | `demonic archaeology` (4,000) → `legendary weapon forging` (6,000) → `ultimate demon armaments` (8,000) |
| Crafting | Melee at an electric smithy, firearms at a machining table. Every recipe names its research tier explicitly |
| Orbital trader | `legendary weapon collector`, commonality 0.15 among orbital traders. Buys DMC weapons as well as selling them |
| Caravan | `legendary artifact caravan`, registered with both outlander factions |
| Quests | Weapons carry `RewardStandardLowFreq` |

No raider, mercenary, tribal, Empire or Traders Guild pawn will spawn carrying one.

## Compatibility

- RimWorld 1.5 / 1.6, verified against 1.6
- Royalty is optional — it is only used for equip/hit sound flavour, guarded with `MayRequire`
- No other DLC required, no dependency on other mods
- Safe to add to an existing save

## Fixed in the expansion pass

These were live bugs in the previous version, verified against vanilla data before changing:

1. **The weapons were not player-exclusive.** The four melee weapons carried
   `MedievalMeleeDecent` / `MedievalMeleeAdvanced`, and both guns carried
   `SimpleGun` / `Pistol` / `Revolver` / `Autopistol` — the exact tags mercenary, tribal,
   Empire and Traders Guild pawnkinds roll their weapons from. Raiders could spawn holding
   Yamato. All weapons now carry only `DMCWeapon`.

2. **Every patch file was inert.** `NPCEquipment_Removal.xml` and the three `.disabled`
   files used `<Defs>` as their root element; RimWorld only runs patches from a `<Patch>`
   root, so all four were loaded as def files and did nothing. The one remaining patch is
   now correct.

3. **The research tree unlocked nothing.** `recipeMaker` is inherited from
   `BaseMeleeWeapon`, which has no `researchPrerequisite` — Yamato was craftable at a
   fueled smithy on day one for 200 steel, despite 18,000 points of research existing.

4. **The elemental system was never wired up.** `DMC_DemonicFire`, `DMC_DemonicIce`,
   `DMC_DemonicLightning` and six hediffs were defined and referenced by nothing. King
   Cerberus's "fire head", "ice head" and "lightning head" all dealt identical plain
   `Blunt` damage.

5. **Blue Rose never exploded.** `Bullet_BlueRoseExplosive` set `explosionRadius` while
   inheriting `BaseBullet`, whose `thingClass` is `Bullet` — and `Bullet.Impact` does not
   read `explosionRadius`. Every vanilla explosive projectile overrides `thingClass` to
   `Projectile_Explosive`; this one now does too.

6. **Four weapons referenced sound defs that do not exist.** `Interact_MonolithActivate`
   (Yamato, Devil Sword Dante, Mirage Edge) and `Interact_MeleeWeapon` (King Cerberus) are
   not SoundDefs in any DLC, so they threw a cross-reference error on every load.

7. **The caravan trader could not spawn.** A non-orbital `TraderKindDef` only appears if a
   `FactionDef` lists it in `caravanTraderKinds`; nothing did.

8. **Stat offsets were off by roughly a factor of twenty.** `MeleeHitChance`,
   `MeleeDodgeChance` and `ShootingAccuracyPawn` are measured in skill-like points, not
   percentages — the vanilla brawler trait grants `MeleeHitChance` +4. The old defs granted
   +0.25 and +0.15, which is functionally nothing.

## License

MIT. Devil May Cry is a trademark of Capcom; this is an unofficial fan mod.

---

*"Power is everything. Without power, you cannot protect anything."* — Vergil

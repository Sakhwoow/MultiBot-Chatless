# Multibot Chatless + Bridge — Roadmap de reprise

Statut : roadmap active issue de l'audit initial v1c du 1er août 2026, resynchronisée avec l'état post-merge du 14 août 2026.
Dernière mise à jour : 14/08/2026 — service d'enchantement d'objet `ENCHANT_TRADE_V1` implémenté et validé en jeu, UI 440 px/i18n validées ; prochain chantier normal fixé à l'ajout/retrait d'items précis dans les règles de loot.
Cette roadmap est la source de vérité active du projet. Les anciens trackers et le fichier `TODO.md` ont été consolidés ici.

## Baseline auditée

Audit de synchronisation : `audit-multibot-roadmap-next-item-v1-2026-08-14-143927`.

- Addon : `L:\ChromieCraft_3.3.5a\Interface\AddOns\MultiBot`
  - branche `main` ;
  - HEAD et `origin/main` : `106074c3c93f80812f73af27e746860c7c8a4dcf` ;
  - merge PR #61 : **Add chatless group Roll UI** ;
  - worktree propre au début et à la fin de l'audit.
- Bridge : `L:\AC_PB\azerothcore-wotlk\modules\mod-multibot-bridge`
  - branche `main` ;
  - HEAD et `origin/main` : `210bd1f4f6597fe4f0691ec729ec4904ebe2d463` ;
  - merge PR #26 : **Add chatless group Roll support** ;
  - worktree propre au début et à la fin de l'audit.
- Playerbots : `L:\AC_PB\azerothcore-wotlk\modules\mod-playerbots`
  - branche `master`, commit `a7b885d27134466dbc1c91d39b8241ea725a1bbb` ;
  - **lecture seule stricte** ; invariant avant/après audit : `OK`.
- AzerothCore : branche `Playerbot`, commit `092e9ba6ff8dc6d861dddd1f31baa9d404381a85`, worktree propre pendant l'audit.
- Communication actuelle : bridge-first pour les principaux rafraîchissements UI et pour plusieurs actions d'écriture explicitement bornées ; des occurrences `SendChatMessage` subsistent et doivent être classées/migrées famille par famille.
- Fallback automatique legacy désactivé par défaut : `MultiBot.allowLegacyChatFallback = false`. Certains chemins de compatibilité historiques restent toutefois explicitement documentés jusqu'à leur migration ou leur suppression validée.

## Règles de progression

Audit → Analyse → Proposition → Validation utilisateur → Patch minimal → Vérifications → Compilation → Tests en jeu → Audit final → Archivage

- Aucun patch à l'aveugle.
- Aucun changement dans `mod-playerbots`.
- Un patch = un objectif.
- Rollback et hashes obligatoires.
- Ne jamais ajouter d'exécuteur bridge générique acceptant une commande Playerbots arbitraire.

## Contribution externe Jellypowered — RÉFÉRENCE CONSERVÉE, ATTRIBUTION OBLIGATOIRE

Source reçue le 04/08/2026 :

- auteur : Jellypowered `<Jellypowered@gmail.com>` ;
- commit 1 : `13059a9f334d1e5aaa8560ab29a1814e48b07054` ;
- commit 2 : `7ff1347535be6d5a3256d933731c11c4b3f3b38e` ;
- commit 3 : `04061f084bd189487f1ac0e99892316146f1bea0` ;
- la PR est conservée comme source de recherche et ne doit pas être fusionnée directement dans `main`.

Décision validée :

- auditer la contribution dans un environnement isolé ;
- conserver les parties techniquement sûres et utiles ;
- adapter ou réécrire les parties incompatibles avec notre bridge actuel ;
- intégrer progressivement par patches à objectif unique ;
- ne jamais modifier `mod-playerbots` ;
- ne marquer aucune fonction comme intégrée avant vérification, compilation, tests en jeu et validation explicite de l'utilisateur.

Fonctions candidates identifiées dans la contribution lors de l'audit initial ; leur statut projet actuel doit être lu dans les phases et jalons ci-dessous, pas déduit de cette liste :

1. helpers de parsing numérique strict et réponses structurées ;
2. inventaire détaillé `INV_BAG`, `INV_ITEM_LOC`, `INV_EQUIP_LOC` ;
3. lectures bulk inventaire et compétences ;
4. équipement d'objet ;
5. abandon et partage de quête ;
6. lancement de sorts ;
7. application de talents ;
8. échange d'objets ;
9. artisanat ciblé ;
10. modifications des transferts banque, banque de guilde et vendeur.

Crédits obligatoires :

- les audits et rapports conservent les trois hashes de commits, le nom et l'adresse de l'auteur ;
- chaque PR intermédiaire indique précisément le code repris, adapté, réécrit ou rejeté ;
- une reprise substantielle de code utilise, lorsque pertinent :
  `Co-authored-by: Jellypowered <Jellypowered@gmail.com>` ;
- une réécriture seulement inspirée mentionne :
  `Design inspired by the Jellypowered bridge contribution.` ;
- les crédits sont préparés pour la PR uniquement après validation des tests en jeu de la partie concernée ;
- aucune attribution ne doit suggérer qu'une fonction non testée ou non intégrée est déjà livrée.

Politique de tests :

- les tests ciblés restent obligatoires après chaque patch ;
- les tests exhaustifs transversaux de toutes les fonctions pourront être exécutés vers la fin du projet ;
- ce report des tests exhaustifs ne permet pas de déclarer une fonction validée avant ses propres tests ciblés.

Statut de reprise : contribution conservée comme source de référence. Les fonctions du projet déjà mergées sont suivies par leurs audits, patches et PR propres ; ce document ne doit pas déduire leur provenance sans preuve. Toute reprise future issue de cette contribution doit conserver l'attribution prévue ci-dessus et rester soumise au protocole Audit → Validation → Patch → Tests.

## Phase 0 — Assainissement documentaire — TERMINÉE

Objectif : repartir avec une documentation courte, actuelle et non contradictoire.

Réalisé par `patch-multibot-docs-cleanup-roadmap-v1-2026-08-01-162227` :

- les 18 anciens trackers ont été sauvegardés puis retirés du dossier actif ;
- `TODO.md` a été consolidé dans cette roadmap puis retiré ;
- `docs/ROADMAP.md` est la source de vérité active ;
- `docs/DEBUG_RUNBOOK.md` consolide le guide de debug et d'observabilité ;
- le README référence les deux documents actifs ;
- le package contient un rollback intégral et vérifiable.

Critère de sortie : phase validée par `verify.ps1`, avec hashes post-patch conformes et aucun ancien document actif.

## Validation livrée — Formations chatless — APPLICATION ET CONSULTATION VALIDÉES LE 03/08/2026

Patches fonctionnels validés :

- application : `patch-multibot-formation-chatless-v1c-2026-08-01-181300` ;
- consultation : `patch-multibot-formation-query-chatless-v1-2026-08-03-195340` ;
- localisation : `patch-multibot-formation-query-i18n-v1b-2026-08-03-210300`.

Périmètre validé :

- les clics gauche `arrow`, `queue`, `near`, `melee`, `line`, `circle`, `chaos` et `shield` utilisent désormais `RUN~FORMATION~GROUP` ;
- le bridge applique directement `FormationValue::Load()` sans passer par `HandleCommand()` ni par l'action Playerbots `set formation` ;
- le fonctionnement est validé avec la stratégie `passive`, en groupe et pour l'ensemble des bots contrôlables d'un raid ;
- aucun fichier de `mod-playerbots` n'a été modifié ;
- l'icône de l'addon n'est mise à jour qu'après un `FORMATION_ACK` complet ;
- aucun message `formation ...` n'est envoyé dans PARTY ou RAID pour ces clics ;
- le clic droit utilise `MultiBot.Comm.RequestFormations()` puis `GET~FORMATIONS~GROUP` ;
- le bridge lit la valeur effective de chaque bot avec `FormationValue::Save()` et renvoie `FORMATIONS_BEGIN/ITEM/END` ;
- le résultat est affiché localement, une ligne par bot, dans un tooltip traduit pour les huit locales supportées ;
- aucun message PARTY, RAID, WHISPER ou `TellMaster` n'est produit par la consultation.

Preuves de validation :

- compilation `worldserver` validée par l'utilisateur ;
- audit runtime : `audit-multibot-runtime-tests-v1c-2026-08-03-184046.zip` ;
- SHA-256 de l'audit : `7E3FBD948C51FAE34351B97B416BDFA663061F4577932F6AC251C79ECE933F25` ;
- 11 requêtes `RUN~FORMATION`, 11 réponses `FORMATION_ACK`, 55 applications réussies et 0 échec ;
- les huit formations ont provoqué visuellement le déplacement attendu des bots ;
- l'icône a été mise à jour visuellement sans message chat visible ;
- aucune erreur Lua MultiBot ni ancien blocage `PassiveMultiplier` observé ;
- audit de consultation : `audit-multibot-runtime-tests-v1c-2026-08-03-203219.zip` ;
- SHA-256 de cet audit : `44627A920618C747BD9EEB0384D118FFFA13157828677172E46A642436677CB5` ;
- 9 requêtes `GET~FORMATIONS`, 9 séquences `FORMATIONS_BEGIN/END` et 23 réponses individuelles `FORMATIONS_ITEM` ;
- tooltip local et traductions validés visuellement par l'utilisateur, sans sortie chat.

Reste explicitement hors périmètre :

- la formation Playerbots `far` existe dans le module de référence mais n'est pas exposée par l'interface actuelle.

## Validation intermédiaire — STATE framing + Strategy Mutation — STATIQUE VALIDÉE LE 07/08/2026, RUNTIME FINAL À TERMINER

Baseline intégrée :

- PR #49 : synchronisation bridge, contrôles stratégies, favoris persistants et stabilisation STATE ;
- PR #50 : diagnostics explicites des rejets `RunStrategyCommand` ;
- PR #51 : déduplication mécanique des helpers partagés de workflow roster ;
- addon `main` : `270911305acf3e806d389712a34a9433131db981`.

État STATE validé statiquement :

- capacité `STATE_FRAMING_V1` présente côté addon et bridge ;
- requêtes unitaires `GET~STATE` et globales `GET~STATES` gérées par transactions tokenisées ;
- framing `STATE_BEGIN/STATE_ITEM/STATE_END` et framing global `STATES_BEGIN/.../STATES_END` ;
- `STATE_ABORT` pris en compte comme échec de la requête concernée ;
- timeout par bot à 5 s et timeout global à 15 s ;
- limite de 32 requêtes STATE actives, 128 bots, 256 stratégies par scope, 192 caractères par stratégie et 32768 octets cumulés ;
- nettoyage des transactions sur timeout/erreur/déconnexion ;
- garde d'ordre par bot pour empêcher une réponse ancienne de remplacer un état plus récent.

État mutations stratégies validé statiquement :

- capacité `STRATEGY_MUTATION_V1` présente côté addon et bridge ;
- mutations `co/nc` structurées via `RUN~STRATEGY` pour les chemins utilisant `MultiBot.Comm.RunStrategyCommand()` ;
- résultat serveur via `STRATEGY_ACK` avec compteurs `matched/succeeded/failed` ;
- limites de taille, nombre d'opérations et requêtes actives ;
- timeout à 5 s ;
- neuf diagnostics de rejet explicites ajoutés par F07 ;
- `RunStrategyCommand()` ne contient aucun `SendChatMessage`.

Preuve d'audit final statique :

- archive : `audit-multibot-state-strategy-final-v1-2026-08-07-224000-2026-08-07-224709.zip` ;
- SHA-256 : `B00DBE597F554F9E20F2ABEFDC22097BC2A06DCDD3F07FD9F6522F98A7DF38DA` ;
- 57 contrôles, 0 échec ;
- addon, bridge et `mod-playerbots` ont des empreintes avant/après identiques pendant l'audit ;
- `mod-playerbots` reste strictement en lecture seule.

Reste à terminer avant de fermer définitivement ce bloc :

- le lot Warlock Stones/Soulstones/Pets/Curses est validé pour sa migration chatless et ne fait plus partie des reliquats `co/nc` prioritaires ;
- le comportement sans fallback silencieux des mutations stratégies a été durci et mergé après cette baseline intermédiaire ;
- exécuter/consolider la matrice runtime finale : zéro/un/plusieurs bots, listes longues, fragment manquant/dupliqué/désordonné, réponse tardive, timeouts, déconnexion en cours de transaction, mutations valides/invalides, bot absent, plusieurs bots, smoke test toutes classes, zéro erreur Lua, contrôle chat et logs ;
- classifier puis migrer les autres familles legacy réellement automatiques avant de déclarer le projet entièrement chatless.

## Validation livrée — Sélecteurs Warlock chatless + Stones — MIGRATION CHATLESS VALIDÉE, RELIQUATS SUSPENDUS

Périmètre validé et mergé :

- les sélecteurs Warlock Stones, Soulstones, Pets et Curses utilisent le transport structuré `STRATEGY_MUTATION_V1` / `RUN~STRATEGY` lorsque le bridge est disponible ;
- le chemin bridge attend l'état serveur autoritatif au lieu de valider localement une mutation avant l'ACK ;
- les contrôles Warlock invalides `dps` et `dps debuff` ainsi que le placeholder Buff désactivé ont été retirés et le layout a été compacté ;
- le bridge contient le mécanisme de bascule Firestone/Spellstone et l'endpoint diagnostique à la demande `GET~WEAPON_ENCHANT` / `WEAPON_ENCHANT` ;
- aucun fichier de `mod-playerbots` n'est modifié.

Décision de roadmap au 14/08/2026 :

- la **vérification réelle finale du `TEMP_ENCHANTMENT_SLOT` Firestone/Spellstone** reste un chantier suspendu à reprendre seulement à la fin de la roadmap normale ;
- les **quatre warnings LuaLint restants dans `Strategies/MultiBotWarlock.lua`** restent également suspendus ;
- ces reliquats ne doivent pas interrompre le chantier suivant de la Phase 5.

## Synchronisation post-merge — État livré au 14/08/2026

Les jalons suivants, postérieurs à la mise à jour du 08/08, sont présents dans les branches `main` auditées :

- mutations stratégies : suppression du fallback chat silencieux lorsque le chemin structuré est requis ;
- Outfits : transport bridge-first et négociation `OUTFIT_V1` ;
- inventaire : lecture/rafraîchissement natifs via `INVENTORY_V1` ;
- banque, banque de guilde et achat vendeur : durcissements serveur des actions `ITEM_ACTION` ;
- vente inventaire `SELL_VENDOR` : bridge-first lorsque `INVENTORY_BULK_SELL_V1` est négocié ; le fallback legacy de compatibilité demeure hors chemin normal ;
- `OPEN_ITEMS` : bridge-first via `INVENTORY_OPEN_V1`, avec traitement résiduel borné côté serveur ;
- `GROUP ROLL` : bridge-first via `GROUP_ROLL_V1`, avec mode normal et mode item, filtrage aux bots visibles/contrôlables du groupe, rate-limit serveur et ACK structuré.

Jalons de merge principaux :

- Addon PR #58 — **Migrate inventory Sell Vendor to the bridge** ;
- Bridge PR #24 — **Add safe bridge-first SELL_VENDOR inventory action** ;
- Addon PR #60 — **Add bridge-first OPEN_ITEMS inventory action** ;
- Bridge PR #25 — **Add residual auto-safe OPEN_ITEMS handling** ;
- Addon PR #61 — **Add chatless group Roll UI** — merge `106074c3c93f80812f73af27e746860c7c8a4dcf` ;
- Bridge PR #26 — **Add chatless group Roll support** — merge `210bd1f4f6597fe4f0691ec729ec4904ebe2d463`.

Validation `GROUP ROLL` :

- roll normal 0–100 : OK ;
- roll avec objet par Shift+clic : OK ;
- seuls les bots éligibles au contexte Playerbots invoqué participent au roll item ;
- aucun whisper/chat parasite sur le workflow ;
- protection contre double envoi et refus d'un item vide/invalide ;
- pending nettoyé sur déconnexion/changement de monde ;
- UI finale validée : `240x245`, fond opaque style inventaire, padding horizontal `10 px`, padding vertical haut `10 px` ;
- compilation Bridge déjà validée sans erreur.

## Phase 1 — Baseline de compilation et tests de non-régression

Objectif : prouver le fonctionnement de l'état actuel avant toute correction source.

### Serveur / bridge

- Compiler l'état Git audité sans modification.
- Vérifier le chargement de `mod-multibot-bridge` et de sa configuration.
- Vérifier `HELLO/HELLO_ACK`, `PING/PONG`, erreurs et logs.

### Addon / jeu

Tester avec zéro, un et plusieurs bots :

- chargement initial, `/reload`, déconnexion/reconnexion ;
- bots personnels, AddClass bots et randombots groupés ;
- roster, states, details, stats et PVP stats ;
- inventaire, banque bot, banque de guilde, vendeur ;
- spellbook, talents, glyphes et outfits ;
- quêtes, objets de jeu, character info, réputations, monnaies ;
- recettes, craft et trainer ;
- RTI, Pull Control, Combat Strategies, Disperse et Loot Rules.

Mesurer le chat visible avec `MultiBot.allowLegacyChatFallback = false`.

Critère de sortie : matrice de tests remplie, baseline compilée, bugs reproductibles séparés des impressions anciennes.

## Phase 2 — Durcissement du bridge

Objectif : traiter les risques de sécurité et de blocage avant de développer de nouvelles fonctions.

- Ajouter des parseurs numériques stricts : chaîne entière valide, absence de signe négatif, contrôle d'overflow et bornes métier.
- Borner la taille totale des messages et la longueur de chaque champ.
- Borner les quantités d'actions item, notamment l'achat vendeur.
- Ajouter un rate limiting par joueur et par famille de requêtes `HELLO`, `PING`, `GET` et `RUN`.
- Revalider joueur, session, carte, propriété du bot, proximité PNJ et état du bot au moment de l'exécution.
- Borner les logs et désactiver les logs console par défaut même si la configuration est absente.
- Retourner des erreurs structurées et distinctes pour message malformé, limite dépassée, permission refusée et état incompatible.

Critère de sortie : patch compilé, tests de messages malformés/volumineux, aucune boucle longue pilotable par le client.

## Phase 3 — Stabilisation du protocole addon/bridge

Objectif : fiabiliser les transactions et supprimer les ambiguïtés d'ACK.

- Documenter chaque commande `GET` et `RUN`, ses champs, bornes, réponses et erreurs.
- Identifier les payloads non bornés ; fragmenter notamment le roster si les limites client l'exigent.
- Uniformiser les séquences `BEGIN/ITEM/END` et les tokens de transaction.
- Ajouter timeout, annulation, déduplication et gestion des réponses hors ordre côté addon.
- Distinguer : requête reçue, commande transmise à Playerbots et résultat effectivement vérifié.
- Vérifier perte, duplication, réponses tardives, changement de carte et déconnexion du bot.

Critère de sortie : protocole documenté, transactions déterministes, aucune frame bloquée sur une requête perdue.

## Phase 4 — Corrections runtime prioritaires

Traiter un problème par patch, dans cet ordre :

1. Reconnexion : bots inconnus dans l'UI de groupe Blizzard jusqu'à `/reload`.
2. AddClass bots créés ou sélectionnés au niveau 1 pour un joueur niveau 80.
3. Vérification fonctionnelle de Disperse.
4. Lenteur de l'affichage des glyphes et recentrage des icônes.
5. ID de quête affiché temporairement à la place du titre ; étudier l'avancement de quête par bot.
6. Inventaire au-delà de 16 emplacements.
7. Outfit avec deux armes à deux mains.
8. Compatibilité de l'UI talents/glyphes avec la configuration actuelle.
9. Quick Hunter/Shaman : croix stable et absence de quick bars pour un joueur humain.
10. Raidus : rafraîchissement ouverture/fermeture et purge des bots inconnus/SavedVariables.
11. Respect global du frame strata configuré.
12. Harmonisation de la frame PVP Stats.

Critère de sortie : chaque correction possède reproduction avant, test après et non-régression ciblée.

## Phase 5 — Migration des commandes chat restantes

Objectif : réduire le chat par familles fonctionnelles, sans toucher à Playerbots.

Avant chaque migration, classer l'occurrence `SendChatMessage` comme :

- commande manuelle volontaire ;
- fallback diagnostic ;
- message d'information ;
- mécanisme UI à migrer ;
- code mort à supprimer.

Ordre recommandé et état réel :

1. **Formations — application par clic gauche : TERMINÉ / VALIDÉ** via `RUN~FORMATION~GROUP`.
2. **Consultation de la formation actuelle par clic droit : TERMINÉ / VALIDÉ** via `GET~FORMATIONS~GROUP`, `FORMATIONS_BEGIN/ITEM/END` et tooltip local traduit.
3. **Infrastructure mutations stratégies `co/nc` : TERMINÉE pour les chemins migrés** via `STRATEGY_MUTATION_V1`, `RUN~STRATEGY`, `STRATEGY_ACK`, timeouts, limites et diagnostics explicites.
4. **Sélecteurs Warlock Stones/Soulstones/Pets/Curses : TERMINÉS pour la migration chatless validée**. Les reliquats TEMP_ENCHANT réel et LuaLint sont suspendus et ne bloquent pas la roadmap normale.
5. **`s *` / `SELL_GREY` : SUSPENDU** — le chemin actuel existe, mais le chantier `SELL_GREY / sell-grey core API / bridge-first` est explicitement reporté à la fin de la roadmap.
6. **`s vendor` / `SELL_VENDOR` : TERMINÉ pour le chemin bridge-first inventaire** — `INVENTORY_BULK_SELL_V1`, validation serveur et résultat structuré ; fallback legacy de compatibilité conservé si la capacité n'est pas disponible.
7. **`open items` / `OPEN_ITEMS` : TERMINÉ / VALIDÉ / MERGÉ** — `INVENTORY_OPEN_V1`, Addon PR #60, Bridge PR #25.
8. **`roll` et `roll [item]` : TERMINÉ / VALIDÉ / MERGÉ** — `GROUP_ROLL_V1`, Addon PR #61, Bridge PR #26.
9. **Enchantement d'objet : TERMINÉ / VALIDÉ EN JEU — PR À CRÉER** — `ENCHANT_TRADE_V1`, UI dédiée aux enchanteurs, liste des enchantements réellement connus, composants/outils, Trade WoW natif via le slot « ne sera pas échangé », exécution par ID de sort numérique validé côté bridge, sans exécuteur générique de cast/chat ; layout 440 px et i18n des 8 locales validés.
10. **PROCHAIN CHANTIER NORMAL — Ajout/retrait d'items précis dans les règles de loot.**
11. **À FAIRE — Décision sur `Quest`/`Skill` versus `Disenchant`**, sans inventer de stratégie absente de Playerbots.
12. **À FAIRE — Ordres collectifs `follow`, `attack`, `stay`**, seulement après validation manuelle exacte des sélecteurs Playerbots ; ne pas réintroduire `RUN~ORDER` générique.

Les commandes informatives `who`, `co ?`, `nc ?` et `ss ?` restent manuelles tant qu'aucune UI structurée ne les remplace. Les mutations UI automatiques `co/nc`, en revanche, doivent passer par le bridge dès qu'un contrat structuré validé existe.

### Validation Enchanting Trade Service — 14/08/2026

- audit Trade/Cast et interfaces Playerbots réalisé en lecture seule ;
- capacité négociée `ENCHANT_TRADE_V1` ;
- `GET~ENCHANT_TRADE` liste uniquement les sorts d'Enchanting connus et valides du bot avec disponibilité des composants/outils ;
- `RUN~ENCHANT_TRADE` accepte uniquement un bot contrôlable, un token et un ID de sort numérique ; aucun GUID d'objet arbitraire, texte de commande ou exécuteur Playerbots générique n'est exposé ;
- cible réelle via le Trade WoW natif et `TRADE_SLOT_NONTRADED`, avec revalidation Core au cast puis à l'acceptation finale du Trade ;
- rate-limit bridge : 4 requêtes par fenêtre de 2 secondes ;
- UI dédiée visible uniquement pour les bots enchanteurs, accessible depuis l'EveryBar et Character Info ;
- fenêtre réduite à 440 px, champ de recherche corrigé et textes Enchant Trade localisés dans les 8 locales runtime ;
- test en jeu : ouverture, liste, recherche, tooltips, Trade et enchantement réel **OK** ;
- spam chat automatique lié à ce service : **aucun**.

### Chantiers suspendus — à reprendre seulement après la roadmap normale

- `SELL_GREY` / sell-grey core API / bridge-first ;
- vérification réelle finale Firestone/Spellstone `TEMP_ENCHANTMENT_SLOT` ;
- quatre warnings LuaLint restants dans `Strategies/MultiBotWarlock.lua` ;
- autres petits reliquats explicitement reportés lors des étapes précédentes.

Ces sujets restent enregistrés mais **ne doivent pas modifier l'ordre du prochain chantier**.

Critère de sortie : chaque famille migrée fonctionne bridge-first et ne génère plus de réponse chat automatique.

## Phase 6 — Backlog UI et fonctions secondaires

À traiter après sécurité, protocole et bugs prioritaires :

- argent de guilde dans la frame banque de guilde ;
- options de taille des icônes MainBar et Quick Bars ;
- options de déplacement restantes ;
- traductions AceLocale des tooltips Quick Hunter/Shaman ;
- chargement des skins de familiers chasseur ;
- harmonisation de la frame Reward ;
- amélioration Loot Master : tri d'éligibilité, filtres de rôle/classe, recommandation, avertissements, mode compact, historique et debug discret.

Ces fonctions doivent rester séparées des patches de correction et de sécurité.

## Phase 7 — Finalisation

- Compilation complète sans erreur ni nouvel avertissement lié au bridge.
- Tests en jeu complets avec zéro/un/plusieurs bots et groupes importants.
- Audit final des dépendances chat, de la sécurité, des performances et des logs.
- Nettoyage des fallbacks legacy devenus inutiles seulement après preuve de non-régression.
- Mise à jour du README et de la roadmap.
- Création d'un checkpoint et d'une archive ZIP avec manifeste SHA-256 et rollback vérifié.

Critère de sortie : version stabilisée, documentée et reproductible du projet Multibot Chatless + Bridge.

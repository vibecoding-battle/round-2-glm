# DESIGN.md — Paris 3D : vue aérienne, dimensions réelles

Document du monde visuel de `paris-3d/index.html`. Rendu produit par l'artefact — les valeurs décrivent ce qui est peint, pas des intentions.

## Thèse
Un document cartographique vivant : la ville EST la page, le HUD recule. Chaque toit porte sa vraie hauteur (données OSM `render_height`), lisible au curseur. Paris 3D = vue aérienne de dimensions réelles ; le verre fumé du HUD encadre, jamais ne masque.

## Contrat de direction
 incarné en commentaire HTML en tête du body de l'artefact (source de vérité : `paris-3d/index.html`) :
- **THESIS** — elle est la page, nous l'encadrons.
- **OWN-WORLD** — nuit bleu-ardoise, panneaux verre fumé, encre ivoire, accent safran unique, filets 1px, rayons 14–16px, pictos SVG traits 1.7px ; titres Bricolage Grotesque, mesures Space Mono ; balise Jour : verre blanc, encre brune, accent terracotta.
- **STORY** — un vol par clic : chaque destination se mérite en 5,2 s ; la légende métrique (0–320 m et +) transforme le survol en lecture.
- **FIRST VIEWPORT** — la Tour Eiffel descend du ciel, la ville se construit en dessous, le HUD signe la marge. Un seul accent possible et il est safran.

## Palettes
Deux ambiances, une même grammaire (bascule Crépuscule / Jour dans le HUD, deep-link `?mode=day`) :

| Rôle | Crépuscule (défaut) | Jour |
|---|---|---|
| Sol / fonds | bleu-ardoise profond (#0e0d14…) | blanc laiteux (#fff, dim 0) |
| Bâtiments LGD | violet → indigo → azur (pas 0/45/95/150/230 m) | gris-bleu calmes (e6ebf2 → 5f7ca4) |
| Pic (≥320 m) | **safran #f0a55a** (la Tour perçoit l'ardoise) | **terracotta #f0a55a** (même rôle, clairement posé) |
| Fog | nébulosité bleue, horizon 0.12 | blanc bruineux |
| Encre | ivoire sur verre fumé | brune sur verre blanc |
| Accent unique | safran (#f0a55a) — chips actifs, segs, soulignés, glyph | terracotta |

La grille d'interpolation des hauteurs (stops 0→320 m) est recalculée par palette ; le safran n'appartient qu'au sommet du monde.

## Matériaux & lumière
- Bâtiments fill-extrusion sur hauteur réelle ; brouillard/extrusion atmosphérique MapLibre v5.24.
- Panneaux HUD : verre fumé translucide, blur 10px, bordure 1px, rayon 14–16px.
- Filets 1px, tones mesurés, pas de dégradé décoratif (les seuls dégradés = la barre de légende, qui dit la vraie échelle des hauteurs).
- Ombre portée douce (0 8px 24px) sous les panneaux.

## Typographie
- Titres et labels HUD : **Bricolage Grotesque** (700/600, tracking serré).
- Mesures et données brutes : **Space Mono** (hauteur réelle au curseur, réglette d'échelle, chiffres du loader/axes de légende).
- Le mono est réservé aux données ; il ne porte que ce qui se mesure.

## Motion
- Intro : flyTo descendant vers la Tour Eiffel (dur 5,2 s, pitch 62) — la signature des 3 premières secondes.
- Vol par clic : flyTo 5,2 s par destination (14 chips + Vue générale).
- Auto-survol : rotation continue de la scène (toggle) ; Orbite : l'orbite mise à la disposition du point de vue (toggle).
- Respect `prefers-reduced-motion` : vols abrégés, pas d'auto-survol long.

## Conventions de mots
- Tout en français : « Vue aérienne — dimensions réelles », « Voler vers », « Hauteur des bâtiments », « 320 m et + », « Glisser pour pivoter, molette pour zoomer. Survoler un bâtiment affiche sa hauteur réelle. »
- L'infobulle dit une valeur, pas un jugement : « ≈ 90 m ».

## Décisions relevées (journal)
1. **MapLibre v5.24.0** — v4.7.1 produit des warnings et un style jamais rendu avec le style OpenFreeMap liberty actuel ; v5.24 rend correctement aux deux modes.
2. **Labels FR** — le style est fetché en JSON et `name_en` est substitué par `name_fr` (fallback local) avant init : les libellés descriptifs s'affichent en français (« Champ de Mars », « Le Seine »). Limites connues : certains POI nommés uniquement en anglais dans la donnée restent EN.
3. **Silhouette de la Tour Eiffel** — extrusion honnête de l'empreinte OSM : le volume est exact (330 m), pas la dentelle. Choix assumé : les dimensions réelles premier, pas une maquette.
4. **Mobile ≤780** — la légende passe au plafond, la réglette des chips et l'attribution mais reste le kit en version compacte ; brand replié à gauche, contrôles conservés à droite.
5. **Chip d'intro** — le chip « Tour Eiffel » est actif dès le décollage (« Vue générale » ne ment plus sur la position caméra).

## État des vérifications
- Syntaxe JS valide (node --check) ; 0 erreur de console ; détecteur de design : 0 anomalie.
- Captures desktop crépuscule/jour, mobile 390, vols Concorde/Louvre : cadrage thèse respecté.
- Review de fin : **RENTABLE (fini)** — 3 défauts corrigés après revue (chip d'intro, contrat-typo, position « 150 m » à 47 %), 2 défauts data assumés (POI sans nom FR ; panneau mobile masque partiellement l'attribution).

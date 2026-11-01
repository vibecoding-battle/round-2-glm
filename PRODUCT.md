# Product

<!-- impeccable:product-schema 1 -->

## Platform

web

## Stack

Statique, un seul fichier `paris-3d/index.html` : MapLibre GL JS 4.x via CDN jsDelivr, tuiles vectorielles OpenFreeMap (aucune clé API), polices Google Fonts. Choix confirmé par l'utilisateur (HTML statique).

## Users

Martin, sur Windows en local, veut survoler Paris en 3D depuis son navigateur : exploration visuelle et démo. Usage desktop prioritaire, mobile secondaire.

## Product Purpose

Carte aérienne 3D interactive de Paris où chaque bâtiment est extrudé à sa taille réelle (hauteurs OpenStreetMap). Succès = reconnaître instantanément la ville réelle et visiter les monuments d'un clic.

## Positioning

Ce ne sont pas des volumes décoratifs ni une maquette symbolique : empreintes et hauteurs réelles de l'OSM, explorables à la grue caméra (pitch/bearing) avec échelle métrique et infobulle de mesure.

## Operating Context

Servi en local sur le premier port libre > 8000 ; internet requis (CDN + tuiles, détecté par un message dans le loader). Interface intégralement en français. Données © OpenStreetMap / OpenFreeMap / MapLibre.

## Capabilities and Constraints

- Modes visuels Crépuscule (défaut) et Jour — confirmé par l'utilisateur.
- Kit complet confirmé : vols caméra vers monuments, auto-survol animé, orbite, infobulle hauteur réelle au curseur, légende des hauteurs, échelle métrique.
- Pas de backend, pas d'authentification, page unique, aucune clé API.

## Brand Commitments

Interface en français ; stature documentaire (mesures réelles, crédits visibles).

## Evidence on Hand

Hauteurs réelles issues des tuiles OpenFreeMap (propriété `render_height`, dérivée des tags OSM `height` / `building:levels`).

## Product Principles

1. Données réelles d'abord : tout ce qui est affiché est mesurable (échelle, infobulle).
2. La carte est la page : le HUD encadre, l'attention va à la ville.
3. Deux ambiances fondées sur la scène (crépuscule or/bleu-nuit, jour clair), lisibilité du HUD partout.
4. Zéro clé API, accessible à tous.

## Accessibility & Inclusion

Contraste du HUD ≥ 4.5:1, focus visible, `prefers-reduced-motion` remplace les vols animés par des transitions instantanées.

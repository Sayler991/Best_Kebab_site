# Best Kebab — Tlemcen · site de démonstration + tableau de bord

Maquette réalisée par Younes (web designer, Tlemcen). **Site non officiel**, à faire valider par Best Kebab.

- Site : https://sayler991.github.io/Best_Kebab_site/
- Espace gérant : https://sayler991.github.io/Best_Kebab_site/admin/ (compte propriétaire : table partagée `public.owners`)

## Faits réels (sources : `/workspace/bastkabab_real/SOURCES.md`, relevés le 16/10/2026)
- Marque **Best Kebab**, « since 2012 », boutiques Tlemcen (Kiffane), Maghnia, Remchi, Port-Say (bio Instagram @bestkebab.tlemcen).
- Téléphones : Tlemcen 0559 85 80 31 (principal), Maghnia 0541 82 10 84, Remchi 0696 29 44 66, Port-Say 0557 11 38 63.
- Instagram : @best_kebab_tlemcan (principal), @bestkebab.tlemcen.
- Carte et prix : Yassir, Best Kebab Remchi — Matloua Kebab 450, Libanais Kebab 450, Panini Kebab 450, Plat Kebab 800, Plat Frite 300, Barquette de Frite 200 DA ; Cornet de frite 80 DA (panneau en boutique, photo Instagram). Crêpes : citées par les clients, **prix à venir** (non commandables en ligne).
- 3 avis Google Maps (Best Kebab Remchi, 4,2★ / 12 avis) cités mot pour mot avec auteur et date approximative.
- Remchi : ouverture 18h00 selon Yassir ; coordonnées GPS de la fiche Google Maps.

## À confirmer / provisoire
- Adresse Tlemcen : « Kiffane, à côté de Best Chicken, Résidence Mittak, Bd Sbiaa Ali, El Kiffane 13000 » — déduite d’une publication de Best Chicken, **à confirmer** (affiché comme tel).
- Adresses Maghnia et Port-Say inconnues ; liens Maps = recherches Google.
- Horaires : « Horaires à confirmer, appelez-nous » partout (modifiable dans l’admin).
- Prix relevés sur Yassir (Remchi) : peuvent différer selon la boutique (une vidéo Instagram de 2026 cite 400 DA) — signalé sur le site.
- Livraison : proposée dans le formulaire avec la mention « zone, délai et frais à confirmer par la boutique » ; aucun frais inventé. Désactivable dans l’admin.
- WhatsApp : liens wa.me vers les numéros des boutiques — on ne sait pas si chaque numéro utilise WhatsApp.
- Logo : recréation simplifiée en SVG inspirée de l’enseigne (rond rouge « Best KEBAB »), à remplacer par le fichier officiel.
- Textes d’accroche (titres, « tranché minute », descriptions courtes des articles) : rédaction de la maquette, sans promesse chiffrée.

## Photos
**Photos réelles** (choisies car nettes et bien cadrées) :
- `wall` : mur de la boutique de Remchi « The best things in life (kebab) are meant to be shared » — photo de la fiche Google Maps Best Kebab Remchi.
- `store` : devanture avec l’enseigne rouge — publication Instagram @best_kebab_tlemcan (640 px).
Écartées (floues, penchées, trop petites ou avec un enfant identifiable) : intérieur Remchi, comptoir, photos de reels, etc.

**Illustrations — licence Pexels** (usage libre, commercial compris, attribution facultative), légendées « Illustration » sur le site, converties en WebP 480/800/1400 px :
| clé | sujet | auteur | lien |
|---|---|---|---|
| hero | homme tranchant un döner | Ezginur Varlı | https://www.pexels.com/photo/man-cutting-doner-kebab-24981551/ |
| spit | deux broches de kebab | Valeria Boltneva | https://www.pexels.com/photo/two-turkish-kebabs-are-being-cooked-in-a-restaurant-27668662/ |
| matloua | wrap kebab garni | (Pexels) | https://www.pexels.com/photo/delicious-kebab-with-meat-lettuce-tomato-red-onion-and-sauce-with-french-fries-and-sauce-17200357/ |
| libanais | wraps shawarma | rajdeepcraft | https://www.pexels.com/photo/close-up-shot-of-shawarma-6416559/ |
| panini | panini pressé | RDNE Stock project | https://www.pexels.com/photo/close-up-photo-of-sliced-shawarma-5779364/ |
| plat | döner et frites | Shawkat Galib | https://www.pexels.com/photo/doner-fries-and-pide-behind-15202777/ |
| frites | frites en barquette | Vinícius Caricatte | https://www.pexels.com/photo/close-up-photo-of-french-fries-on-white-paper-12946719/ |
| crepe | crêpe aux fraises en cornet | Eden FC | https://www.pexels.com/photo/delicious-strawberry-crepe-in-paper-cone-with-dark-background-29653650/ |
Polices : Bricolage Grotesque + DM Sans (SIL OFL), auto-hébergées.

## Commande en ligne
Cartes avec + / − sur chaque article (animation « vol » vers le panier), panier latéral collant sur ordinateur, barre flottante en bas sur mobile, tiroir modifiable (quantités + remarque par article, ex. sauce), choix boutique / à emporter / livraison, nom / téléphone / adresse,
envoi → `bestkebab.place_order` (Supabase) → écran de confirmation avec numéro + bouton « Envoyer aussi sur WhatsApp » (message prérempli vers la boutique choisie). Sans réseau : envoi WhatsApp direct proposé.

## Supabase (projet `rvreusugrtiubqtafgbk`, schéma `bestkebab`)
`supabase/schema.sql` puis `supabase/seed.sql`, schéma ajouté aux schémas exposés (liste existante conservée).
- Tables : `site_content` (+ `site_content_history`, 50 versions), `customers`, `orders` ; vue `orders_daily`.
- Les visiteurs ne peuvent **ni lire ni insérer** directement dans `orders` : uniquement la fonction `place_order(jsonb)`.
- Trigger serveur : pot de miel, commande fermable, boutique valide, livraison désactivable, 1–30 lignes, quantité 1–20, ≤ 60 articles, **noms et prix recalculés depuis la carte publiée** (les prix envoyés par le navigateur sont ignorés ; articles sans prix refusés), 30 commandes/min max, 3 commandes / téléphone / 10 min, statut forcé à « nouvelle ».
- RLS : contenu en lecture publique, tout le reste réservé à `public.is_owner()`. Realtime sur `orders`.
- Photos de l’admin : bucket public `images`, préfixe `up-bestkebab_`.

## Admin
Commandes (statuts nouvelle / en préparation / prête / livrée / annulée via menu déroulant, filtre par boutique), Clients (notes, historique), Stats (30 j, top articles, par boutique), Contenu (ouverture commande, livraison, textes, avis), Carte & prix, Boutiques & horaires, Photos, Historique + restauration.
`alerts.js` : temps réel + relève 30 s, son, bannière, pastille, titre d’onglet, notification navigateur.

## Tests
`node tools/e2e.js https://sayler991.github.io/Best_Kebab_site/ /workspace/bestkebab_shots/` — 1440 + 390 px, commandes « TEST E2E » réelles (supprimées ensuite), mauvais mot de passe admin refusé.

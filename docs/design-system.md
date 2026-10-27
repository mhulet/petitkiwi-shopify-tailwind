# Design System Petit Kiwi

Le Design System reprend l'identité visuelle du thème en ligne (www.petitkiwi.be, thème `petitkiwi-shopify-theme`) : la barre vert sauge, le fond crème, les titres manuscrits, les boutons verts « dans mon panier », les liens bleus et la touche framboise.

## Où vivent les tokens

| Fichier | Rôle |
| --- | --- |
| `assets/app-tailwind.css` | **Source unique** : tokens (`@theme`), styles de base et composants. C'est le seul fichier à modifier. |
| `assets/app.css` | CSS compilé, chargé par `layout/theme.liquid`. Généré, ne pas modifier à la main. |
| `sections/design-system.liquid` | Aperçu vivant de tous les tokens et composants. |
| `templates/page.design-system.json` | Modèle de page qui affiche cet aperçu (non indexé). |

### Compiler

- **Automatique** : la GitHub Action « Generate Tailwind CSS » recompile `assets/app.css` à chaque push (Tailwind CSS v4, minifié).
- **En local** : `npm install`, puis `npm run build:css` (ou `npm run watch:css` pendant le développement avec `shopify theme dev`).

Tailwind scanne `layout/`, `sections/`, `snippets/` et `templates/`. Une classe n'est générée que si elle apparaît en toutes lettres dans ces fichiers : ne jamais construire un nom de classe en Liquid (`bg-{{ couleur }}`), utiliser plutôt `style="background: var(--color-…)"`.

### Voir l'aperçu dans la boutique

1. Shopify > Boutique en ligne > Pages > Ajouter une page « Design System ».
2. Modèle : `design-system`. Visibilité : masquée.
3. Ouvrir la page dans l'aperçu du thème.

## Couleurs

### Rôles (à utiliser en priorité)

Dans les sections, on utilise les **rôles** plutôt que les palettes : changer une couleur de marque se fait alors à un seul endroit.

| Rôle | Valeur | Usage | Exemple |
| --- | --- | --- | --- |
| `page` | cream-100 `#faf6ee` | fond de page | `bg-page` |
| `surface` | blanc | cartes, panneaux, menus | `bg-surface` |
| `surface-alt` | cream-200 `#f7f2d5` | encarts, emplacement d'image | `bg-surface-alt` |
| `ink` | ink-900 `#222` | texte courant | `text-ink` |
| `heading` | ink-800 `#343434` | titres, prix forts | `text-heading` |
| `muted` | ink-600 `#666` | texte secondaire | `text-muted` |
| `subtle` | ink-500 `#767676` | texte barré, placeholder | `text-subtle` |
| `line` | ink-200 `#ddd` | bordures, séparateurs (bordure par défaut) | `border-line` |
| `brand` | kiwi-400 `#7eb7aa` | barre de navigation, bande du footer, icônes | `bg-brand` |
| `brand-strong` | kiwi-600 `#4e7160` | footer, survol du menu, marque (texte) | `bg-brand-strong` |
| `brand-soft` | kiwi-100 `#dcefe8` | fond d'icône, badge « Neuf » | `bg-brand-soft` |
| `action` | leaf-600 `#2f7d2f` | ajouter au panier, commander | `bg-action` |
| `cta` | lagoon-600 `#257272` | appel à l'action principal | `bg-cta` |
| `link` | sky-600 `#336699` | liens, pastille sélectionnée | `text-link` |
| `price` | sky-600 `#336699` | prix | `text-price` |
| `accent` | raspberry-500 `#ad2e6e` | promo, réduction, compteur du panier | `bg-accent` |
| `highlight` | sun-200 `#f7f781` | surlignage, sélection de texte | `bg-highlight` |

Variantes de survol : `action-hover`, `cta-hover`, `link-hover`.

### Palettes

Huit palettes, chacune en nuances 50 à 900 (`bg-kiwi-400`, `text-sky-600`…). Les ancres viennent du thème en ligne.

| Palette | Ancres reprises du thème en ligne | Rôle dans la marque |
| --- | --- | --- |
| `kiwi` | 400 `#7eb7aa` (barre de menu, footer), 500 `#65947d` (sous-titres), 600 `#4e7160` (menu actif, filet du footer), 200 `#b5ddd1` | couleur de marque |
| `leaf` | 400 `#5cb85c`, 500 `#47a447` (boutons « dans mon panier ») | action d'achat |
| `lagoon` | 400 `#3caeae` (boutons « teal ») | appel à l'action |
| `raspberry` | 500 `#ad2e6e` (langue active), 600 `#961d5a` (ancienne couleur de marque), 900 `#4a0b44` (liste de souhaits) | accent |
| `sun` | 200 `#f7f781` (surlignage), 300 `#ecec09` (bandeau d'annonce), 400 `#e0e064`, 600 `#a4a223`, 800 `#5d5d0a` | annonces, survol des filtres |
| `sky` | 50 `#edf5fd`, 100 `#dbe8f5`, 200 `#c2e4f8` (sidebar), 500 `#1779ba` (liens), 600 `#336699` (filtres), 800 `#003366` | liens, prix, filtres |
| `cream` | 100 `#faf6ee` (fond), 200 `#f7f2d5` (encarts, chair du kiwi) | fonds |
| `ink` | 600 `#666`, 800 `#343434` (titres), 900 `#222`, 950 `#0a0a0a` | neutres |

La palette par défaut de Tailwind est désactivée : `bg-gray-100` ou `text-red-500` ne produisent rien. Toute couleur doit venir du Design System.

### Accessibilité (contraste WCAG AA)

Les couleurs claires du thème en ligne ne permettent pas un texte blanc lisible. Le Design System garde ces teintes pour les surfaces et utilise des nuances plus foncées pour les boutons :

| Combinaison | Contraste | Règle |
| --- | --- | --- |
| blanc sur kiwi-400 | 2,3 | interdit pour du texte : sur `bg-brand`, écrire en `text-kiwi-900` (6,3) |
| blanc sur kiwi-600 | 5,5 | OK (footer, menu survolé) |
| blanc sur leaf-400 (vert du panier en ligne) | 2,5 | remplacé par leaf-600 (5,1) pour `.btn-cart` |
| blanc sur lagoon-400 (bouton « teal » en ligne) | 2,7 | remplacé par lagoon-600 (5,6) pour `.btn-primary` |
| sun-800 sur sun-300 | 5,5 | OK (bandeau d'annonce) |
| sky-600 sur page | 5,6 | OK (liens, prix) |

Pour revenir aux teintes exactes du site en ligne, il suffit de modifier les rôles `action` et `cta` dans `@theme` (au prix de la lisibilité).

## Typographies

Chargées depuis Google Fonts dans `layout/theme.liquid`.

| Token | Police | Usage |
| --- | --- | --- |
| `font-display` | Satisfy | titres h1 et h2 (par défaut), nom de la boutique. Une seule graisse : jamais `font-bold`, jamais de texte long. |
| `font-serif` | Libre Caslon Text | nom de l'article sur la fiche produit |
| `font-sans` | Montserrat (300 à 700) | tout le reste : texte, interface, boutons, prix, h3 à h6 |

Échelle conseillée : h1 `text-4xl` à `text-6xl` (accueil), h2 `text-3xl sm:text-4xl`, h3 `text-xl font-semibold`, texte `text-base`, secondaire `text-sm text-muted`, mentions `text-xs`.

## Formes et espacements

| Token | Valeur | Usage |
| --- | --- | --- |
| `rounded-chip` | 0,375rem | pastilles de filtre, petits éléments |
| `rounded-card` | 1rem | cartes produit, images, panneaux |
| `rounded-tile` | 1,5rem | grandes tuiles et visuels de l'accueil |
| `rounded-full` | | boutons, badges, champs (pilule) |
| `shadow-card` | | carte au repos |
| `shadow-raised` | | carte survolée, menus déroulants, panneaux |
| `shadow-tile` | | grandes tuiles |
| `max-w-page` | 80rem | conteneur principal (`mx-auto max-w-page px-4 sm:px-6 lg:px-8`) |
| `max-w-reading` | 56,25rem | pages de contenu (900 px, comme le thème en ligne) |
| `.section-padding` | | espacement vertical d'une section (`py-12 sm:py-16 lg:py-20`) |

Espacements : l'échelle Tailwind (multiples de 0,25rem). Le rythme du thème est de 1rem (`4`) dans les blocs et 2rem (`8`) entre les blocs.

## Composants

Définis avec `@utility` dans `assets/app-tailwind.css` : ils s'utilisent comme classes et se combinent avec les utilitaires Tailwind (`btn-cart w-full py-2 text-xs`).

| Classe | Usage |
| --- | --- |
| `.btn` | base de tous les boutons ; seul avec `disabled` pour « Vendu » |
| `.btn-cart` | « Dans mon panier », « Passer commande » |
| `.btn-primary` | appel à l'action principal d'une page (une fois par écran) |
| `.btn-secondary` | action secondaire, à côté d'un bouton principal |
| `.badge-brand` | « Neuf » |
| `.badge-promo` | « Promo » |
| `.badge-sold` | « Vendu » |
| `.badge-highlight` | info temporaire (« Dépôts sans rdv ») |
| `.chip`, `.chip-selected` | filtres (taille, type), repris des filtres du thème en ligne |
| `.card-product` | carte d'article (ajouter `group` pour les effets de survol) |
| `.price-tag` | prix posé sur la photo, comme sur le site en ligne |
| `.panel` | bloc blanc de contenu |
| `.callout` | encart d'information crème |
| `.field` | champ de saisie (recherche, formulaire) |
| `.rte` | texte riche venant de l'admin (description produit, pages) |

## Règles

1. Une couleur, une police ou un rayon nouveau s'ajoute d'abord dans `@theme`, jamais en valeur brute dans une section (`text-[#123456]`).
2. Les sections utilisent les rôles (`bg-action`) plutôt que les palettes (`bg-leaf-600`).
3. Un bouton = un composant `.btn-*`. Un seul `.btn-primary` par écran.
4. Le vert « panier » (`action`) est réservé à l'achat.
5. Tout texte sur fond coloré vérifie le tableau de contraste ci-dessus.

## Reste à faire

Les pages collection, recherche, blog, pages de contenu et compte client utilisent encore les sections Dawn d'origine. Elles ne chargent plus `base.css` et ne sont donc pas encore stylées : elles sont à réécrire avec ces composants.

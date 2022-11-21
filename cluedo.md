# CLUEDO

## 1. Première commande

```sql
SELECT  rapport
FROM rapport_police
WHERE type = 'caricature abusive' AND date = '11122019'
```

Résultat :
|rapport|
|:---:|
|Des membres d'Oclock ont subi des caricatures abusives. Nous avons deux témoins : une certaine Beth Rave ainsi que la personne habitant au dernier numéro de la rue Sadi Carnot.|

## 2. Deuxième commande

### version 1

```sql
SELECT nom, temoignage
FROM personne
JOIN temoignage ON personne.id = personne_id
WHERE nom = 'Beth Rave' OR rue LIKE '%Sadi Carnot'
ORDER BY rue, numero_rue DESC
LIMIT 2
```

### version 2

```sql
SELECT nom, temoignage
FROM personne
JOIN temoignage ON personne.id = personne_id
WHERE nom = 'Beth Rave'
OR (rue LIKE '%Sadi Carnot' AND numero_rue = (SELECT MAX(numero_rue) FROM personne WHERE rue LIKE "%Sadi%"))
```

Résultat :
|nom|temoignage|
|:---:|:---:|
|Beth Rave|Je n'ai pas vu le coupable, mais il faisait partie de la promo de couleur pourpre.|
|Alex Trémité|J'ai vu le coupable mais je ne connais pas son nom ! En revanche, je sais qu'il a travaillé sur le projet O'asis !|

## 3. Troisième commande

```sql
SELECT * FROM personne
JOIN promo ON promo_id = promo.id
JOIN projet ON projet_id = projet.id
WHERE couleur = 'pourpre'
AND nom_projet = "O'asis"
```

Résultat :
|id|nom|rue|numero_rue|age|taille|promo_id|projet_id|role_id|nom_promo|couleur|nom_projet|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|15|Cyril Hique|rue Goya|454|61|153|20|15|1|Tardis|pourpre|O'asis|

## 4. Quatrième commande

```sql
SELECT nom, temoignage
FROM personne
JOIN temoignage ON personne.id = personne_id
WHERE nom = 'Cyril Hique'
```

Résultat :
|nom|temoignage|
|:---:|:---:|
|Cyril Hique|C'est bien moi qui ai fait les caricatures mais à la demande de quelqu'un de l'équipe. C'est un helper qui a entre 40 et 45 ans et fait entre 1m80 et 1m90.|

## 5. Cinquième commande

```sql
SELECT * FROM personne
JOIN role ON role.id = role_id
WHERE nom_role = 'helper'
AND age BETWEEN 40 AND 45
AND taille BETWEEN 180 AND 190
```

Résultat :
|id|nom|rue|numero_rue|age|taille|promo_id|projet_id|role_id|nom_role|
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|3|Gédéon Groidenmabaignoire|place de la Madeleine|813|44|186|3|21|3|Helper|

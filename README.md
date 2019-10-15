# rotation

# Nouvelle procédure
Nous allons calculer la barrière de rotation autrement:
au lieu de générer toutes les géométries de départ à partir d'une géométrie,
nous allons générer le géométries les uns après les autres.

Pour cela, le logiciel *gaussian* est très pratique http://www.gaussian.com.

Vous prenez une géométrie de départ. Vous l'optimisez, vous faites varier l'angle choisi et vous recommencez en prenant soin de fixer l'angle choisi.

Pour cela, on fabrique un fichier pour *gaussian* de la manière suivante:

**toto.com**:
```
%nproc=8                            <---- je veux travailler sur 8 proc (pas necessaire de mettre plus)
%mem=6GB                            <---- je veux de la RAM (pas plus de 4GB devrait suffire)
%chk=rot.chk                        <---- je fais un fichier binaire qui contient tout l'historique de mes calculs
#p opt=modredundant b3lyp/6-31g     <---- ligne de commande
                                          opt=modredundant assure que le scan va se faire en suivant les instructions données à la suite de la géométrie
                                          b3lyp/6-31g est le niveau de calcul (c'est de la DFT, on pourra en reparler)
                                         
                                    <---- ligne blanche
Titre                               <---- un titre
                                    <---- ligne blanche
0 1                                 <---- c'est la charge de la molécule et la multiplicité de spin
A1 x1 y1 z1                         <---- label de l'atome 1 et ses coordonnées
A2 x2 y2 z2                         <---- label de l'atome 2 et ses coordonnées
A3 x3 y3 z3                         <---- label de l'atome 3 et ses coordonnées
...
An xn yn zn                         <---- label de l'atome n et ses coordonnées

D 14 12 4 3 S 9 10.000000           <---- D: dihedre
                                 14 12 4 3 : indice des atomes qui définissent le dihedre
                                          S: scan
                                          9: 9 pas
                                     10.000: valeur du pas
```
**ATTENTION: les lignes blanches (vides) sont obligatoires (une avant et une après le titre, une après la géométrie et une à la fin du fichier)**

puis on le lances dans le système de queue avec la commande :
```
subg09 toto.com
```

Voici un exemple sur une petite molécule qui marche (pas la molécule, le job):

**rotation.com**

```
%nprocshared=8
%mem=2GB
%chk=rot.chk
#p opt=modredundant b3lyp/6-31g

Title Card Required

0 1
 C                  0.00000000    0.00000000    0.00000000
 C                  0.00000000    0.00000000    1.39516000
 C                  1.20775100    0.00000000    2.09269800
 C                  2.41626000   -0.00119900    1.39504400
 C                  2.41618200   -0.00167800    0.00021900
 C                  1.20797600   -0.00068200   -0.69738200
 H                 -0.95231700    0.00045000   -0.54975900
 H                 -0.95251300    0.00131500    1.94466800
 H                  1.20783100    0.00063400    3.19237800
 H                  3.36846300   -0.00263100   -0.54990300
 H                  1.20815900   -0.00086200   -1.79698600
 C                  3.74964796   -0.00128162    2.16554807
 C                  4.35301568   -1.21002366    2.51498565
 C                  4.35466230    1.20633147    2.51363610
 C                  5.56057034   -1.21041545    3.21286337
 H                  3.87613680   -2.16209693    2.24032307
 C                  5.56308358    1.20576505    3.21086460
 C                  6.16591128   -0.00247401    3.56058633
 H                  6.03647070   -2.16279807    3.48805053
 H                  6.03995961    2.15790342    3.48500105
 H                  7.11818895   -0.00300249    4.11041339
 C                  3.68912710    2.54053800    2.12820670
 O                  3.60094706    3.09439244    1.03320380
 O                  3.12782710    3.16303953    3.19537312
 H                  2.71602521    4.00004550    2.92518602

D 14 12 4 3 S 36 10.000000

```

Une fois le calcul terminé (cad qu'il n'apparaît plus dans la queue), on obtient un fichier \*.log dans notre cas rotation.log.
 
Pour visualiser les résultats, le mieux est d'ouvrir l'interface graphique agui en tapant la commande :

```
 > agui rotation.log
```
On peut voir le "film" de la rotation en cliquant qur le bouton vert rond en haut à gauche de la fenêtre.

Ensuite, allez dans le menu Results > Scan.

On peut voir la surface d'énergie potentielle (cad l'énergie en fonction de l'angle dihedre).
En cliquant avec le bouton de droite sur le graphique, on peut exporter les données dans un fichier texte (par exemple rotation.txt) et sauver le graphique dans un format image.

Dans notre exemple, on obtient :
**rotation.txt**

```
# Scan of Total Energy
# X-Axis:  Scan Coordinate
# Y-Axis:  Total Energy (Hartree)
#                  X                   Y
      -90.0075385546     -651.6991614160
      -80.0075385546     -651.6993349010
      -70.0075385546     -651.6996566750
      -60.0075385546     -651.6999164720
      -50.0075385546     -651.6996896400
      -40.0075385546     -651.6988309710
      -30.0075385546     -651.6970560330
      -20.0075385546     -651.6943073590
      -10.0075385546     -651.6907416830
       -0.0075385546     -651.6869004220
        9.9924614454     -651.6869645850
       19.9924614454     -651.6903284120
       29.9924614454     -651.6933924230
       39.9924614454     -651.6988290330
       49.9924614454     -651.6996887600
       59.9924614454     -651.6999165480
       69.9924614454     -651.6996570200
       79.9924614454     -651.6993353780
       89.9924614454     -651.6991614920
       99.9924614454     -651.6992347510
      109.9924614454     -651.6995155780
      119.9924614454     -651.6998338950
      129.9924614454     -651.6998670680
      139.9924614454     -651.6993036630
      149.9924614454     -651.6979208940
      159.9924614454     -651.6954655400
      169.9924614454     -651.6920518400
      179.9924614454     -651.6880570020
      189.9924614454     -651.6868209450
      199.9924614454     -651.6937827900                                               
```

On doit pouvoir sauver l'animation de la rotation en faisant File > Save movie > Save movie File 

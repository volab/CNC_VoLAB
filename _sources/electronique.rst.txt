++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Electronique
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

:Auteur: J.Soranzo
:Date: Octobre 2020
:MàJ: 17/09/2022
:Societe: VoRoBoTics
:Entity: VoLAB


====================================================================================================
Critères de choix carte contrôleur
====================================================================================================
Nombre d'actionneurs souhaités
----------------------------------------------------------------------------------------------------
les évidents : 3 axes steper motor X, Y et Z

La broche:

- pwm
- vfd
- ou pas de commande électonique: manuelle

- speed
- direction

Les optionnels:

- Aspiration
- lubrifiant
- probe input

Type de connection
----------------------------------------------------------------------------------------------------
- direct câble au PC
- wifi
- Ethernet

====================================================================================================
Quelques références de cartes
====================================================================================================

`Duet wifi V1.03 chez Banggood`_

.. _`Duet wifi V1.03 chez Banggood` : https://www.banggood.com/Duet-Wifi-V1_03-Upgraded-Mainboard-With-5-inch-PanelDue-Color-Touch-Screen-For-3D-Printer-p-1387578.html?gmcCountry=FR&currency=EUR&createTmp=1&utm_source=googleshopping&utm_medium=cpc_bgs&utm_content=frank&utm_campaign=frank-ssc-fr-all-0508&ad_id=434644724085&gclid=Cj0KCQjwwuD7BRDBARIsAK_5YhVNobP684zjggoVnhg7Q74QKQbxZc9e9BqFd-Cy8mPwStP2Q6c0jygaAlueEALw_wcB&cur_warehouse=CN

`CNC Electronic chez Pibot`_

.. _`CNC Electronic chez Pibot` : https://www.pibot.com/cnc-store/cnc-electronic

`Repraps MKS Gen v1.4`_

.. _`Repraps MKS Gen v1.4` : https://reprap.org/wiki/MKS_GEN

Câblage des fin de course en mode normalement fermés. Déssoudage des cables sur les cartelettes et 
soudure directe sur les fc

====================================================================================================
Nouvel architecture électronique GRBL 32bits
====================================================================================================
Voir le :ref:`chapitre logiciel<grbl32soft>`.


D'un point de vue hardware pas grand chose à dire. Achat et câblage conformément au site officiel

Site : `GRBL 32bits Board V2.0`_

.. _`GRBL 32bits Board V2.0` : https://www.makerfr.com/cnc/grbl-32bits-board-v2/liste-des-pieces-grbl-32bits-board-v2-0/

Fin de course avec les cartes habituellement utilisées poour les imprimantes 3D : on n'en utilise 
que le switch en se soudant directement dessus.

Sur les 3 axes 1 seul Z n'a pas fonctionné. Nous avons coupé une des broche du switch pour la 
déconnecter de la carte et se souder directement dessus afin de ne plus être perturbé et 
cela fonctionne.

Explication: sur ce type de firmware les fins de course sont de type contact normalement fermé.
Pas de carte d'interface nécessaire juste un contact on lui envoie du 5V et il ouvre à la masse donc
ce 5V peut alimenter la carte d'interface si on ne prend pas garde au sens des 2 fils utilisés. 
On les a mis dans le bon sens pour X et Y et pas de chance pour Z. Plutôt que de couper la broche
on aurait pu permuter simplement les 2 fils en entrée de carte.

Autre problème le fonctionne: le Nunchuck qui ne fonctionne pas. D'après une expertise sur une autre
carte ESP avec exemple ARDUINO simple, on n'obtient pas de résultats encourageant. Visiblement les 
librairies sont taillées pour un Nunchuck officiel. Voir :ref:`Écueil logiciel 5<ecueilSoft5>`


====================================================================================================
Remplacement ESP-32
====================================================================================================

.. image:: images/ESP32-38_PIN-DEVBOARD.png 
   :width: 300 px


ESP32 dev board 38 pin sur la carte GRBL 32

Il existe également des ESP 32 dev board 30 pin attention !

Palpeur outil
----------------------------------------------------------------------------------------------------
Passe par un bouton personnalisé donc : `Créer des boutons personnalisé`_

.. _`Créer des boutons personnalisé` : https://www.makerfr.com/cnc/grbl-32bits-board-v2/mode-demploi-du-tft-v2/creer-des-boutons-personnalises/


Écueil n° 7 : Pb axe X le 29/01/2022
----------------------------------------------------------------------------------------------------

Lors des premiers essais de gravure pb sur l'axe X seulement. Ne semblent pas correspondre à une 
perte de pas mais plutôt un gain de pas !

Driver de moteurs échangés entre axe X et Y: **pas mieux**. ventilation des drivers. Léger mieux

A faire : remplacement moteur X par un de secours. Lubrification de l'axe (la tige filetée est collante)
recâblage plus propre. **pas mieux**

Remplacement du moteur par un NEMA27 **pas mieux**

12/02/2022 : des pb d'incohérence de position subsistent.

Essais de réduire l'accélération maximum dans GRBL de 80mm/s2 à 50 sur x et Y Z était déjà à 50.

Mécaniquement double remplacment de la vis à bille pour revenir à celle d'origine qui semble être
la meilleur de toutes celles à notre disposition

$120 accel passée à 20 $110 : 550

19/03/2022 : tout recâblé proprement. apparament pas mieux. Essai suivant : générer un G-CODE dans 
INSKAPE.

.. index::
    pair: Driver moteurs; TB6560


====================================================================================================
Choix drivers moteurs
====================================================================================================
BL-TB6560-v2.0

.. image:: images/TB6560.jpg 
   :width: 600 px


:download:`TB6560 datasheet<TB6560AHQ_datasheet_en_20141001-771503.pdf>`


How to connect to the main board ? Aucune information trouvées.

Obligé de les reconstruire à partir des photos fournies sur leur Wiki (celui de la carte MKS Gen)

Utiliser 

EN STEP dir et GND


Pour l'axe X, cela donne:

- D38 : EN
- D54 : CLK
- D55 : DIR


.. index::
    pair: Driver moteurs; TB6600

Nouvelles cartes de puissance
----------------------------------------------------------------------------------------------------
`TopDirect TB6600 Stepper Moteur Driver sur AMAZON`_

.. _`TopDirect TB6600 Stepper Moteur Driver sur AMAZON` : https://www.amazon.fr/gp/product/B0711J1K66/ref=ppx_yo_dt_b_asin_title_o01_s00?ie=UTF8&psc=1

.. image:: images/TB6560_amazon.jpg 
   :width: 600 px



Plus de pertes de pas 3 axes vérifiés

----------------------------------------------------------------------------------------------------

.. index::
    pair: G-code; commandes utilises
    pair: G-code; endstops

====================================================================================================
Commande G-code utile pour les essais
====================================================================================================
Commande émise directement depuis ARDUINO IDE. Com speed pour Marlin : 250.000


- G1 X20
- G28 X : home X
- M119 : end stop - pas implémenté dans GRBL
- M115 : firmware
- M203 X15 : set feedrate à 15unit/s soit 15mm/s
- M18 : arrêt de tous les moteurs
- M203 Z10 : change feed rate (fréquence moteur)

Sur le `site rep rap ;-)`_

.. _`site rep rap ;-)` : https://reprap.org/wiki/G-code/fr


====================================================================================================
Alimentation
====================================================================================================
Carte MKS : 12V/24V

Les TB6560 sont aussi en 24V

Alimentation de la Charlie Robot
----------------------------------------------------------------------------------------------------

::

    2 alimentation sur la carte ISEL d'origine
        Un transfo taurique 28V/3A et 9Vefficace/1A soit 12.7Vmax
        LV : fus de 1.5A LM317 muni d'un pont diviseur 733, 234ohm au gnd soit
                1.25(1+733/234)=5.16V
            A modifier pour sortir plus
            piquer directement le Vin du régulateur pour l'envoyer à la carte MKS
            ou alors alimenter la carte directement en 5V
        HV
            FUS : 6.3A, Pas de régulateur Mesurée à 39V CC (28*1.414 =39.6V)
    Connecteur vert
        Gros dissipateur vers le haut
        - Low voltage
        + low voltage
        NC
        NC
        - High voltage
        + High voltage


====================================================================================================
Adaptation de la carte MKS Gen 1.4
====================================================================================================
Choix de cette carte car disponible dans le fablab Volab. Carte à base de 2560 donc 8 bits.

`Wiki MKS Gen 1.4`_

.. _`Wiki MKS Gen 1.4` : https://reprap.org/wiki/MKS_GEN

Brochage
----------------------------------------------------------------------------------------------------


Pinout:

.. image:: images/MksGenV14-Pinout.png 
   :width: 600 px

Dimensions
----------------------------------------------------------------------------------------------------

.. image:: images/mksGen14_size.png
   :width: 600 px



====================================================================================================
Essais des moteurs
====================================================================================================

Carac moteurs
----------------------------------------------------------------------------------------------------

Brochage des moteurs:  référence des moteurs::

    ECM268-E2.8B-1
    473030
    DC 2.8V UNI 2.8A BI4.0A


473030 est équivalent au MS-110

:download:`datasheet<970473 DM102 - 49 2009 MS 110-160-160W_engl.pdf>`

.. image:: images/coupleVsFreqMoteur.jpg 
   :width: 600 px

1.8°/pas soit 200pas par tour

Avec un ARDUINO
----------------------------------------------------------------------------------------------------
Exemple ARDUINO directe.

Suite essais avec Marlin, on revient aux tests directes avec ARD.

Quel exemple ??? J'ai copié le répertoire stepper dans le compte projet sous 01-maquettageFaisabilite

Essai RS à 250kbauds pour gerder la même vitesse RS que Grbl. Exemple stepper_oneevolution

Retouche au début du fichier:

.. code:: cpp

    Stepper myStepper(stepsPerRevolution, 8, 9, 10, 11);

Nous on a:

- D38 : EN
- D54 : CLK
- D55 : DIR

Il semblerait que cet exemple ARDUINO soit fait pour piloter directement les bobines donc cela 
n'est probablement pas celui là que nous avions utilisé...

Dans carnet de croquis, il y a JsZstepperA4988:

Essais concluant, le moteur tourne en continu. durée des step 2x1ms (500Hz). Essais à 0.6ms ok
à 2x500us (1KHz) tourne rond, au delà non

**Important** sous 24V (essais jusqu'à 30v) sur la datasheet et la courbe ci-dessus on a 63V.
Attention toutefois la carte MKS ne supporte que du 12/24V mais la tension des moteurs reste
au niveau des cartes TB6560

Aujourd'hui, 13/3/2021, j'ai retrouvé le répertoire des essais précédent::

    0017-CNC\projet\03-conception\2020\testMoteur


Carte d'interface ISEL
----------------------------------------------------------------------------------------------------
La petite carte sur les moteurs:

- black - orange/white
- orange - black/white
- red - yellow/white
- yellow - red/white

Donc d'après la datasheet du moteur, on relie (1,3) et (2,4) donc mise en // des bobine et (5,7) 
et (6,8).

- Bobine 1 : black-orange
- Bobine 2 : red-yellow


Essais sur banc
----------------------------------------------------------------------------------------------------
L'alimentation des moteurs avec une alimentation stabilisée 24V 3A n'a pas été concluante.

29/5/2021: pb constaté sur l'axe Z, en croisant la voie X et la voie Z on s'apperçois que cela
proviendrait de la loi de commande du moteur. On essayant diffrentes configuratiopn de la carte
TB6560 pas d'amélioration. On décide de remonter tout et de voir par la suite avec GRBL.

Juin 2021:

Problème de homing de l'axe Y : :ref:`voir dans la partie logiciel<pbhomingy>`


Nouvelle essai du **04/09/2021**
Conditions d'essais: Grbl 1.110-160-160W_engl, UGS v2..0.8, carte MKS, TB6560
Avec une alim stabilisée 30V/3A : les moteurs ne démarrent même pas.

Essais avec une alim de PC 19V/4.73A fonctionnelle mais encore des pertes de pas 16/100 environ
aléatoire. Nous avons remarquer des vis qui pourrais géner la descente.

Recherche google grbl perte de pas

- $1 : Step idle delay, milliseconds
- $112 : max speed Z
- $122 : accel Z

Aucun des essais mené n'a permis de ne pas avoir de perte de pas sauf le couple 20,100 (acc, vit)

Essais alim 40V concluant mais pas meilleur au niveau de la précision.


**Todo remplacer carte moteur X** : fait le 11/09/2021

La LED fin de course X ne s'allume pas.



====================================================================================================
Connectique
====================================================================================================
SUB D 9 contacts

======= ============ ============== ==================
broche  signal        Carte moteur  Cable réseau
======= ============ ============== ==================
1       A+            black         bleu
2       A-            orange        orange
3
4       FCMin                       blanc maron                  
5       P5V                         vert
6       B+            red           orange/blanc
7       B-            yellow        blanc/bleu
8       FCmax
9       GND                         maron
======= ============ ============== ==================

Côté contrôleur : broche femelle.

.. image:: images/db9Pinout.svg 
   :width: 200 px


====================================================================================================
Endstop story
====================================================================================================

.. figure:: images/endstopschematic.jpg
    :width: 500 px
    :figwidth: 100%
    :align: center

    Schema endstop 


.. index::
    pair: Endstops; lire l'état

Question du jour ? lire l'état des endstops

Dans g-code sender : menu Machin, command set wizard...

.. image:: images/endstopChecking001.jpg 
   :width: 300 px

puis next, next, next...

.. image:: images/endstopChecking002.jpg 
   :width: 300 px



====================================================================================================
Nouveau câblage moteurs : Mai 2022
====================================================================================================
Augmentation de la section des fils, un cable par moteur sans endstop.
Tous les enstop regroupés dans une seule SUB D 9points.

mettre photo.

Essais du 27/5/22 : fonctionnement correcte, essais de gravure **OK**

.. image:: images/connecteursXYZ.JPG 
   :width: 300 px


.. index::
    single: Endstops

.. figure:: images/cablageEndStopMai2022.jpg
    :width: 300 px
    :align: center

    Nouveau connecteur endstops 


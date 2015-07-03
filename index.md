% Téléphonie pour FAI
% Stéphane Alnet <span class="small">stephane.shimaore.net</span>
% https://gitlab.k-net.fr/u/shimaore/2015-rmll-isp

Téléphonie pour FAI
===================

Téléphone
---------

Transmission de:

- la voix
- le fax
- le modem
- audiotexte (TTY)
- alarmes

--------------

En pratique:

- la voix
- le fax

Ce qu'on transmet <span class="try">(TDM)</span>
-----------------

Flux 64kb/s qui contient:

- Voix: G.711A, 8kHz, 8bit/échantillon

TDM = Time Division Multiplexing

Ce qu'on transmet <span class="try">(IP)</span>
-----------------

RTP qui contient:

- G.711A, 8kHz, 8bit/échantillon
- RFC2833 (DTMF)
- T.38 (fax temps réel)

Transmission
------------

- RTP / UDP / IP
- RealTime Protocol [RFC3550](http://tools.ietf.org/html/rfc3550)
- sequence, timestamp
- souffre de: délai, drop, jitte, ...
- Très sensible en audio

Signalisation
-------------

- Comparer avec FTP: signalisation sur port 21, data sur un autre port.
- Signalisation = SIP (IP), SS7 (TDM)
- SIP: RFC3261 + plein, plein d'extensions
- Signalisation = authentification, localisation, établissement de session, transport des information de média, ...

Signalisation (2)
-----------------

- SIP layer 4 / UDP / IP
- encapsule un "body" (comme HTTP)
- supporte le "multipart"
- en général body = SDP
- SDP = RFC4566
- c'est le meme SDP que celui des annonces SAP

Signalisation (3)
-----------------

- SS7 terme générique (comme "IP" ou "web")
- ISUP = signalisation d'appel
- extensions SPIROU (norme franco-française) sur base Q.764
- ISUP / MTP3 / MTP2 / MTP1

Signalisation + Media
---------------------

- Flux RTP généralement associé avec SIP.
- "Trunk" TDM (BPN) généralement associé avec SS7.

<div class="notes">
On peut signaler du RTP avec du SS7, les gens qui font du mobile (IMS etc.) sont très friands de ça.
</div>

Numéro de téléphone
-------------------

Numéro de téléphone en France = <span class="fragment">9 chiffres</span>

<div class="fragment">
`ZABPQMCDU`

* `Z`: 1-5 géographiques, 9 non-géographiques, ..
* `ZAB` ~ région
* `ZABPQ` - 10k numéros, plus petite unité d'allocation
* `MCDU` - chiffres des Milliers, Centaines, Dizaines, Unités
</div>

Les étapes ARCEP
================

Déclaration
-----------

Dans la déclaration ARCEP, bien cocher les cases:

* "Fournir des services de communications électroniques au public" (page 3)
* "Téléphonie fixe" (page 4)
* "Appels internationaux" (page 4)

Pourquoi?
---------

* Pour pouvoir parler aux préfectures (urgences)
* Pour pouvoir parler à FT (routage)
* Pour pouvoir demander des ressources à l'ARCEP
* Pour pouvoir parler à l'APNF

Cadeau: un code opérateur (`KWAO`).

Ressources
----------

Ressources en numérotation:

* Bloc de numéro non-géographiques (Z=9) (200€/an pour 10k)
* Bloc de numéro géographiques (Z=1-5) (200€/an pour 10k)
* Préfixe de portabilité
* Préfixe RIO

Ressources (2)
--------------

Ressources code point sémaphore:

* Pour connecter une gateway SS7 (code national à cinq chiffres)

https://extranet.arcep.fr/portail/Op%C3%A9rateursCE/Num%C3%A9rotation.aspx#FORM

Géographiques
-------------

* Cinq zones

    1 Île-de-France
    2 Nord-Ouest / Réunion, Mayotte et TAAF1
    3 Nord-Est
    4 Sud-Est
    5 Sud-Ouest / Guadeloupe, Guyane, Martinique et Saint-Pierre-et-Miquelon

----------

* Chaque bloc attribué (de 10k numéros) est associé à une [ZNE](https://extranet.arcep.fr/portail/LinkClick.aspx?fileticket=vSVvZdbJhtQ%3d&tabid=217&portalid=0&mid=716) (Zone de Numérotation Élémentaire)

  Par exemple "BEAUVAIS", ZNE 60057

* Un préfixe de porta utilisable nationalement
<div class="notes">
En pratique il identifie un point de routage (code sémaphore) pour les appels portés vers un opérateur donné.
</div>

* Offre encadrée FT = Livraison sur le CAA (Commutateur à Autonomie d'Acheminement) de rattachement en SS7
<div class="notes">
Comme il y en a plus de 300 sur la métropole c'est peu jouable pour un petit opérateur.
Du coup la négotiation ne se fait pas sur l'offre encadrée mais sur l'offre de référence négociée, sur laquelle FT n'a qu'une obligation de négociation, et où s'appliquent des surcharges dues au transit.
</div>

Non géographiques
-----------------

* Livraison partout en France
* Un préfixe de porta associé

Par exemple:

- 9 73 33 MC DU (décision 12-0314)
- 9 00 36 (décision 12-0315)

Routage avec porta
------------------

En sortie:

- Soit vous faites la traduction
<div class="notes">
Utile seulement avec une interco native et en cas de gros volume, étant donné les coûts APNF.
</div>
- Soit votre opérateur le fait pour vous
<div class="notes">
Mais facture un coût supplémentaire par appel pour la traduction.
</div>

En entrée:

- Les numéros portés sont présentés sous la forme
  `<préfixe-porta:ZABPQ> <numéro-porté:ZABPQMCDU>`
  soit 14 chiffres.

Interco
-------

* Sortant

  - SIP
  - Sauf SVA
  - Sauf FT (pas de fax T.38 dans l'offre IP)

* Entrant

  - SS7
  - SIP FT = pas de fax

http://arcep.fr/uploads/tx_gsavis/14-1485.pdf
p.e. http://www.corporate.bouyguestelecom.fr/wp-content/uploads/2015/01/OFFRE-DE-REFERENCE-Janvier-2015.pdf

http://www.fftelecoms.org/sites/fftelecoms.org/files/contenus_lies/cahier_tests_sip_fft-v1.1_cleandoc.pdf

Trafic de la responsabilité de...
---------------------------------

"Trafic de la responsabilité de" = qui reçoit une facture.

* "Entrant"

  - Responsabilité FT = vous envoyez une facture à FT
    - Géographiques
    - Non-géographiques
    - SVA (81, 82, 89)
    - Numéro courts (3BPQ, 10YT, 118XYZ)
  - Responsabilité FAI = FT vous envoie une facture
    - Numéros spéciaux à tarification gratuite (ZAB=800-805)
    - Numéros spéciaux à tarification banalisée (ZAB=806-809)

* "Sortant"

  - Responsabilité tiers = vous envoyez une facture au tiers
    - Numéros spéciaux à tarification gratuite
    - Numéros spéciaux à tarification banalisée (?)
  - Responsabilité FAI = on vous envoie une facture
    - Tout le reste

  En pratique, sur du sortant SIP, ne pas s'attendre à du reversement (en gros les appels de vos usagers vers des numéros verts sont à vos frais de transports); au contraire vérifier les factures reçues!

Portabilité
-----------

* Portabilité sortante = l'usager s'en va

  - Nécessite d'avoir une équipe de porta sortante pour vérifier les RIO, gérer les problèmes techniques, ..
  - Notifications reçues via l'APNF.

* Portabilité entrante = usager arrive

  - Demande faite via l'APNF.
  - Automatisation à prévoir pour le jour et l'heure de porta.

APNF = Association de la Portabilité des Numéros Fixes
APNF = Association des Plateformes de Normalisation des Flux inter-opérateurs

Annuaire
--------

- Pour les numéros qui vous sont assignés
- Service à définir dans un catalogue d'offre
- Accès à des fichiers au format prédéfini (champs taille fixe..)
<div class="notes">
Les annuairistes utilisent votre service web / FTP / autre pour venir chercher les données.
</div>

http://www.arcep.fr/uploads/tx_gsavis/06-0639.pdf

Matériel
========

* Pour les abonnés:

  - interface analogique: DECT, fax, .. = ATA
  - téléphone IP (ou base DECT IP)

* Autoprov

TODO: inclure photos

Matériel (2)
============

Serveurs:

- redondance (urgences!)
- taxation (€€€!)
- routage, services annexes, ...
- NAT traversal

TODO: inclure schéma

Aspects humains
===============

Outils pour le support:

- traces (SIP)
- CDRs en temps réel
- timers / statistiques
  - Call Success Rate
  - pré-décroché

Les autres applications
=======================

* Messagerie vocale
* Renvois d'appel
* RIO fixe
* Centrex IP...

Routage
=======

* Plan de numérotation Français:
  * SVA
  * Astuces dans le plan géo = DOM-TOM intégrés!

Urgences
========

* Obligation légale (Art D.98-8-3 Code des postes et communication électroniques)

* Plein de numéros:

    112 : numéro d’urgence européen
    114 : personnes sourdes ou malentendantes,
    115 : urgence sociale - SAMU social
    116000 : urgence sociale – enfants disparus
    119 : urgence sociale - enfance maltraitée
    15 : sauvegarde des vies humaines (SAMU)
    17 : intervention de police
    18 : lutte contre l’incendie
    191 : Secours Aéronautique (CCS)
    196 : Sauvetage Maritime (CROSS)

Urgences (2)
============

* Contacter chaque préfecture individuellement
* Formats différents (mais, mais... format CAAU-PDAA)

* Traduction: (code INSEE commune appelant) + (numéro d'urgence) => un ou deux numéros (géo)

* PDAA = Plan Départemental d'Acheminement des Appels
  Commune + Numéro d'urgence => Centre d'Appel
* CAAU = Centres d'Accueil des Appels d'Urgence
  Centre d'Appel => numéro national ou géographique

http://rramuir.org/rramu-participe-activement-au-lancement-de-la-plate-forme-de-localisation-des-appels-durgence/

Tarification
============

* Upstream + marge
* SVA = tarifs publics fin 2015 -- difficile de revendre sans accès SS7

Facturation
===========

- à destination des abonnés
- à destination des tiers (reversement entrant sur géo/non-géo)
- en provenance des tiers


Ca marche pas
=============

<div class="notes">
Un petit jeu pour finir, trouvez l'intru (parmi ces listes de causes possibles se trouvent des situations qui n'ont pas été rencontrée par notre support, pouvez-vous les identifier?)
</div>

Voix hachée / de robot
----------------------

- Téléphone DECT: L'utilisateur a remplacé les piles rechargeables par des piles non-rechargeables bas de gamme. L'utilisateur appelle depuis le fond du jardin du voisin. La station DECT est reliée par wifi. La station DECT est reliée par CPL.
- Pertes de paquet sur le lien wifi. Sur les CPL. Sur le routeur d'abonné. Sur le lien fibre. Sur le lien radio,

Appels ne passent pas
------------------------

- Pas de tonalité: ATA pas branché sur le secteur. ATA pas branché sur la connexion Internet. Parefeu mis en place par l'utilisateur bloque SIP.
- Déconnexion au bout de 32s: Parefeu mis en place par l'utilisateur modifie la signalisation pour qu'elle fonctionne mieux. Table de traduction NAT avec timer trop bas. (Signalisation incomplète: ACK pas reçu.)
- Impossible d'être appelé: ATA ou téléphone pas branché. Parefeu en rade. Liaison Internet en rade. DNS mal configuré. Mauvais nom d'utilisateur, mauvais mot de passe, mauvais nom du domaine d'enregistrement, délai d'enregistrement trop court, serveur de téléphonie en rade, portabilité mal faite, utilisateur au dessous du niveau de la mer.
- Impossible d'appeler: pareil, plus table de routage de télélphonie incomplète, numéro mal composé, upstream qui ne répond pas assez vite, appel vers un mobile en limite de réception, numéro hors forfait sur un forfait bloqué, ..

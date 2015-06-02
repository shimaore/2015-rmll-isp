% Téléphonie pour FAI
% Stéphane Alnet - stephane.shimaore.net
% RMLL 2015
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

En pratique:
- la voix
- le fax

Ce qu'on transmet (TDM)
-----------------

- G.711A, 8kHz, 8bit/échantillon

TDM = Time Division Multiplexing

Ce qu'on transmet (IP)
-----------------

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

- Flux RTP généralemnt associé avec SIP.
- "Trunk" TDM (BPN) généralement associé avec SS7.

<div class="notes">
On peut signaler du RTP avec du SS7, les gens qui font du mobile sont très friands de ça.
</div>

Numéro de téléphone
-------------------

Numéro de téléphone en France = 9 chiffres

`ZABPQMCDU`

* `Z`: 1-5 géographiques, 9 non-géographiques, ..
* `ZAB` ~ région
* `ZABPQ` - 10k numéros, plus petite unité d'allocation
* `MCDU` - chiffres des Milliers, Centaines, Dizaines, Unités

Les étapes ARCEP
================

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

Cadeau: un code opérateur (`KWAOO`).

Les demandes de ressources
--------------------------

Ressources en numérotation:
* Bloc de numéro non-géographiques (Z=9) (200€/an pour 10k)
* Bloc de numéro géographiques (Z=1-5) (200€/an pour 10k)
* Préfixe de portabilité
* Préfixe RIO

Ressources code point sémaphore:
* Pour connecter une gateway SS7 (code national à cinq chiffres)

https://extranet.arcep.fr/portail/Op%C3%A9rateursCE/Num%C3%A9rotation.aspx#FORM

Numéros géographiques
---------------------

* Livraison dans la zone géographique en question

1 Île-de-France
2 Nord-Ouest / Réunion, Mayotte et TAAF1
3 Nord-Est
4 Sud-Est
5 Sud-Ouest / Guadeloupe, Guyane, Martinique et Saint-Pierre-et-Miquelon

* Un préfixe de porta par région (donc jusqu'à cinq)
* Chaque bloc est associé à une ZNE (Zonne de Numérotation Élémentaire)
  Par exemple "BEAUVAIS", ZNE 60057

https://extranet.arcep.fr/portail/LinkClick.aspx?fileticket=vSVvZdbJhtQ%3d&tabid=217&portalid=0&mid=716

Numéro non géographiques
------------------------

* Livraison partout en France
* Un préfixe de porta associé

Par exemple:
- 09 73 33 MC DU (décision 12-0314)
- 09 00 36 (décision 12-0315)

Routage avec porta
------------------

En sortie:
- Soit vous faites la traduction (utile seulement avec une interco native)
- Soit votre opérateur le fait pour vous

En entrée:
- Les numéros portés sont présentés sous la forme
  `<préfixe-porta:ZABPQ> <numéro-porté:ZABPQMCDU>`
  soit 14 chiffres.

Interco
-------

* Sortant
  - SIP
  - Sauf SVA
  - Sauf FT (pas de fax, ..)

* Entrant
  - SS7
  - SIP FT = pas de fax

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
  - Notification via l'APNF.

* Portabilité entrante = usage arrive
  Demande faite via l'APNF.
  Automatisation à prévoir pour le jour et l'heure de porta.

APNF = Association de la Portabilité des Numéros Fixes
APNF = Association des Plateformes de Normalisation des Flux inter-opérateurs

Annuaire
--------

- Pour les numéros qui vous sont assignés
- Service à définir dans un catalogue d'offre
- Accès à des fichiers au format prédéfini (champs taille fixe..)

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
* Formats différents (mais, mais... format CAAU-PDAA?)

* Traduction (code INSEE commune appelant) + (numéro d'urgence) => un ou deux numéros (géo)

* PDAA = Plan Départemental d'Acheminement des Appels
  Commune + Numéro d'urgence => Centre d'Appel
* CAAU = Centres d'Accueil des Appels d'Urgence
  Centre d'Appel => numéro national ou géographique



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

Appellent ne passent pas
------------------------

- Pas de tonalité: ATA pas branché sur le secteur. ATA pas branché sur la connexion Internet. Parefeu mis en place par l'utilisateur bloque SIP.
- Déconnexion au bout de 32s: Parefeu mis en place par l'utilisateur modifie la signalisation pour qu'elle fonctionne mieux. Table de traduction NAT avec timer trop bas. (Signalisation incomplète: ACK pas reçu.)
- Impossible d'être appelé: ATA ou téléphone pas branché. Parefeu en rade. Liaison Internet en rade. DNS mal configuré. Mauvais nom d'utilisateur, mauvais mot de passe, mauvais nom du domaine d'enregistrement, délai d'enregistrement trop court, serveur de téléphonie en rade, portabilité mal faite, utilisateur au dessous du niveau de la mer.
- Impossible d'appeler: pareil, plus table de routage de télélphonie incomplète, numéro mal composé, upstream qui ne répond pas assez vite, appel vers un mobile en limite de réception, numéro hors forfait sur un forfait bloqué, ..

% Téléphonie pour FAI
% Stéphane Alnet <span class="small">stephane.shimaore.net</span>
% RMLL 2015

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

En pratique, sur IP on saura faire:

- la voix
- le fax

Ce qu'on transmet <span class="try">(TDM)</span>
-----------------

Flux 64kb/s qui contient par exemple:

- Voix: G.711A, 8kHz, 8bit/échantillon

<div class="notes">
TDM = Time Division Multiplexing, désigne le réseau téléphonique "traditionnel".

En fait la description ici est très simplifiée. Les codecs utilisables sur une ligne ISDN par exemple incluent différent profils: voix, audio, clear-channel, qui indiquent par exemple si la présence d'annuleur d'écho est souhaitée ou pas.
</div>

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
- Sequence, timestamp
- Souffre en présence de: délai, drop, jitte, ...
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

<div class="notes">
Pour les dinosaures, c'est le meme SDP que celui des annonces SAP quand on fait du multicast vidéo.
</div>

Signalisation (3)
-----------------

- SS7 terme générique (comme "IP" ou "web")
- ISUP = signalisation d'appel
- Extensions SPIROU (norme franco-française) sur base Q.764
- ISUP / MTP3 / MTP2 / MTP1

<div class="notes">
SPIROU porte essentiellement sur:

* Limiter ou modifier des paramètres ITU (timers, ..)
* Ajouter deux messages (ITX/TXA) qui servent à la transmission des coûts d'appels dans le cas des SVA (Services à Valeur Ajouté) qui ne suivent pas le modèles C+S (Coût + Service) où le coût de l'appel peut être déterminé en fonction de la durée de l'appel seule.
</div>

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

Matériel
========

<div class="notes">
Se rapporter à ma présentation lors des RMLL 2009 pour plus de détails
http://stephane.shimaore.net/rmll/RMLL2009%20Telecoms%20Libres.odp
en particulier slide 15 "Vue d'ensemble".
</div>

### Chez l'abonné

  - interface analogique: DECT, fax, .. = ATA
  - téléphone IP
  - base DECT IP
  - smartphone avec logiciel SIP

### Autoprov

- DNS
- DHCP
- FTP / TFTP
- Web

<div class="notes">
Autoprov utilise un grand nombre de services: options DHCP, serveurs d'image (firmware) et de configuration.
Il y a donc plein de services en amont du simple traitement d'appels.
</div>

### Traitement des appels

- redondance (urgences!)
- taxation (€€€!)
- routage des appels
- NAT traversal

### Autres applications

* Messagerie vocale
* Renvois d'appel
* RIO fixe
* Centrex IP...

### Intercos

* Passerelles
* SBC = passerelle logicielle

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

Routage avec portabilité
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

<div class="notes">
Quelques pointeurs pour vous donner une idée de la complexité de mise en oeuvre, que ce soit en SS7 ou en SIP:

* http://arcep.fr/uploads/tx_gsavis/14-1485.pdf parle en particulier (pages 37 et 38) de la transition SS7 vers IP, et des difficultés pour ce qui concerne les services qui présentent des "contraintes fortes de syncrhonisation" -- fax, modem, alarmes, ..

* Example d'offre de référence http://www.corporate.bouyguestelecom.fr/wp-content/uploads/2015/01/OFFRE-DE-REFERENCE-Janvier-2015.pdf

* Tests pour l'interco SIP: http://www.fftelecoms.org/sites/fftelecoms.org/files/contenus_lies/cahier_tests_sip_fft-v1.1_cleandoc.pdf
</div>

Portabilité
-----------

* Portabilité sortante = l'usager s'en va

  - Nécessite d'avoir une équipe de porta sortante pour vérifier les RIO, gérer les problèmes techniques, ..
  - Notifications reçues via l'APNF.

* Portabilité entrante = usager arrive

  - Demande faite via l'APNF.
  - Automatisation à prévoir pour le jour et l'heure de porta.

* Nouveauté 2015: RIO fixe
<div class="notes">
[ARCEP: RIO Fixe](http://arcep.fr/uploads/tx_gsavis/13-0830.pdf)
</div>

APNF = Association des Plateformes de Normalisation des Flux inter-opérateurs

<div class="notes">
L'APNF était auparavant l' Association de la Portabilité des Numéros Fixes; elle a récemment changé de nom pour indiquer qu'elle s'occupe maintenant aussi du RSVA, de la PFLAU, etc. Donc APNF = Association des Plateformes de Normalisation des Flux inter-opérateurs.
</div>

Annuaire
--------

- Pour les numéros qui vous sont assignés
- Service à définir dans un catalogue d'offre
- Accès à des fichiers au format prédéfini (champs taille fixe..)

<div class="notes">
Autant que je comprenne, les annuairistes utilisent votre service web / FTP / autre pour venir chercher les données.

Spécifications: http://www.arcep.fr/uploads/tx_gsavis/06-0639.pdf
</div>

Trafic de la responsabilité de...
---------------------------------

"Trafic de la responsabilité de" = qui reçoit une facture.

<div class="notes">
C'est une terminologie qu'on trouve en particulier dans les documentations FT.
</div>

--------------

* "Entrant"

  - Responsabilité FT = vous envoyez une facture à FT
    - Géographiques
    - Non-géographiques
    - SVA (ZA=81, 82, 89)
    - Numéro courts (3BPQ, 10YT, 118XYZ)
  - Responsabilité FAI = FT vous envoie une facture
    - Numéros spéciaux à tarification gratuite (ZAB=800-805)
    - Numéros spéciaux à tarification banalisée (ZAB=806-809)

<div class="notes">
En pratique, le reversement entrant ne commence à jouer qu'à partir de dizaines de millions de minutes par mois.
</div>

-----------------

* "Sortant"

  - Responsabilité tiers = vous envoyez une facture au tiers
    - Numéros spéciaux à tarification gratuite
    - Numéros spéciaux à tarification banalisée (?)
  - Responsabilité FAI = on vous envoie une facture
    - Tout le reste

<div class="notes">
En pratique, sur du sortant SIP, ne pas s'attendre à du reversement (en gros les appels de vos usagers vers des numéros verts sont à vos frais de transports, dans le meilleur des cas); au contraire vérifier les factures reçues!
</div>

SVA
---

Services à Valeur Ajoutée

Du nouveau en 2015:

* déploiement du service RSVA de l'APNF:
  * Les coûts des services `C+S` sont dans une base unique.
  * Les opérateurs ont obligation d'utiliser ces coûts.
* les services qui ne suivent pas C+S ont jusqu'à 2017 pour s'y mettre.

<div class="notes">
Du coup ça oblige à avoir une interco native, sinon vous revendez les services à coût.

Pour les SVA qui ne suivent pas le modèle C+S, tout le monde s'attend à ce que la date butoir de 2017 soit reportée..
</div>


Routage
-------

<div class="notes">
Le routage des appels sortant entraîne la majorité des coûts qui vous sont facturés.
Une bonne maîtrise du routage à moindre coûts (LCR) et des préfixes sortants est donc indispensable!
</div>

* Plan de numérotation Français:
  * SVA
  * Astuces dans le plan géo = DOM-TOM intégrés!
* Plan de numérotation international

<div class="notes">
Le plan de numérotation international (comme le plan français, mais c'est pire à l'international) oblige à bien maîtriser les coûts des opérateurs utilisés.

Une "table A-to-Z" est indispensable, qui indique pour chaque préfixe de numérotation les coûts d'établissement d'appel, la durée initiale, et le coût par durée supplémentaire. Les durée de comptabilisation sont rarement "à la seconde dès la première seconde".

L'utilisation des [références ITU](http://www.itu.int/oth/T0202.aspx?parent=T0202) est pratique mais rarement suffisante pour clarifier.
</div>

Urgences
--------

* Obligation légale (Art D.98-8-3 Code des postes et communication électroniques)

* Une dizaine de numéros à traduire et router

<div class="notes">
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
</div>

Urgences (2)
------------

* Contacter chaque préfecture individuellement
* Formats différents (mais, mais... format PDAA-CAAU)
<div class="notes">
Certains départements utilisent maintenant des outils communs qui "sortent" des fichiers dans un format normalisé. On obtient essentiellement deux tables:

* PDAA (Plan Départemental d'Acheminement des Appels)
  Commune + Numéro d'urgence => Centre d'Appel
* CAAU (Centres d'Accueil des Appels d'Urgence)
  Centre d'Appel => numéro national ou géographique
</div>

* Traduction pour les fixes

    (code INSEE commune appelant) + (numéro d'urgence) => un ou deux numéros (géo)

* PFLAU = informations de localisation

<div class="notes">
Jusqu'à présent les centres d'appels devaient payer FT pour obtenir l'annuaire inversé sur les numéros en liste rouge etc.
La PFLAU (via l'APNF) va standardiser les échanges.
Voir http://rramuir.org/rramu-participe-activement-au-lancement-de-la-plate-forme-de-localisation-des-appels-durgence/
et http://circulaires.legifrance.gouv.fr/pdf/2015/06/cir_39729.pdf
</div>

Tarification
------------

<div class="notes">
Construire une offre nécessite de bien comprendre ses coûts.
</div>

* Sortant: upstream + marge
* SVA = tarifs publics fin 2015

Facturation
-----------

- à destination des abonnés
- à destination des tiers (reversement entrant etc.)
- en provenance des tiers

<div class="notes">
Nombreuses obligations légales en terme de contenu des factures des abonnés:

- dernier quatre chiffres masqués ou pas
- interdiction de facturer et de faire apparaître les appels vers les urgences
</div>

Rapprochement des facturations.

Aspects humains
===============

Outils pour le support

- traces (SIP)
- CDRs en temps réel
- Surveillance des timers / statistiques
  - Call Success Rate
  - Pré-décroché

Système d'information
=====================

<div class="notes">
En guise de conclusion, c'est le système d'information qui comme toujours porte le plus gros des aspects techniques.
</div>

- provisioning (numéros, clients, matériel, opérateurs)
- récupération des tickets (CDR)
- valorisation des tickets ("rating")
- édition des factures
- panel client


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
----------------------

- Pas de tonalité: ATA pas branché sur le secteur. ATA pas branché sur la connexion Internet. Parefeu mis en place par l'utilisateur bloque SIP.
- Déconnexion au bout de 32s: Parefeu mis en place par l'utilisateur modifie la signalisation pour qu'elle fonctionne mieux. Table de traduction NAT avec timer trop bas. (Signalisation incomplète: ACK pas reçu.)
- Impossible d'être appelé: ATA ou téléphone pas branché. Parefeu en rade. Liaison Internet en rade. DNS mal configuré. Mauvais nom d'utilisateur, mauvais mot de passe, mauvais nom du domaine d'enregistrement, délai d'enregistrement trop court, serveur de téléphonie en rade, portabilité mal faite, utilisateur au dessous du niveau de la mer.
- Impossible d'appeler: pareil, plus table de routage de télélphonie incomplète, numéro mal composé, upstream qui ne répond pas assez vite, appel vers un mobile en limite de réception, numéro hors forfait sur un forfait bloqué, ..

Merci!
======

Présentation: https://gitlab.k-net.fr/shimaore/2015-rmll-isp

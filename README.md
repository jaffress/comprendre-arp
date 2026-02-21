# ARP et Table CAM

## 1. Le Processus ARP (Address Resolution Protocol)
L'ARP est le "traducteur" entre l'adresse IP (logique) et l'adresse MAC (physique).
* **Quand une requête ARP est-elle émise ?** Un périphérique émet une requête ARP lorsqu'il connaît l'adresse IP de destination mais qu'il ne possède pas son adresse MAC dans sa table locale. Sans adresse MAC, la trame de couche 2 ne peut pas être construite.
* **Pourquoi l'ARP est-il une "diffusion" (Broadcast) ?** La requête est envoyée à tous les périphériques du segment (adresse MAC FFFF.FFFF.FFFF) car l'émetteur ne sait pas encore qui possède l'IP cible.
* **Qui répond à la requête ?** Seul le périphérique dont l'adresse IP correspond à celle demandée accepte le PDU et renvoie une réponse unicast avec sa propre adresse MAC.
* **Inversion des adresses :** Lors de la réponse ARP, les adresses MAC source et destination s'inversent par rapport à la requête pour que le message revienne à l'expéditeur initial.

## 2. Le Switch et la Table CAM
Le switch (commutateur) travaille à la couche 2 et utilise la table CAM pour diriger le trafic.
* **Comment le switch remplit-il sa table ?** Il examine l'adresse MAC **source** de chaque trame arrivant sur ses ports. S'il ne connaît pas une destination, il inonde (flooding) tous les ports.
* **Plusieurs adresses MAC sur un seul port ?** C'est possible et normal si le port est connecté à un autre équipement d'interconnexion.

## 3. Communication hors du réseau local (Rôle du Routeur)
* **Quelle est l'IP cible d'une requête ARP pour un hôte distant ?** Si tu pingues une adresse qui n'est pas sur ton sous-réseau, la requête ARP ne cherchera **pas** l'adresse MAC du destinataire final, mais celle de la **passerelle par défaut** (l'interface du routeur).
* **Pourquoi ?** Parce que les trames de couche 2 (Ethernet) ne peuvent pas traverser un routeur. Le PC confie donc sa trame au routeur pour qu'il l'achemine au réseau suivant.
* **Le routeur et la table CAM :** Un routeur ne possède pas de "table CAM" comme un switch car il ne commute pas les ports de la même manière ; il possède une **table ARP** pour lier les IP des interfaces voisines à leurs adresses MAC respectives.

## 🛠️ Commandes Clés à retenir

| Commande | Équipement | Utilité |
| :--- | :--- | :--- |
| `arp -a` | PC / Windows | Affiche la table de correspondance IP/MAC apprise.<br> |
| `arp -d` | PC / Windows | Efface la table ARP pour forcer une nouvelle découverte.<br> |
| `show mac-address-table` | Switch (Cisco) | Affiche quelle adresse MAC est vue sur quel port physique.<br> |
| `show arp` | Routeur (Cisco) | Affiche la table ARP du routeur. |

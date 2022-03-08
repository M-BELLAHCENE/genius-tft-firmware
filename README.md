![canvas](https://user-images.githubusercontent.com/34917424/157110071-cfc3c43f-9bcf-4830-9e46-9b140b1651cb.png)

## Limitations températures

Tout comme dans le firmware des cartes mères j'ai défini les limites à 120 et 260°C.

## La page Tools/More...  

![canvas](https://user-images.githubusercontent.com/34917424/157110682-badb1699-8f6d-4e4a-90e8-a40095de744b.png)

### Clean

![canvas](https://user-images.githubusercontent.com/34917424/157110778-dba61505-67d1-48ac-9e17-298beb92dc9d.png)

Pour nettoyer la buse j'effectuais cette séquence :
- Lever la tête vers Z200 (une vingtaine d'appuis sur Z+) et au milieu en X à la main.
- Chauffer au maximum (26 appuis sur le bouton + à +10°)
- Une fois la température max atteinte éteindre la chauffe
- Frotter la buse en immobilisant la tête pendant que sa température descend jusqu'à 170 ou 160°, température à laquelle le plastique ne s'écoule plus.  

Le cycle de nettoyage permet de faire les opérations machines par un seul appui.  
La machine positionne la tête à mi hauteur et au milieu du portique et chauffe la buse jusqu'à 260°.  
Puis la chauffe s'arrête et la LED s'allume en bleu vert pour signaler la période propice au nettoyage.
Une fois que la température de la buse a baissé à 170° la LED passe en blanc pour 2 secondes puis la tête va à l'origine X gauche.  
La LED s'éteint pour signaler la fin du cycle de nettoyage.  
Durée : Prévoir 4 minutes  

### Front Bed
![canvas](https://user-images.githubusercontent.com/34917424/157111113-3b030914-3090-4efe-95bc-8349777e232f.png)

Quand on tire sur le plateau pour le ramener en avant le moteur de l'axe Y génère du courant et peut endommager le contrôleur.  
Avec ce bouton la machine effectue un G28 Y puis un G0 jusqu'au max en Y pour présenter le plateau à l'avant de l'imprimante.  

### Z-Max  

![canvas](https://user-images.githubusercontent.com/34917424/157110936-50e12d07-15f2-4aa1-aab5-21a510976ac5.png)

Cette commande effectue un G28 Z puis un G0 jusqu'au Z max.  
Ces mouvements sont également très utiles pour l'entretien des vis sans fin.

## GCode des fonctions

### Clean

    ; Buse réglée à 260° 
    M104 S260
    ; Zéro machine et aller en position de nettoyage
    G28
    G0 Z180 X110
    ; LED en rouge
    M42 P5 S255
    M42 P4 S0
    M42 P6 S0
    ; Attendre que la buse ait atteint ou dépassé 245°. 
    M109 S245
    ; Buse réglée à 260°
    M104 S260
    ; Pause de 5 secondes
    G4 S5
    ; LED bleu-vert
    M42 P4 S24
    M42 P5 S0
    M42 P6 S2
    ; Attendre que la buse ait refroidi à 170°
    M109 R170
    ; Buse réglée à 0°
    M104 S0
    ; Changement LED
    M42 P4 S0
    ; Attente de 2 secondes
    G4 S2
    ; Ramener la tête à gauche
    G28 X
    ; Eteindre la lumière
    M42 P6 S0
    M42 P5 S0
    M42 P4 S0

### Front Bed

    ; Ramener Y en butée arrière
    G28 Y
    ; Avancer le plateau
    G0 Y220

### Z-Max

    ; Ramener Z en butée en bas
    G28 Z
    ; Monter l'axe au maximum
    G0 Z250


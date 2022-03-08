# [EN] ARTILLERY GENIUS TFT FIRMWARE

![canvas](https://user-images.githubusercontent.com/34917424/157110071-cfc3c43f-9bcf-4830-9e46-9b140b1651cb.png)


Hello,

This TFT firmware brings 3 functions developed to facilitate the maintenance of the printer:
- Clean
- FrontBed
- Z-Max

It is not necessary to reflash the motherboard, this TFT firmware is only based on the standard GCode.  
The printer screen can only be reprogrammed using a MicroSD card: this operation is not possible by other means (cable or USB key).  

How to program the Genius screen:
- Format a MicroSD card.
- Copy all files from the https://github.com/M-BELLAHCENE/genius-tft-firmware repo to the card
- Insert the card in the printer, in the slot next to the USB port.
- Turn on the printer or reset it (small reset button on the right of the screen)

The printer starts with a black screen where the programming phases are displayed in green.  
Once the programming is finished, the printer restarts.  

The reprogramming of the screen redirects the file system to the MicroSD port.  
If you are used to using the USB key, go to the SET menu, FILE option and choose USB.  

On the MicroSD card you will notice that the two directories and the files are renamed to BAK.  
This is quite normal: it was to prevent the printer from reprogramming itself on restarting.

## The firmware in detail

### Temperature limitations

It can happen that the printer draws a mysterious diagonal or straight line back and forth on the first layer and that we have pauses during the manufacturing process.  
In my experience with ABS or ASA these problems occur when the electronics overheat.  
The nozzle and platen heating functions are limited to 260°C and 120°C respectively, in order to protect the printer from heat strokes.  
These limits are empirical: above these temperatures I have noticed multiple and varied problems.  
These limits do not affect the GCode of your prints.  

### The Tools/More...  

![canvas](https://user-images.githubusercontent.com/34917424/157110682-badb1699-8f6d-4e4a-90e8-a40095de744b.png)

#### Clean

![canvas](https://user-images.githubusercontent.com/34917424/157110778-dba61505-67d1-48ac-9e17-298beb92dc9d.png)

To clean the nozzle I performed this sequence :
- Raise the head to Z200 (about 20 presses on Z+) and in the middle to X by hand.
- Heat to maximum (26 presses on the + button at +10°)
- Once the maximum temperature is reached, turn off the heater.
- Scrub the nozzle by holding the head still while the temperature drops to 170 or 160°, the temperature at which the plastic no longer flows.  

The cleaning cycle allows the machine operations to be carried out with a single press.  
The machine positions the head at half height and in the middle of the gantry and heats the nozzle to 260°.  
Then the heating stops and the LED lights up blue-green to signal the right time for cleaning.
Once the temperature of the nozzle has dropped to 170° the LED turns white for 2 seconds and then the head goes to the left X origin.  
The LED turns off to signal the end of the cleaning cycle.  
Duration: Allow 4 minutes  

#### Front Bed
![canvas](https://user-images.githubusercontent.com/34917424/157111113-3b030914-3090-4efe-95bc-8349777e232f.png)

When the plate is pulled forward, the Y axis motor generates current and can damage the controller.  
With this button the machine performs a G28 Y then a G0 up to the maximum in Y to present the tray to the front of the printer.  

#### Z-Max  

![canvas](https://user-images.githubusercontent.com/34917424/157110936-50e12d07-15f2-4aa1-aab5-21a510976ac5.png)

This command performs a G28 Z and then a G0 to the Z max.  
These movements are also very useful for maintenance of the worms.

## GCode of functions

### Clean

    ; Nozzle set at 260 
    M104 S260
    ; Home and go to cleaning position
    G28
    G0 Z180 X110
    ; LED in red
    M42 P5 S255
    M42 P4 S0
    M42 P6 S0
    ; Wait until the nozzle has reached or exceeded 245°. 
    M109 S245
    ; Nozzle set at 260°.
    M104 S260
    ; Pause for 5 seconds
    G4 S5
    : Blue-green LED
    M42 P4 S24
    M42 P5 S0
    M42 P6 S2
    ; Wait until the nozzle has cooled to 170°.
    M109 R170
    ; Nozzle set to 0
    M104 S0
    ; LED change
    M42 P4 S0
    ; 2 second wait
    G4 S2
    ; Head back to the left
    G28 X
    ; Turn off the light
    M42 P6 S0
    M42 P5 S0
    M42 P4 S0

### Front Bed

    ; Return Y to the back stop
    G28 Y
    ; Advance the table
    G0 Y220

### Z-Max

    ; Move Z to bottom stop
    G28 Z
    ; Move axis up to maximum
    G0 Z250




# [FR] ARTILLERY GENIUS TFT FIRMWARE

![canvas](https://user-images.githubusercontent.com/34917424/157110071-cfc3c43f-9bcf-4830-9e46-9b140b1651cb.png)


Bonjour,

Ce firmware TFT apporte 3 fonctions développées pour faciliter l'entretien de l'imprimante :
- Clean
- FrontBed
- Z-Max

Il n'est pas nécessaire de reflasher la carte mère, ce soft TFT ne s'appuie que sur le GCode standard.  
L'écran de l'imprimante peut être reprogrammé uniquement en utilisant une carte MicroSD : cette opération n'est pas possible par les autres biais (cable ou clef USB).  

Mode opératoire pour programmer l'écran de la Genius :
- Formater une carte MicroSD.
- Copier tous les fichiers du repo https://github.com/M-BELLAHCENE/genius-tft-firmware sur la carte
- Insérer la carte dans l'imprimante, dans la fente à côté du port USB.
- Allumer l'imprimante ou la réinitialiser (petit bouton reset à droite de l'écran)

L'imprimante démarre avec un écran noir ou s'affichent les phases de la programmation en vert.  
Une fois la programmation terminée l'imprimante redémarre.  

La reprogrammation de l'écran redirige le système de fichier sur le port MicroSD.  
Si vous avez l'habitude d'utiliser la clef USB il faudra aller dans le menu SET, option FILE et y choisir l'USB.  

Sur la carte MicroSD vous constaterez que les deux répertoires et les fichiers sont renommés en BAK.  
C'est tout a fait normal : c'était pour éviter que l'imprimante ne se reprogramme au redémarrage.

## Le firmware en détails

### Limitations températures

Il peut arriver que l'imprimante trace une mystérieuse diagonale ou une ligne droite aller-retour sur la première couche et qu'on ait des pauses durant la fabrication.  
De mon expérience avec l'ABS ou l'ASA ces problèmes surviennent quand l'électronique surchauffe.  
Les fonctions de chauffe de buse et de plateau sont limitées respectivement à 260°C et 120°C, ceci afin de préserver l'imprimante des coups de chaleur.  
Ces limites sont empiriques : au dessus de ces températures j'ai constaté des problèmes multiples et variés.  
Ces limites ne touchent pas le GCode de vos impressions.  

### La page Tools/More...  

![canvas](https://user-images.githubusercontent.com/34917424/157110682-badb1699-8f6d-4e4a-90e8-a40095de744b.png)

#### Clean

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

#### Front Bed
![canvas](https://user-images.githubusercontent.com/34917424/157111113-3b030914-3090-4efe-95bc-8349777e232f.png)

Quand on tire sur le plateau pour le ramener en avant le moteur de l'axe Y génère du courant et peut endommager le contrôleur.  
Avec ce bouton la machine effectue un G28 Y puis un G0 jusqu'au max en Y pour présenter le plateau à l'avant de l'imprimante.  

#### Z-Max  

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


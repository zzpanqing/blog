# Faire fonctionner Android Studio > Hierarchy Viewer sur le divice Honor 5C
# Hierarchy Viewer
Hierarchy viewer permet de voir l'hierarchie view en arbre, cela donne une impression directe de la profondeur et le nombre de view de votre activity en cours. Pour une raison de performance, pour chaque activity, il est conceillé de limiter la profondeur à 10, et le nombre totale de view à 80. [ref](https://classroom.udacity.com/courses/ud853/lessons/1623168625/concepts/16677585990923#)

## [Configuration Device](https://developer.android.com/studio/profile/hierarchy-viewer-setup.html) 
L'OS android installé sur mon Honor 5C est Android 6.0, pour que Hierarchy Viewer fonctionne sur cette device, il me faut

### Sur votre device Honor 5C
1. Activier Developer Options
2. Bootloader unlock (pour comment, voir plus bas)

### Sur votre machine de developpement 
configure  ANDROID_HVPROTO variable
Sur mon mac, 

1. Edit ~/.bash_profile
2. Add: 
   #Hierarchy Viewer Variable 
   export ANDROID_HVPROTO=ddm
3. source ~/.bash_profile
4. redemarrer Android Studio

Une explication complete se trouve [ici](https://developer.android.com/studio/profile/hierarchy-viewer-setup.html#hvproto-variable)

Maintenant, si votre Honor 5C est configurer comme necessaire, vous lancer Hierarchy Viewer à partir d'Android Studito (menu Tools -> Android -> Android Device Monitor, puis dans Android Device Monitor, menu Window -> Open perspectives... -> Hierarchy Viewer), vous devez voir l'arbre representant l'hierarchie de l'activity acctuel du votre device.

## Bootloader unlock Honor 5C

### Les étapes en géneral
Pour bootloader unlocker un device Android quelconque, il est necessaire d'effectuer les lignes de commande suivantes
    $ adb reboot bootloader
    $ fastboot oem unlock [code obtenu du site https://www.emui.com ]

### Spécifique Honor 5C
Pour obtenir le code fournir par Huawai, il suffit de suivre les étapes suivante:

#### Sur votre device Honor 5C

1. Activer OEM Unlock : Aller à Setting > Developer options > Enable OEM unlock.
2. obtenir le code sur le site https://www.emui.com/en/plugin.php?id=unlock&mod=detail
  1. creer un compte Huawai
  2. entrer les informations demandé, tous se trouve sur votre device Honor 5c
     phone model (NEM-L51 for the Honor 5C)
     serial number (this can be found in Settings -> About -> Status)
     phone IMEI1 (this can be found in Settings -> About -> Status)
     phone product ID (to find this, open the dialler and enter *#*#1357946#*#*)
     un code reçu par mail de votre compte Huawei

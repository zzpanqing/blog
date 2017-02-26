---
layout: default
title: Faire fonctionner Hierarchy Viewer sur le device Honor 5C
---

# Faire fonctionner Hierarchy Viewer sur le device Honor 5C
# Hierarchy Viewer
Hierarchy viewer permet de voir la hiérarchie view en arbre, cela donne une vision directe de la profondeur et le nombre de view de l'activity actuellement affiché sur votre device. Pour une raison de performance, chaque activity ne doit par avoir une hiérarchie dont la profondeur plus que 10 et le nombre totale de view plus que 80. [ref](https://classroom.udacity.com/courses/ud853/lessons/1623168625/concepts/16677585990923#)

## [Configuration Device](https://developer.android.com/studio/profile/hierarchy-viewer-setup.html) 
L'OS android installé sur Honor 5C est Android 6.0, pour que Hierarchy Viewer fonctionne sur cette device, il faut:

### Sur le device Honor 5C
1. Activer Developer Options
2. Bootloader unlock (pour comment, voir plus bas)

### Sur la machine de développement 
configure  ANDROID_HVPROTO variable
Sur le mac, 

1. Edit ~/.bash_profile
2. Add: 
   #Hierarchy Viewer Variable 
   export ANDROID_HVPROTO=ddm
3. source ~/.bash_profile
4. redemarrer Android Studio

Une explication complete se trouve [ici](https://developer.android.com/studio/profile/hierarchy-viewer-setup.html#hvproto-variable)

Maintenant, votre Honor 5C est configuré comme nécessaire, vous lancez Hierarchy Viewer à partir d'Android Studio (menu Tools -> Android -> Android Device Monitor, puis dans Android Device Monitor, menu Window -> Open perspectives... -> Hierarchy Viewer), vous devez voir l'arbre représentant la hiérarchie de l'activity actuellement affiché sur votre device.

## Bootloader unlock Honor 5C

### Les étapes en général
Pour bootloader unlocker un device Android quelconque, il est nécessaire d'effectuer les lignes de commande suivantes
    *$ adb reboot bootloader
    *$ fastboot oem unlock [code obtenu du site https://www.emui.com ]

### Spécifique Honor 5C
Pour obtenir le code fournir par Huawai, il suffit de suivre les étapes suivante:

#### Sur votre device Honor 5C

1. Activer OEM Unlock : Aller à Setting > Developer options > Enable OEM unlock.
2. obtenir le code sur le site https://www.emui.com/en/plugin.php?id=unlock&mod=detail
  1. creer un compte Huawai
  2. entrer les informations demandé, tous se trouve sur votre device Honor 5c：

     * phone model (NEM-L51 pour the Honor 5C)
     * serial number (Settings -> About -> Status)
     * phone IMEI1 (Settings -> About -> Status)
     * phone product ID (ouvrir le dialler and enter *#*#1357946#*#*)
     * un code reçu par mail de votre compte Huawei, code à entrer à la commande $ fastboot oem unlock

    L'explication complete se trouve [ici](http://www.modaco.com/forums/topic/377348-how-to-bootloader-unlock-your-honor-5c-official-and-unofficial-methods/)
    
3. Effectuer les lignes de commande expliqué dans [Les étapes en géneral] avec le code que vous avez obtenu par mail




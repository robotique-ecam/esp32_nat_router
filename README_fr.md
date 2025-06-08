# Routeur NAT ESP32 avec support WPA2 Enterprise

Cette version française résume le fonctionnement et la configuration du projet **ESP32 NAT Router**. Ce firmware permet d'utiliser un module ESP32 comme routeur WiFi NAT. Il peut servir de relais de portée pour un réseau existant, offrir un réseau invité avec un SSID différent ou connecter des appareils simples sur un réseau d'entreprise WPA2-Enterprise.

## Performances

Les tests réalisés montrent un débit de plus de 15 Mb/s selon la configuration de l'ESP32 (fréquence CPU, options de compilation...).

## Premier démarrage

Après le flash du firmware, l'ESP32 crée un point d'accès ouvert nommé `ESP32_NAT_Router`. La configuration s'effectue via l'interface web (http://192.168.4.1) ou la console série.

## Interface web de configuration

Depuis l'interface, renseignez les paramètres du réseau amont (section *STA Settings*) puis cliquez sur **Connect**. Une fois l'ESP32 redémarré et connecté, configurez les paramètres du point d'accès (*Soft AP Settings*) et validez avec **Set**. Les réglages sont sauvegardés en mémoire NVS.

Pour des raisons de sécurité il est possible de désactiver l'interface web depuis la console en utilisant la commande suivante :

```bash
nvs_namespace esp32_nat
nvs_set lock str -v 1
```

La réactivation se fait en remettant la valeur à 0.

## Accès aux appareils derrière le routeur

L'ajout d'une redirection de port permet d'accéder aux services d'un appareil du réseau interne. Exemple :

```bash
portmap add TCP 8080 192.168.4.2 80
```

Dans ce cas, le port 8080 de l'ESP32 est redirigé vers le port 80 de l'hôte `192.168.4.2`.

## Interprétation de la LED intégrée

La LED indique l'état de la connexion amont (allumée si connecté) et clignote un nombre de fois correspondant au nombre d'appareils connectés.

## Interface en ligne de commande

La console série permet de modifier l'ensemble des paramètres (SSID, mot de passe, configuration IP, redirections de ports...). Utilisez la commande `help` pour lister les commandes disponibles.

## Flashage des binaires précompilés

Utilisez `esptool.py` pour flasher les binaires disponibles dans le dossier `build` suivant le modèle de votre ESP32. Les commandes détaillées sont décrites dans la documentation originale.

## Compilation du projet

Le projet peut être compilé soit avec l'environnement **ESP-IDF**, soit avec **PlatformIO**. Reportez-vous au README principal pour les étapes détaillées.

## DNS

Une fois connecté à son réseau amont, l'ESP32 transmet automatiquement l'adresse du serveur DNS appris aux clients. Avant cela, l'adresse par défaut est `8.8.8.8`.

## Dépannage

En cas de problème avec la console série (caractères spéciaux, historique...), essayez un autre terminal ou ajustez la configuration de l'UART.


# Stream Delay

Application Windows gratuite qui ajoute un **délai réglable en direct** à un stream OBS → Twitch,
sans redémarrer le stream, sans reconnexion et sans écran noir.

Ce dépôt ne contient que les versions publiées de l'application.

## ⬇️ Télécharger et lancer

**[➡️ Télécharger la dernière version (StreamDelay.exe)](https://github.com/FraZy34/stream-delay-app/releases/latest/download/StreamDelay.exe)**

1. Téléchargez `StreamDelay.exe` (et, si vous voulez vérifier le fichier, `SHA256SUMS.txt` sur la
   page [Releases](https://github.com/FraZy34/stream-delay-app/releases/latest)).
2. Lancez-le. Si Windows affiche « Windows a protégé votre ordinateur » (exe non signé) :
   *Informations complémentaires* → *Exécuter quand même*.
3. Une petite fenêtre s'ouvre et l'interface s'affiche dans le navigateur : un assistant en
   3 étapes vous guide (clé de stream, réglage d'OBS, test).

**Mises à jour :** automatiques depuis la v0.3.0. Au lancement, une fenêtre propose
« Mettre à jour maintenant » quand une nouvelle version sort (jamais pendant un live).
Le fichier est vérifié (empreinte SHA-256) avant d'être installé, et vos réglages sont conservés.

> Configuration requise : Windows 10 ou 11, 64 bits, OBS Studio.

## Comment ça marche

```
OBS ──RTMP──▶ Stream Delay (127.0.0.1:1935) ──tampon sur disque──▶ Twitch
```

OBS envoie à Stream Delay, qui garde le flux dans un tampon et le renvoie à Twitch avec le
décalage voulu, **sans ré-encodage** (aucune perte de qualité, presque aucun CPU).

| Bouton | Ce qui se passe | Ce que voient les spectateurs |
|---|---|---|
| **Armer** (ex. 30 s) | Le tampon se remplit pendant 30 s. Le stream reste en direct. | Rien ne change. |
| **Activer** | Passage instantané en différé, sur une image clé. | Les 30 dernières secondes sont rejouées, puis le stream continue avec 30 s de retard. |
| **Couper** | Retour au direct sur la prochaine image clé (≤ 2 s). | Un saut en avant : les 30 s en attente ne sont jamais diffusées. |

## Réglage d'OBS

- **Paramètres → Stream** : Service **Personnalisé…**, Serveur `rtmp://127.0.0.1:1935/live`,
  Clé `obs` (la vraie clé Twitch se règle dans Stream Delay). Désactivez la « Diffusion améliorée ».
- **Paramètres → Sortie** (Avancé) : intervalle d'images clés **2 s**.

## Sécurité et vie privée

- **Aucune télémétrie.** Connexions sortantes : le serveur Twitch configuré uniquement
  (`*.twitch.tv`, `*.live-video.net`), et GitHub au lancement pour les mises à jour.
- Tout écoute **uniquement sur 127.0.0.1** : rien n'est accessible depuis le réseau.
- La clé de stream est **chiffrée par Windows (DPAPI)** : seul votre compte Windows peut la relire.
- Le tampon est vidé à la fermeture et au lancement.

VPN
###

Quelques services ne sont accessibles qu'à travers le VPN du lolnet; Il peut
donc etre pratique de s'y inscrire.

Rien de bien compliqué si vous suivez la démarche ici. 

Pour ça, il faut que vous ayez déjà un accès à la machine host, question de
pouvoir copier quelques fichiers la bas.

Se connecter depuis une machine linux
=====================================

Créez votre clé en local
------------------------

Alors, il faut que vous copiez quelques trucs en local, sur votre machine. Pour
faire ça, un petit coup de scp::

    scp -r root@lolnet.org:/root/infra .

Ensuite, il faut que vous génériez votre clé, en root::

    sudo su
    cd infra
    ./install.sh -t openvpn

Uploader la clé et la signer
----------------------------

Normalement vous avez une clé qui est écrite sur la sortie standard. vous la
prenez et vous la copiez collez sur le serveur, dans le fichier qui va bien,
j'ai nommé ”/root/2.0/keys/{machinename}.csr”

Pour signer la clé, c'est pas bien compliqué non plus::

    cd /root/2.0/
    source vars
    sign-req {machinename}

Si à cette étape vous avez une erreur, c'est peut etre qu'un enregistrement
vous concernant est déjà présent dans la base de données. Supprimez
l'enregistrement en question dans keys/index.txt et recommencez !


Récupérer la clé en local
-------------------------

::

    scp root@lolnet.org:/root/2.0/keys/{machinename}.crt /tmp/.
    sudo su
    cp /tmp/{machinename}.crt /etc/openvpn/keys/

Et voila !

A plus qu'à redémarrer le service !
-----------------------------------

::

    sudo service openvpn restart


Se connecter avec un téléphone
==============================

La procédure pour se connecter avec un téléphone est quasiment similaire, vous
allez devoir d'abord vous connecter depuis un ordinateur, récupérer les
fichiers qui vont bien en faisant un scp::

    scp -r root@lolnet.org:/root/infra .


D'abord, modifiez les fichiers pour changer l'HOSTNAME qui va etre utilisé et
le PREFIX. Dans mon cas j'ai utilisé un prefixe dans `/tmp`.

Ensuite, il faut que vous génériez votre clé.

    cd infra
    ./install.sh -t openvpn

Le reste de la procédure est assez similaire.

Ensuite, copiez les fichiers sur le téléphone, renommez le .conf en .ovpn et
soyez bien sur de mettre les fichiers .crt et .key au meme niveau que le
fichier .ovpn, puis changez les chemins relatifs.

Ça devrait être tout !

VPN
###

Quelques services ne sont accessibles qu'à travers le VPN du lolnet; Il peut
donc etre pratique de s'y inscrire.

Rien de bien compliqué si vous suivez la démarche ici. Aparemment mr Quatre me
dit qu'il est possible de faire de differentes manières, en voici en tout cas
une qui permet à votre pc de se connecter automatiquement au VPN du lolnet.

Pour ça, il faut que vous ayez déjà un accès à la machine host, question de
pouvoir copier quelques fichiers la bas. Si c'est pas le cas, vous pouvez nous
demander de vous ajouter, c'est une histoire de déposer une clé publique
quelque part, rien de bien sorcier.

Créez votre clé en local
========================

Alors, il faut que vous copiez quelques trucs en local, sur votre machine. Pour
faire ça, un petit coup de scp::

    scp -r root@lolnet.org:/root/infra .

Ensuite, il faut que vous génériez votre clé, en root::

    sudo su
    cd infra
    ./install.sh -t openvpn

Uploader la clé et la signer
============================

Normalement vous avez une clé qui est écrite sur la sortie standard. vous la
prenez et vous la copiez collez sur le serveur, dans le fichier qui va bien,
j'ai nommé ”/root/2.0/keys/{machinename}.csr”

Pour signer la clé, c'est pas bien compliqué non plus::

    cd /root/2.0/
    source vars
    sign_req {machinename}

Si à cette étape vous avez une erreur, c'est peut etre qu'un enregistrement
vous concernant est déjà présent dans la base de données. Supprimez
l'enregistrement en question dans keys/index.txt et recommencez !

S'attribuer une IP
==================

XXX

Récupérer la clé en local
=========================

::

    scp root@lolnet.org:/root/2.0/keys/{machinename}.crt /tmp/.
    sudo su
    cp /tmp/{machinename}.crt /etc/openvpn/keys/

Et voila !

A plus qu'à redémarrer le service !
===================================

::

    sudo service openvpn restart



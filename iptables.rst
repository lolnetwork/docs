Iptables
########

Un des trucs dont je ne me rapelles jamais, c'est comment fonctionne iptables.
Dans notre cas, le truc le plus important à se souvenir, c'est comment ajouter
des regles de foward, il me semble, mais ce document peut servir à d'autres
choses si besoin.

Par exemple, je viens de créer une nouvelle VM et je veux forwarder un port de
la machine hote vers cette machine specifique. Voici comment faire::

  iptables -A PREROUTING -t nat -i br0 -p tcp --dport 12345 -j DNAT --to 172.19.2.120:22
  iptables -A FORWARD -p tcp -d 172.19.2.120 --dport 22 -j ACCEPT

Et hop, gratos parce que je suis sympa, je vous mets comment configurer son
.sshconfig qui va bien::

  Host nom
      User alexis
      HostName hack.lolnet.org
      Port 12345

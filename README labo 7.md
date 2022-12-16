labo 7 

curious

Le programme va copier tout le contenu des répertoires /homme dans le fichier "imjustcurious"

on peux s'en protéger avec la commande "sudo chmod 751 [user] -R". Seul l'utilisateur pourra accéder à son
dossier. le programme curious ne pourra donc plus copier les home des autres user. 

fb

c'est une fork bomb qui va excécuté plein de processus.
on voit grace à ghidra et virustotal que celui-ci est une bombe fork.

on peut s'en protéger en modifiant le fichier /etc/security/limits.conf en limitant le nombre de 
processus simultané à 500.

	ulimit -u
	watch ps -u [user]

shining

le programme va écrire en boucle à l'infinit dans un fichier jusqu'à saturer le disque dur de celui-ci. 

ce fichier peux faire plus de 3 Gb en seulement quelque seconde. Si on le laisse tourner il peux se
remplir jusqu'à saturé le disque. 

on peut s'en protéger en allant dans /etc/security/limits.conf et en utilisant fsize

	ulimit -f

udp_server

si on lance le programme en user normal, il fonctionne normalement. mais si on l'excécute en root, 
il fera une action plus grave.

Pour s'en protéger, rien de plus simple, éviter d'utiliser le compte root. (surtout si le programme
demande les droits.

jailling

on va d'abord créé un utilisateur avec sudo adduser [user]

on va ensuite se rendre dans "ssh_config"
 
	sudo nano /etc/ssh/sshd_config

ou l'on va ajouté deux ligne

	match User [user]
	ChrootDirectory /home/[user]

pour une session interactive, cela nécessite au moins un shell, et des noeuds /dev
basique tels que "null", "zero", "stdin", "stdout", "stderr" et "tty"

il faut donc créé les fichiers /dev comme suit en utilisant la commande mknod. Dans la commande, l'option -m est utilisée pour spécifier les bits de permissions du fichier, c signifie fichier de caractère
et les deux nombres sont des nombres majeurs et mineurs que des fichiers pointent. 

	mkdir -p /home/[user]/dev

	cd /home/[user]/dev/

	mknod -m 666 null c 1 3
	mknod -m 666 tty c 5 0
	mknod -m 666 zero c 1 5
	mknod -m 666 random c 1 8

ensuite, il faut mettre la permission appropriée sur la chroot jail. 

	chown root:root /home/[user]
	chmod 0755 /home[user]
	ls -ld /home/[user]

il faut maintenant configurer l'interpéteur de commande
en créant d'abord un répertoire bin puis y copiez les fichiers /bin/bash.

	mkdir -p /home/[user]/bin
	cp -v /bin/bash /home/[user]/bin/

ensuite on identifie les fichiers requis libs partagés et on les copies dans le répertoire lib

	ldd /bin/bash -> identifier

créé le dossier /lib64

	mkdir -p /home/[user]/lib64
	sudo mkdir lib

on dépalce ensuite les librairies

	cp -y /lib/x86_64-linux-gnu/libtinfo.so.6  /home/[user]/lib
	cp -y /lib/x86_64-linux-gnu/libc.so.6 /home/[user]/lib
	cp -y /lib64/ld-linux-x86-64.so.2 /home/[user]/lib
[ou utiliser un programme pour le faire]

si l'on veut utilisé ls, il faudra copier les dossiers de librairies adéquate en suivant la manipulation
ci-dessus.

il faut ensuite faire une limite de l'espace disque de la VM avec les quotas.

	mkdir /media/users/

	dd if=/dev/zero of=/media/users/[user].im bs=512K count=200
	mkfs.ext4 /media/users/[user].img

	mkdir /home/[client]
	mount -o loop /media/users/[user].img /home/[user]

	nano /etc/fstab

		/dev/sr0	/media/cdrom0 [voir screen]

	adduser [user]

	chown [user]:[user] /home/[user]


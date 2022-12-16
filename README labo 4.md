Antivirus virustotal 

- echo = pas de virus détecter par virustotal, score de 0/64
* fb = pas de virus détecter par virustotal, score de 0/64
- info = même chose
- write = même chose

Extraction de strings 

- echo = rien de trop bizarre indiquant un potentiel virus
- fb = potentiel bombe fork (fork vu avec les strings)
- info = potentiellement dangeueux avec un rm -rf dans le code
- write = potentiel toctou avec un faccess et fopen 

récupération info sur le fichier avec file

- echo = compiler avec les info de debug, exécutable elf 64-bit
- fb = stripped, exécutable elf 64-bit
- info = exécutable elf 64-bit
- write = exécutable elf 64-bit

identification unique 

- echo = 191b659f908cb94ac183b7b8e95612ed06614119
- fb = e498308b6ce5747dd4c86bb592eae26bfa2566bc
- info = ef0ef82683b979e046b2e0892467528183417d55
- write = 19914efe0537a0d5713e98cece389571365c12e9

ln -s [le chemin à poniter] [nom du lien symbolique] (P.S. celui-ci doit être le même que le document supprimé)


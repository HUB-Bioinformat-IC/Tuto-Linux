# Introduction

Bienvenue dans la salle Bioinfo : voilà quelques astuces pour vous aider
à appréhender les ordis de la salle!

Ce tutoriel est structuré en 3 parties : 
- Se servir du terminal et autres basiques, en introduction très basique à Linux et à la ligne de commande.
- Installer les essentiels, une série de commandes pour installer les basiques sur votre ordi.
- Pratiquer une bioinformatique reproductible, une série d'outils utiles pour améliorer la fiabilité et la reproductibilité de ses analyses/projets/pipelines.


## Qu'est-ce que sont ces ordis?

Caractéristiques des machines :

-   Hardware : 32 cœurs, 128Gb de RAM, GPU Nvidia T100.

-   OS : Ubuntu 20.04 (bientôt 22.10?)

-   Desktop environment : GNOME

    En gros l\'interface utilisateur du système d\'exploitation. Il en
    existe d\'autres, comme KDE.

-   Windowing system par défaut : X (Xorg, X11, d\'autres noms...) sous
    20.04, Wayland sous 22.10

    En gros le système qui gère l\'affichage. Bien technique, mais
    passer de l\'un à l\'autre peut résoudre certains bugs. Plutôt
    rester sur Xorg quand on a un GPU Nvidia (début 2023). On peut
    choisir cela sur l\'écran de démarrage/login (engrenage en bas à
    droite).


# Se servir du terminal et autres basiques

Le terminal est une partie essentielle de Linux.

## Système de fichier

Sur la gauche du curseur est toujours l\'invite de commande :

``` example
nomutilisateur@nomhote:répertoirecourant $
```

On peut écrire des commandes à sa droite. Le répertoire (dossier)
courant (*working directory*) est celui dans lequel s\'éxecute par
défaut la plupart des commandes de manipulations de fichier.

Les répertoires sont organisés comme un arbre, commençant à la racine
(`/`). Un exemple d\'arborescence:

``` example
/
|-- bin
|   |-- file1
|   |-- file2
|-- etc
|   |-- file3
|   |-- directory1
|       |-- file4
|       |-- file5
|-- home
|-- var
```

Le terminal est donc toujours à un point donné du système de fichier. Ce
point est décrit par un chemin. Il en existe deux types :

1.  Le chemin absolu : il commence par `/`. Chaque répertoire/dossier
    est séparé par un `/`
2.  Le chemin relatif : il est relatif à votre dossier actuel. Il peut
    optionnellement commencer par `./`

Il existe des wildcards quand on veut faire référence à des fichiers ou
répertoires, qui permettent d\'englober plusieurs résultats :

-   `*` n\'importe que nombre de n\'importe quel caractère
-   `?` un unique caractère, n\'importe lequel
-   `[]` un unique caractère parmi ceux entre crochets

## Commandes de base

-   `pwd` : (print working directory) affiche le répertoire courant

-   `cd` (change directory), permet de changer de répertoire. Prend un
    argument, le chemin vers la destination. Les arguments sont donnés
    avec un espace :

    ``` example
    [username@hostname ~]$ cd /home/username/Documents/Projet_RNASeq
    ```

    Il existe des synonymes de répertoires pour faciliter le déplacement
    :

    -   `.` : répertoire actuel,
    -   `..` : répertoire parent,
    -   `~` : répertoire \'maison\', situé à
        `/home/your_username/`,
    -   `-` : répertoire précédemment visité.

-   `ls` (list) liste les fichiers et dossiers du répertoire courant
    (par défaut), et celui du chemin passé en argument s\'il y en a un.
    Possède des options, qui prennent la forme de `--nomdeloption` ou
    `-n`, n étant souvent la première lettre de l\'option. On peut
    combiner les options, ex : `ls -alh`. Les plus importantes :

    -   `-l` affichage en liste,

    -   `-a` affiche les fichiers cachés (commencent par un point),

    -   `-h` (si `l`) affiche les tailles de fichiers en format
        humainement compréhensibles.

        ``` example
        # pour afficher les fichiers txt d'un répertoire
        [username@hostname ~]$ cd ls -lh *.txt
        ```

-   `cp <source> <destination>` copie le fichier source au nom
    destination (si fichier ou inexistant) ou dans le répertoire
    destination (si c\'est un répertoire).

-   `mv <source> <destination>` déplace le fichier dans le répertoire
    destination s\'il existe, renomme source en destination s\'il
    n\'existe pas.

-   `mkdir <nom>` crée le répertoire.

-   `rm <source>` supprime le fichier source. Dangereux : il n\'y a pas
    de corbeille.

    -   `-r` supprime dans le répertoire source récursivement,
    -   `-f` force la suppression,
    -   `-i` prompte chaque suppression de fichier.

-   `cat` affiche un fichier dans *stdout* (le terminal), permet de
    concaténer des fichiers

-   `echo` affiche son argument dans *stdout*

-   `find` permet de chercher un fichier récursivement dans un
    répertoire.

    ``` example
    find [chemin_racine_de_la_recherche] -name [nom_du_fichier]
    ```
    
## Privilèges administrateur
Certaines commandes (comme celles qui installent des programmes) demandent d'avoir un compte avec des privilèges administrateurs. Il existe (souvent) un tel compte sur un ordinateur, mais afin de ne pas exécuter chaque commande avec ce privilège (ce qui serait risqué), votre compte vous permet d'exécuter une commande administrateur grace à `sudo`. Il vous faudra alors rentrer votre mot de passe.

``` example
username@hostname:~/Documents$ apt update
Reading package lists... Done
E: List directory /var/lib/apt/lists/partial is missing. - Acquire (13: Permission denied)
username@hostname:~/Documents$ sudo apt update
[sudo] password for username:
Get:1 http://archive.ubuntu.com/ubuntu jammy InRelease [270 kB]
[...]
```


## Combiner des commandes

Il est possible de combiner des commandes via des opérateurs. Ceux-ci
vont rediriger les flux de sorties de commandes vers soit un fichier,
soit une autre commande. Les deux plus utiles sont `>` , la
redirection vers un fichier, et `|` , le pipe. Par exemple
pour concaténer des fichiers dans un nouveau fichier, on peut utiliser
`>` :

``` example
[username@hostname ~]$ cat fichier1.txt fichier2.txt > fichier3.txt
```

Le pipe `|` lui redirige la sortie d\'une commande vers une
autre commande. On peut l\'utiliser pour décompresser un gros fichier et
l\'envoyer immédiatement vers une autre commande sans stocker le gros
fichier, décompressé, en mémoire.

``` example
[username@hostname ~]$ zcat fichier1.fastq.tar.gz | aligner_lambda
```


## Se servir de R

Une introduction à R est disponible dans le livre *Bioinformatics Data
Skills* disponible dans la salle, qui donne une bonne base ainsi que
quelques bonnes pratiques pour travailler avec R. Il ne parle que peu du
`tidyverse` (et de manière datée), un dialecte de R spécialisé dans la
manipulation de données sous forme de dataframe (composé principalement
de `dplyr`, `tidyr`, `readr` et
`tibble`). Pour cela, voir R for Data Science. D\'autres
sources intéressantes sont par example :

-   R for Data Science <https://r4ds.had.co.nz/> (une seconde édition
    semble en préparation pour mi-2023 <https://r4ds.hadley.nz/>),
    généraliste
-   Modern Statistics for Modern Biology
    (<https://web.stanford.edu/class/bios221/book/>) est très utile
    (mais vite ardu) pour comprendre les statistiques derrière les algos
    de bioinfo génomique. Le chapitre 3 est une sympathique introduction
    à la visualisation en R.
-   Mastering Shiny (<https://mastering-shiny.org/>) pour tout ce qui
    est développement shiny.



## Obtenir de l\'aide

### Sur votre système

Pour obtenir de l\'aide sur une commande, les man pages sont une source
possible : `man <command>`, par exemple `man tar`. Mais celles-ci sont
longues (1167 lignes pour la précédente) et complexes. Une alternative
(à installer via `pip install tldr`) est `tldr`, qui montre quelques
options classiques et des exemples utiles.

``` example
$ tldr tar

  tar

  Archiving utility.
  Often combined with a compression method, such as gzip or bzip2.
  More information: https://www.gnu.org/software/tar.

  - [c]reate an archive and write it to a [f]ile:
    tar cf target.tar file1 file2 file3

  - [c]reate a g[z]ipped archive and write it to a [f]ile:
    tar czf target.tar.gz file1 file2 file3

  - [c]reate a g[z]ipped archive from a directory using relative paths:
    tar czf target.tar.gz --directory=path/to/directory .

  - E[x]tract a (compressed) archive [f]ile into the current directory [v]erbosely:
    tar xvf source.tar[.gz|.bz2|.xz]

  - E[x]tract a (compressed) archive [f]ile into the target directory:
    tar xf source.tar[.gz|.bz2|.xz] --directory=path/to/directory

  - [c]reate a compressed archive and write it to a [f]ile, using [a]rchive suffix to determine the compression program:
    tar caf target.tar.xz file1 file2 file3

  - Lis[t] the contents of a tar [f]ile [v]erbosely:
    tar tvf source.tar

  - E[x]tract files matching a pattern from an archive [f]ile:
    tar xf source.tar --wildcards "*.html"
```

### Toujours duckduckgo-er avant de demander de l\'aide

Une des compétence les plus importantes de tout bioinfo est de trouver
des réponses via moteur de recherche. N\'hésitez pas à mettre en
pratique avant de demander de l\'aide!

# Installer les essentiels
## Installer des *packages*

Installer des packages sous Ubuntu se fait via l'intermédiaire du gestionnaire de paquets (*package manager*) apt, soit via l'interface plus moderne `apt` ou la plus stable `apt-get`. Ces *packages manager* installent et mettent à jour les paquets depuis des dépots en ligne, différents selon la distribution Linux utilisée. C'est la manière préférentielle d'installer des logiciels et surtout des langages de programmation (comme R, Rust, LaTeX...), car leurs dépendances sont gérées. 

## Installer des applications

Les snaps (une méthode de distribution des applications propre à Ubuntu) disponibles dans le Ubuntu Software Center permettent d'installer beaucoup d'applications (Zoom, VScode, Inkscape...). Le Flathub permet d'accéder à un choix parfois plus large/plus à jour.

## Avoir un IDE (pour autre chose que R)

Il est parfois nécessaire d\'éditer des fichiers de code d\'un autre
langage que R (oui oui ça arrive) et RStudio montre vite ses limites.
Dans ce cas le choix par défaut, et probablement le meilleur est
d\'utiliser Visual Studio Code de Microsoft. Il est disponible à cette adresse : (<https://code.visualstudio.com/download>), ou via le Ubuntu Software Center. N\'hésitez pas à utiliser les differentes extensions
(*language servers*) de VS code, par exemple Pylance pour
`python`, qui apporte de vraies améliorations dans le workflow et dont il est dur de se passer par la suite.

Si Microsoft vous hérisse, d'autres possibilités existent, restreintes à Python comme PyCharm, ou plus généralistes, comme Sublimetext, neovim, ou encore Emacs.

# S\'assurer de la reproductibilité de ses analyses, projets...

## Se servir de git

Maîtriser `git` est absolument essentiel pour un
bioinformaticien. Les bénéfices qu\'apporte le contrôle de version sont
innombrables, et permettent de sécuriser et tracabiliser le
développement de n\'importe quel pipeline, outil ou analyse. Via les
différents services d\'hébergements en ligne (GitHub, GitLab...), c\'est
aussi une très bonne manière de partager son code et de collaborer sur
le même projet.

Une très bonne introduction à git est disponible dans le livre
*Bioinformatics Data Skills*. Après avoir maîtrisé ces bases, il peut
être cependant un peu pénible de toujours saisir les commandes via le
terminal, et il existe des interfaces à `git` dans la plupart
des éditeurs, RStudio et VScode compris.

## Se servir de conda

Conda est un *package manager* alternatif à celui de votre distribution (`apt` dans notre cas).
Cela permet d\'installer des packages dans des environnements
particuliers, permettant d\'isoler et de reproduire facilement vos
environnement de travail ou d\'exécution. Plusieurs channels (source de
packages) sont disponibles, dont bioconda qui contient un grand nombre
de package utiles en bioinformatique. Via une modification du `\$PATH`,
avec un environnement activé il prend la priorité sur ce qui pourrait
être installé autrement.

La manière optimale d\'utiliser conda n\'est pas de créer un
environnement, puis d\'installer un à un les packages nécessaire, mais
plutôt d\'utiliser le fait que conda peut créer des environnement à
partir de fichier yaml via la commande
`conda env create -f environment.yml`.

Un example de fichier yaml :

``` example
name: ont_hiv
channels:
    - defaults
    - bioconda
    - conda-forge
dependencies:
    - isoquant
    - snakemake==7.22.0
    - rna-seqc
    - bx-python
```

Cette manière de créer des environnement conda s\'intègre parfaitement
avec `git`, ce qui permet d\'augmenter la reproductibilité de
votre projet, en listant les packages nécessaires, voire des versions
spécifiques. Vous remarquerez peut-être que conda est assez lent (je
l\'ai arreté après une heure à essayer de créer l\'environnement ci
dessus) pour créer des environnement. C\'est censé s\'améliorer, mais en
attendant l\'alternative est d\'utiliser mamba
(<https://mamba.readthedocs.io/en/latest/>), un *drop-in replacement*
pour conda.

## Se servir de renv

`renv` est un package R qui permet de gérer les versions des dépendances d'un script R. De la même manière que `conda` il s'agit d'installer les dépendances dans une autre localisation que lorsqu'on installe des packages par défaut. Et comme `conda`, il faut activer l'environnement avant de s'en servir pour installer des packages ou exécuter le script. Le fichier qui recense les dépendances et leurs versions est ici le `renv.lock`, qui est rempli et mise à jour automatiquement par renv. En effet ici il faut installer ses dépendances au fur et à mesure, soit via `renv::install()`, soit via `install.packages()` si `renv` a été activé précédemment (ce qui est automatique si `renv` a été initialisé dans ce projet), puis sauvegarder l'état du projet avec `renv::snapshot()`. La documentation de `renv` (<https://rstudio.github.io/renv/articles/renv.html>) est très complète.

## Se servir de Docker

### En général

La dernière technique utile pour s\'assurer de la reproductibilité de
son travail de bioinformaticien est la *containerization*. Qu\'est
qu\'un container? : *a standard unit of software that packages up code
and all its dependencies*. Il existe plusieurs types d\'outils pour
créer des containers, mais le plus important est **Docker**, ainsi que
son double **podman**. Deux définitions s\'imposent :

*image* : un package exécutable qui inclue tout le nécessaire pour
lancer une application

*container* : l\'instance d\'une image, ce que devient l\'image
lorsqu\'elle est exécutée.

L\'idée est donc de construire des images qui contiendront tous les
fichiers nécessaires à une application, qui seront par la suite
portables sur toute machine, tout système d\'exploitation.

Pour créer une image, il faut partir d\'une image de base, une
distribution Linux, sur laquelle on viendra greffer les dépendances
nécessaires à notre projet.

Les images sont en général décrites sous la forme
`owner/image_name:tag` où tag est la version.

Un exemple très simple de `Dockerfile`, fichier qui permet de
créer des images :

``` example
FROM ubuntu
RUN apt update && \
  apt install -y python3 && \
  apt clean
ENTRYPOINT ["python3", "-c"]
```

Il est difficile de résumer en quelques mots comment utiliser
Docker/podman, et leurs documentations respectives sont détaillées et
accessibles, respectivement : <https://docs.docker.com/get-started/> et
<https://podman.io/getting-started/>.

Il existe pléthore d\'images intéressantes, mais parmi elles le projet
rocker est assez utile dans le cas d\'un projet utilisant R :
<https://rocker-project.org/>.

### Distrobox

Un usage particulier des containers est de les utiliser comme
environnement de test, ou comme environnement permettant de faire
fonctionner des outils récalcitrants. Distrobox
(<https://github.com/89luca89/distrobox>) permet de créer des containers
à la volée, basés sur de nombreuses distribution de Linux. Montant par
défaut votre `$HOME` (`/home/your_username/`), il
n\'isole pas vos fichiers mais en revanche vous évite de casser votre
installation Ubuntu.

# Des ressources pour aller plus loin

[Linux Journey](https://linuxjourney.com/) - ne pas se préoccuper du
choix de distribution (on l\'a fait pour vous).

Bioinformatics Data Skills - normalement disponible dans la salle!

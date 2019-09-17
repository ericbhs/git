.. Git documentation master file, created by
   sphinx-quickstart on Tue Oct 30 11:14:15 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

git cheat sheet
===============

.. contents:: :local:

*index/staging* : ce qui garde la liste de ce que git doit garder en mémoire
(ce qui est dans l'index n'est "commité" si on l'a "staged for commit" avec add)

``git status``
	voir dans que état se trouve le répertoire courant

``git init``
	dit que le répertoire courant doit être considéré comme un repository git

*untracked file* : fichier qui n'est pas suivi par git

``add``
-------

``git add myFile.txt``
	-> ajouter un fichier à l'index git
	
``git add .``
	-> ajoute tous les fichiers du répertoire courant à l'index

``commit`` et ``SHA``
---------------------
	
*commit* : suite de modifs accompagnée d'un message

*SHA* : code unique qui identifie le commit

``git commit -m "texte de mon commit"``
	ajoute un commit avec un message (``-m`` : message)
	
``git commit -a -m "texte de mon commit"``

``git commit -am "texte de mon commit"``
	condensé de :
	
	.. code-block:: bash
	
		git add monFichier.txt
		git commit -m "Ajouté itinéraire dans mon fichier"
		
	L'option ``-a`` demande à Git de mettre à jour les fichiers déjà existants dans son index. 


``git commit --amend -m "Votre nouveau message"``
	modifier le message de votre dernier commit	
	
``log``
-------
	
``git log``
	* voir l'historique des commits
	
	* quitter le log : taper ``q``

``checkout``
------------
	
``git checkout SHADuCommit``
	le code revient à l'état d'un commit précédent
	
``git checkout master``
	revient à la branche principale (commit le plus récent)
	
``revert``
----------
	
``git revert SHADuCommit``
	créer un nouveau commit qui fait l'inverse du précédent

``reset``
---------	

``git reset --hard``
	pour annuler tous les changements qui n'ont pas été commités (retour à l'état du dernier commit)

*remote* : autre ordinateur

**workflow** : on fait des commits en local, et à un moment on les envoit sur le remote

``clone``
---------

Pour récupérer l'URL HTTPS d'un repo github : à droite "HTTPS URL", idem pour l'URL SSH

``git clone https://github.com/.....``
	clone un repo dans notre répertoire courant (local)
    
Define ``origin``
-----------------

``git remote add origin https://gitlab.com/<...>.git``

Undefine ``origin``
-------------------

``git remote rm origin``

Define user name and user email
-------------------------------

This is needed for the first pushes

``git config --global user.name "Eric Bohnes"``

``git config --global user.email "eric.bohnes@e-peas.com"``

``push``
--------	

``git push origin master``

	* envoie les derniers commits sur un remote (github, gitlab...)
	* si on a un seul remote il s'appelle automatiquement ``origin``
	* envoie la branche courante, cad ``master``
	
``pull``
--------

``git pull origin master``
	récupérer un commit qui est en ligne (sur github par exemple) sur notre branche ``master`` et depuis ``origin`` sur le remote

``branch``
----------

``git branch``
	affiche la branche courante (par défaut : ``master``)
	
``git branch mon-test``
	crée une autre branche ``mon-test``
	
``git branch -d ma-branche``
	supprime la branche ``ma-branche``
	
``git checkout mon-test``
	va se placer sur la branche ``mon-test``
	
	si on refait ``git branch`` -> il indique qu'on est maintenant sur ``mon-test``
	
``git checkout -b ma-branche``
	équivalent aux 2 commandes :
	
	.. code-block:: bash
		
		git branch ma-branche
		git checkout ma-branche
        
Push a new local branch to a remote Git repository and track it too
-------------------------------------------------------------------

.. code-block:: bash
    
    # Create a new branch:
    git checkout -b feature_branch_name
    # Edit, add and commit your files.
    # Push your branch to the remote repository:
    git push -u origin feature_branch_name

	
``merge``
---------

``git merge ma-branche``
	
	* fusionne la branche où l'on se trouve (``master par ex``) avec la branche ``ma-branche``
	* toutes les modifications qui on été faites sur ``ma-branche`` sont appliquées à la branche où l'on se trouve (``master`` par ex)
	* on peut évidemment faire l'inverse : ``git merge master`` pour récupérer les modifs du ``master`` sur la branche
	
**Conflits** : peut arriver si 2 personnes ont modifié la même ligne de code dans 2 branches différentes

	* problème lors du merge entre les branches
	* suite à la commande 'merge' git place des balises dans texte du fichier pour identifier les conflits et aider à les résoudre
	* une fois le conflit résolu (fichier modifié) on refait un commit. Si on le fait sans message il ouvre un éditeur texte dans lequel il nous propose de modifier le message par défaut

*Rem* : HEAD = l'endroit où je me situe

``blame``
---------

``git blame mon-fichier.txt``
	
	* sort une liste des modifs sur mon-fichier.txt pour savoir qui a fait quoi (pratique si un collègue a fait une modif qu'on ne comprend pas)
	* la liste contient les débuts des SHA des commits

``show``
--------

``git show SHADuCommit``
	permet de voir le contenu exact du commit dont le SHA est ``SHADuCommit``

Pour ignorer certains fichiers :

	* créer un fichier appelé ``.gitignore``
	* mettre dedans les noms des fichiers à ignorer 
	
**Attention : ne jamais versionner les fichiers de configuration**
	
``stash``
---------
	
``git stash``

	* on met de côté des modifications incomplètes (dans un état "pas terminé", pas assez en tous cas pour en faire un commit)
	* le script reviens à l'état du dernier commit
	* permet faire une modif urgente sans commiter les modifications incomplêtes
	* garde l'historique plus clair, évite les commits superflus
	
``git stash pop``

	* récupère ce qu'on avait mis de côté avec git stash
	* attention : ``pop`` supprime ce qu'on a mis dans stash, on ne peut plus récupérer ce qu'il y a dans le stash
	
``git stash apply``
	pour garder les modifications dans le stash, utiliser ``apply`` à la place de ``pop``
	

	
Récupérer et contribuer à un projet
-----------------------------------

**Github** : faire un "fork" = faire une copie d'un repo et le mettre sur mon compte à moi

Pour voir comment le créateur du code a voulu que se fassent les contributions : cf readme.md > contributing

Pour soumettre notre contribution : "Create pull request" sur github

``diff``
--------

``git diff`` ou ``git diff --cached``
	visualiser la différence entre l'état courant du répertoire et ce qui a été indexé pour le prochain commit
    
``cherri-pick`` : apply the changes of a particular commit
----------------------------------------------------------

``git cherry-pick <SHAducommit>``
    
Move some commits to another branch and force push the result
-------------------------------------------------------------

.. code-block:: bash
    
    git reset --keep HEAD~3         # 3 is for the last 3 commits
    git checkout -t -b newbranch    # Create a new branch and checkout on it
    git cherry-pick <SHA commit 1>  # apply the changes of each commit on the branch
    git cherry-pick <SHA commit 2>
    git cherry-pick <SHA commit 3>
    git push origin +master         # force push master on origin (might need to unprotect the branch on remote)
    git push origin newbranch       # push newbranch on origin
	
Show a list of all tracked files
--------------------------------

.. code-block:: bash

	git ls-tree --full-tree -r --name-only HEAD
	
Remove all tracked files from the index
---------------------------------------

.. code-block:: bash

	git rm -r --cached .
	

* ``rm`` is the remove command
* ``-r`` will allow recursive removal
* ``–cached`` will only remove files from the index. Your files will still be there.
* The ``.`` indicates that all files will be untracked. You can untrack a specific file with git rm --cached foo.txt (thanks @amadeann).

The ``rm`` command can be unforgiving. If you wish to try what it does beforehand, add the ``-n`` or ``--dry-run`` flag to test things out.

Remove Windows credentials
--------------------------

Start Menu > Credential Manager > Windows Credentials > ... > Remove

Move commits to another branch
------------------------------

from...

.. code-block:: bash
	
	master A - B - C - D - E
	
to...

.. code-block:: bash
					
	newbranch      C - D - E
	              /
	master   A - B
	
do...

.. code-block:: bash

	git checkout existingbranch # checkout existingbranch
	git merge master            # merge with master to "save" the 3 last commits on existingbranch
	git checkout master         # checkout to master
	git reset --hard HEAD~3     # Go back 3 commits. You *will* lose uncommitted work.
	                            # alternative : git reset --hard <commit SHA>
	git checkout existingbranch

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

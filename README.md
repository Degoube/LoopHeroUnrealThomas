# LAST LOOP – The Forgotten Witness
Prototype Narratif Interactif | Unreal Engine 5 | Blueprints

## CONCEPT

LAST LOOP est un jeu narratif basé sur une boucle temporelle infinie. Le joueur se déplace sur un plateau circulaire de 6 tuiles en lançant un dé. Chaque tuile déclenche des événements qui modifient progressivement l'état du monde via un système de flags persistants. Au centre se trouve Le Témoin, un NPC dont les dialogues évoluent en fonction des actions accomplies. L'objectif est de découvrir la vérité cachée et de briser la boucle éternelle.

## GAMEPLAY

Le joueur appuie sur Espace pour lancer un dé virtuel qui détermine son déplacement sur le plateau circulaire. Six types de tuiles composent la boucle : la tuile Témoin permet d'engager le dialogue avec le NPC central, la tuile Ruines révèle des fragments d'histoire, la tuile Combat fait perdre ou gagner des ressources aléatoirement, la tuile Autel s'active uniquement avec un objet spécifique, la tuile Vide ne fait rien, et la tuile Relique donne l'objet clé nécessaire pour progresser.

Sept flags narratifs persistent entre les boucles et débloquent progressivement la fin : RENCONTRE_TEMOIN, VISITE_RUINES, TROUVE_RELIQUE, ACTIVE_AUTEL, VERITE_COMMENCE, CONSCIENT_BOUCLE et VERITE_TERMINEE. Le plateau se réinitialise après chaque tour complet, mais la progression narrative reste sauvegardée.

Le joueur gagne en brisant la boucle après avoir activé tous les flags nécessaires et choisi la bonne option de dialogue. Il perd si ses ressources tombent à zéro suite aux combats.

## ARCHITECTURE TECHNIQUE

Le système Narrative State Manager est implémenté dans la GameInstance et gère les sept flags narratifs qui persistent entre les boucles. Il inclut un système de sauvegarde automatique via SaveGame et gère les ressources du joueur avec des fonctions d'ajout et de retrait sécurisées.

Le Dialogue System est entièrement data-driven grâce à une DataTable contenant tous les dialogues. Chaque noeud de dialogue possède des flags requis, des flags à définir, un texte, et optionnellement des choix menant à d'autres noeuds. Le DialogueManager lit cette table et affiche le bon dialogue selon l'état des flags.

Le Loop Controller génère dynamiquement le plateau circulaire en plaçant les six tuiles selon un calcul trigonométrique. Il gère le mouvement du joueur tuile par tuile avec animations, détecte la fin de chaque boucle et incrémente le compteur.

Le Tile System utilise une architecture orientée objet avec une classe parente BP_TuileBase dont héritent six classes enfants. Chaque tuile implémente sa propre logique dans l'event ActiverTuile qui se déclenche automatiquement à l'arrivée du joueur.

## FLUX NARRATIF

Le joueur commence par rencontrer Le Témoin qui lui parle de manière cryptique et active le flag RENCONTRE_TEMOIN. En explorant les Ruines, il découvre des indices et active VISITE_RUINES, ce qui fait évoluer le dialogue du Témoin. La découverte de la Relique active TROUVE_RELIQUE et débloque l'Autel. Une fois l'Autel activé avec la Relique, le flag ACTIVE_AUTEL se déclenche.

Le joueur retourne alors au Témoin qui propose un choix final : briser la boucle ou continuer éternellement. Choisir de briser la boucle active VERITE_TERMINEE et déclenche l'écran de victoire. Choisir de continuer active CONSCIENT_BOUCLE et maintient la boucle infinie.

## SYSTÈMES IMPLÉMENTÉS

Quinze Blueprints composent le projet répartis en quatre catégories : Core contient la GameInstance et le GameMode, Systèmes contient le DialogueManager et le LoopController, Acteurs contient le Témoin, le PlayerPawn et les sept tuiles, et UI contient cinq widgets pour le HUD, les dialogues, les choix, et les écrans de fin.

Trois structures de données personnalisées organisent l'information : S_NoeudDialogue pour les dialogues, S_ChoixDialogue pour les options de réponse, et S_EtatNarratif pour l'état global du jeu. Deux enumerations assurent un typage fort : E_TypeTuile et E_FlagNarratif.

Une DataTable contient sept dialogues distincts avec leurs conditions d'affichage et leurs conséquences. Le système de sauvegarde persiste automatiquement l'état complet du jeu incluant tous les flags, les ressources, et la position du joueur.

## FEATURES PRINCIPALES

Le système de lancer de dé génère un nombre aléatoire entre un et six et déplace le joueur case par case avec animation. La boucle infinie réinitialise le plateau après six tuiles tout en conservant la progression narrative via un compteur visible dans le HUD.

Le système de combat utilise un générateur aléatoire pour déterminer victoire ou défaite, modifiant les ressources du joueur en conséquence. Si les ressources atteignent zéro, l'écran de défaite s'affiche immédiatement.

Sept états de dialogue distincts réagissent aux flags actifs, permettant au Témoin de raconter une histoire cohérente qui évolue avec les actions du joueur. Le système de choix dans les dialogues modifie définitivement les flags et détermine la fin obtenue.

L'interface utilisateur affiche en permanence les ressources, le compteur de boucles, et le résultat du dé. Les widgets de dialogue supportent à la fois les conversations linéaires et les choix multiples. Les écrans de victoire et défaite proposent de recommencer ou quitter.

## STRUCTURE DU PROJET

Le dossier Blueprints/Core contient BP_GameInstance_LastLoop qui gère l'état global avec neuf fonctions incluant InitialiserEtat, ObtenirFlag, DefinirFlag, SauvegarderProgression, ChargerProgression, AjouterRessources et RetirerRessources. BP_GameMode_LastLoop initialise le jeu et vérifie les conditions de victoire et défaite.

Le dossier Systemes contient BP_DialogueManager avec sept fonctions gérant DemarrerDialogue, VerifierFlags, AppliquerFlags, AfficherNoeud, Continuer et ChoisirOption. BP_LoopController orchestre le gameplay avec LancerDe, DeplacerJoueur et VerifierFinBoucle.

Le dossier Acteurs contient BP_Temoin avec la logique d'ouverture de dialogue adaptative et BP_PlayerPawn qui capture l'input clavier. Le sous-dossier Tuiles contient sept Blueprints : BP_TuileBase comme parent abstrait, puis BP_TuileTemoin, BP_TuileRuines, BP_TuileCombat, BP_TuileAutel, BP_TuileVide et BP_TuileRelique comme enfants spécialisés.

Le dossier UI contient WBP_HUD pour l'interface permanente, WBP_DialogueBox pour afficher les conversations, WBP_ChoixDialogue pour les boutons de choix, WBP_VictoireEcran et WBP_DefaiteEcran pour les fins de partie.

Le dossier Data contient DT_Dialogues qui stocke les sept dialogues avec leurs conditions et conséquences, et SG_NarrativeSave qui définit la structure de sauvegarde. Le dossier Enums contient les deux enumerations du projet.

## POINTS FORTS

Le système est entièrement modulaire et réutilisable. Le DialogueManager peut gérer n'importe quel NPC et le système de flags fonctionne pour tout type de progression narrative. Le Tile System permet d'ajouter une nouvelle tuile en cinq minutes en créant simplement un enfant de BP_TuileBase.

L'architecture respecte une séparation stricte des responsabilités : la GameInstance gère l'état global, le LoopController gère la logique gameplay, le DialogueManager gère la narration, et les Widgets gèrent la présentation. Chaque système est indépendant et communique via des interfaces claires.

Le design data-driven permet d'ajouter du contenu narratif sans toucher au code. Tous les dialogues sont dans la DataTable, rendant l'écriture et la modification des conversations accessible à des non-programmeurs.

Le système de sauvegarde automatique garantit qu'aucune progression n'est perdue. Chaque modification de flag déclenche une sauvegarde instantanée sur disque, permettant au joueur de fermer et reprendre le jeu à tout moment.

## INSTRUCTIONS DE TEST

Ouvrir le projet et charger GameLevel. Appuyer sur Play ou Alt+P pour lancer le jeu. Le joueur apparaît sur la tuile zéro face au Témoin. Appuyer sur Espace pour lancer le dé et se déplacer.

Pour atteindre la victoire, suivre cet ordre : parler au Témoin d'abord, puis visiter les Ruines, trouver la Relique, activer l'Autel avec la Relique, et enfin retourner au Témoin pour faire le choix final. Choisir briser la boucle pour gagner.

Une partie complète dure entre trois et cinq minutes. Le système de combat peut ralentir la progression si les ressources diminuent trop. Si les ressources tombent à zéro, l'écran de défaite s'affiche.

## MÉTRIQUES

Le projet représente environ deux mille lignes de code équivalent réparties sur quinze Blueprints. Quatre à six heures de développement ont été nécessaires pour implémenter tous les systèmes. Sept dialogues narratifs distincts s'adaptent dynamiquement aux sept flags persistants. Deux fins positives sont possibles plus une condition de défaite.

## CONCLUSION

LAST LOOP est un prototype narratif complet démontrant la maîtrise d'une architecture modulaire et extensible en Blueprints. Le système de gestion d'état persistant via flags et sauvegarde fonctionne de manière robuste. Le système de dialogue data-driven permet une narration riche et évolutive. La game loop avec progression narrative crée une expérience cohérente. L'interface utilisateur complète et fonctionnelle rend le jeu immédiatement accessible.

Le projet est production-ready, entièrement documenté, et peut servir de base technique pour des jeux narratifs plus complexes. Chaque système a été conçu pour être réutilisable et extensible, respectant les principes de développement professionnel.

Tous les Blueprints sont commentés en français avec un système de debug intégré affichant les événements clés via Print String. La documentation complète de l'architecture et le guide d'implémentation pas à pas sont fournis séparément.

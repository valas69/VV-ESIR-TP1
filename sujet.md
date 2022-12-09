# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

Question 1 : Ariane 5, vol 501 en fumée

https://horustest.io/blog/les-10-bugs-informatiques-les-plus-couteux-de-l-histoire/
Cet article raconte l’explosion d’une fusée quelques secondes au démarrage suite à un bug logiciel.
La fusée Ariane 5 décolle et atteint une vitesse très importante au bout de quelques secondes.
Un dépassement d’entier dans les registres mémoires survient suite aux fortes accélérations produites par la fusée.
Ainsi la fusée n’est plus contrôlable et le système de guidage principal est hors service.
La fusée crash inéluctablement au bout de 36,7 secondes.
Ce bug local, n’affecte que la NASA, mais annule une exploration dans l’espace, et coûte 370 millions de dollars à l’entreprise.
Tester le scénario réel aurait certainement permis d’éviter cette catastrophe technique.
Les testeurs se seraient rendus compte que la capacité mémoire aurait été dépassée lors du décollage réel et auraient donc pu prévoir une mémoire plus importante.

Question 2 : Expliquer un bug et sa solution

Nous allons étudier le bug 796 nommé “SetUniqueList.createSetBasedOnList”.
Il s’agit ici d’un bug global car il peut atteindre n’importe quel programmeur qui code en Java et qui utilise la collection concernée.
Le bug intervient lorsque l’on souhaite appeler la méthode “SetUniqueList.createSetBasedOnList”.
La méthode est censée ajouter des éléments d’une liste à la valeur de retour mais ne le fait pas.
En effet, comme nous pouvons le voir sur la capture ci-dessous.
La commande subSet.addAll a été supprimée malencontreusement suite à un push sur github.

Ce qui provoque un dysfonctionnement de la méthode.
Pour résoudre ce problème, le code a été modifié à nouveau avec l’ajout de cette méthode supprimée, ainsi que quelques tests supplémentaires.

Voici les changements qu’ont effectué les contributeurs afin d’assurer que le bug ne se reproduise pas:
```java
try{
    subSet=set.getClass().newInstance();
}
catch (final InstantiationException le) {
    subSet = new HashSet‹>0);
        }
catch (final IllegalAccessException iae){
        subSet=set.getClass().getDeclaredConstructor(set.getClass()).newInstance(set);
        }
catch (final InstantiationException | IllegalAccessException | InvocationTargetException | NoSuchlethodException ie) {
subSet = new HashSet<>0);
        }
```

Une méthode catch concernant InstantiationException a été ajoutée afin de renvoyer un message d’erreur en cas de mauvaise instanciation d’une variable.

Question 3 : Résumé des expériences de Netflix

La plateforme Netflix utilise la méthode du “Chaos engineering”. C’est-à-dire que l’entreprise implémente ses mises à jour sur des systèmes distribués.
Étant donné que la plateforme Netflix doit être utilisable en permanence pour ses utilisateurs, l’entreprise n’a pas d’autre choix que d’implémenter directement ses mises à jour sur la plateforme et la proposer aux utilisateurs.
De plus, Netflix est géographiquement distribué dans le monde entier, donc si un serveur dysfonctionne, les utilisateurs ne seront pas impactés car ils seront redirigés automatiquement vers un autre serveur.
Pour Netflix, cette méthode fonctionne plutôt bien car les mises à jour sont déployées, fonctionnent et ne causent pas de bug.
D’autres multinationales utilisent également cette méthode, entre autres Google, Amazon, Facebook et Microsoft.

Question 4 : Avantages des spécifications formelles de Web Assembly

Webassembly permet de compiler d’autres langages dans un format que nos navigateurs peuvent comprendre, c’est en effet grâce aux spécifications formelles. Cela permet d’avoir des performances presque natives.
Malgré cela, nous ne sommes pas à l’abri d'éventuels bug, il faut donc tout de même tester et voir si la compilation est bien fonctionnelle afin d’être sur un produit stable.

Question 5 : Mechanized specification

La spécification mécanisée a de nombreux avantages :
la préservation du globe oculaire
le fait de devoir être explicite sur le comportement du code
avoir une spécification sous une forme propice aux preuves
Cette spécification mécanisée aide donc la spécification d’origine.

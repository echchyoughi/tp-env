# Réponses TP 6

**Q1.** GITHUB_ENV est un fichier lu par le runner *à la fin* de la step. Les
variables écrites dedans sont injectées dans l'environnement des steps
suivantes, pas dans celle qui les a définies (son environnement est déjà figé).

**Q2.** `$APP_NAME` est vide dans verify-env. Chaque job s'exécute sur un runner
distinct, donc la portée de GITHUB_ENV se limite aux steps suivantes du même job.

**Q3.** `>` écrase le fichier à chaque écriture : seul le dernier output écrit
survit, le premier est perdu. Il faut `>>` pour ajouter à la suite.

**Q4.** GITHUB_ENV est un fichier local au runner, donc invisible des autres
jobs (autres machines). GITHUB_OUTPUT, lui, est remonté à GitHub via `outputs:`
du job et exposé aux autres jobs par le contexte `needs`.

**Q5.** Sans `needs: build`, deploy démarre en parallèle de build, sans attendre
sa fin. `needs.build.outputs.full_version` est alors vide, et l'ordre
d'exécution n'est plus garanti.

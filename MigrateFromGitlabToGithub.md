# MigrateFromGitlabToGithub

Ce document décrit les étapes à réaliser pour migrer un repository Gitlab vers un nouveau repository Github.

## Pré-requis

1. Avoir un compte Gitlab et un compte Github.
2. Avoir un repository Gitlab à migrer.

## Migration

1. Créez un nouveau repository sur GitHub via l'interface utilisateur web. Notez l'URL du nouveau repository GitHub.
2. En local sur votre poste, faire une copie de votre répertoire contenant le projet à migrer :

```bash
cp -r projectToMigrate projectToMigrateGithub
```

3. Ajoutez le nouveau repository GitHub comme remote dans votre repository local :

```bash
git remote add github https://github.com/votre_login/votre_repository.git
```

4. Poussez tout le contenu du repository (toutes les branches et tous les tags) vers GitHub :

```bash
git push --mirror github
```

Cette étape peut provoquer l'erreur suivante :
```bash
remote: Support for password authentication was removed on August 13, 2021.
remote: Please see https://docs.github.com/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls for information on currently recommended modes of authentication.
fatal: Échec d'authentification pour 'https://github.com/votre_login/votre_repository.git/'
```

Voir [ici](#error-on-git-push) comment gérer cette erreur.

5. Delete origin :
```bash
git remote remove origin
```

6. Rename github remote as origin :
```bash
git remote rename github origin
```

7. Do git push :
```bash
git push
```

8. Delete old local repository :
```bash
rm -rf projectToMigrate
```

9. Rename migrated local repository :
```bash
mv projectToMigrateGithub projectToMigrate
```

## Error on git push

Pour gérer l'erreur suite à la commande "git push --mirror github", procéder comme suit :

1. Connectez-vous à votre compte GitHub.
2. Allez dans Settings (Paramètres) en cliquant sur votre avatar en haut à droite, puis sur Developer settings (Paramètres de développeur) dans le menu de gauche.
3. Sélectionnez Personal access tokens (Tokens d'accès personnel), puis cliquez sur Generate new token (Générer un nouveau token).
4. Donnez un nom à votre token, sélectionnez les scopes nécessaires (par exemple, repo pour accéder aux repositories privés et publics).
5. Cliquez sur Generate token en bas de la page.
6. Copiez le token généré et conservez-le en lieu sûr (vous ne pourrez plus le voir une fois la page fermée).
7. Retournez à votre terminal en local dans le dossier projectToMigrateGithub
8. Ajoutez ou mettez à jour le remote github en incluant le PAT dans l'URL :
```bash
git remote set-url github https://<your_token>@github.com/votre_login/votre_repository.git
```
Remplacez <your_token> par le token que vous avez généré.

9. Maintenant, vous pouvez réessayer de pousser le repository :
```bash
git push --mirror github
```

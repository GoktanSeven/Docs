# MigrateFromGitlabToGithub

This document describes the steps to migrate a GitLab repository to a new GitHub repository.

## Prerequisites

1. Have a GitLab account and a GitHub account.
2. Have a GitLab repository to migrate.

## Migration

1. Create a new repository on GitHub via the web user interface. Note the URL of the new GitHub repository.
2. Locally on your machine, make a copy of your directory containing the project to migrate:

```bash
cp -r projectToMigrate projectToMigrateGithub
```

3. Add the new GitHub repository as a remote in your local repository:

```bash
git remote add github https://github.com/your_login/your_repository.git
```

4. Push all the contents of the repository (all branches and all tags) to GitHub:

```bash
git push --mirror github
```

This step may trigger the following error:
```bash
remote: Support for password authentication was removed on August 13, 2021.
remote: Please see https://docs.github.com/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls for information on currently recommended modes of authentication.
fatal: Authentication failed for 'https://github.com/your_login/your_repository.git/'
```

See [here](#error-on-git-push) how to handle this error.

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

To handle the error after the command "git push --mirror github", proceed as follows:

1. Log in to your GitHub account.
2. Go to Settings by clicking on your avatar in the top right corner, then select Developer settings from the left-hand menu.
3. Select Personal access tokens, then click on Generate new token.
4. Give your token a name, select the necessary scopes (for example, repo to access both private and public repositories).
5. Click on Generate token at the bottom of the page.
6. Copy the generated token and keep it safe (you won't be able to see it again once the page is closed).
7. Return to your local terminal in the projectToMigrateGithub directory.
8. Add or update the GitHub remote by including the PAT in the URL:
```bash
git remote set-url github https://<your_token>@github.com/your_login/your_repository.git
```
Replace <your_token> with the token you generated.

9. Now, you can try pushing the repository again:
```bash
git push --mirror github
```

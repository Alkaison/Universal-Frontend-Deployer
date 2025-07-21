# ⚙️ Setup Instructions

Let's begin installing this modular github actions workflows setup into your existing frontend project.

## Initial Step

1. Copy all the files from [workflows](./workflows/ "Workflows") folder of this repository and add them into the `.github/workflows/` directory of your frontend project.

## Step 1 - Main Controller

In `.github/workflows/deploy.yml`:

1. Set the branch to trigger the workflow `(Line: 7)`.
1. Add `ssh_host` of your server at `(Line: 13, 26)`.
1. Add `ssh_user` of your server or create a new user `deploy` only for this workflow `(Line: 14, 27)`.
1. Add `project_path` of your frontend project repo `(Line: 15, 28)`.
1. Add `SSH_PASSWORD` and `SLACK_WEBHOOK_URL` into your GitHub repository actions secret.
1. Set the `url` of the deployed frontend for health check `(Line: 23)`.

## Step 2 - Pull & Build

In `.github/workflows/pull_and_build.yml`:

1. Set the branch to pull the code from `(Line: 30)`.
1. Optional: Update the build folder name in-case it's different for your application `(Line: 33)`.
1. Optional: Update web server reload command or PM2 restart command `(Line: 40)`.

## Step 3 - Rollback

In `.github/workflows/rollback.yml`:

1. Optional: Update the rollback commands for your application `(Line: 30 - 41)`.

> No need to change in rollback.yml file if your project is made using Vite - react.

## 💬 Need Help?

Contact [Alkaison](https://github.com/Alkaison) for help in setup or further improvements as per your project.

# Guide to Deploy Laravel on Render

## Prerequisites
- This guide expects you to have a [laravel app](https://laravel.com/docs/12.x/installation) on a [github repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository).
- Git must be installed and configured on your PC, [here's how](https://docs.github.com/en/get-started/git-basics/set-up-git#setting-up-git).

## [Stage](https://github.com/git-guides/git-add), [Commit](https://github.com/git-guides/git-commit) and [push](https://docs.github.com/en/get-started/using-git/pushing-commits-to-a-remote-repository) the files below to the root (uppermost folder) of your laravel app github repository.
- [.env.example](https://github.com/SP4CEBARsystems/Deploy-Laravel-on-Render/blob/main/.env.example)
- [Dockerfile](https://github.com/SP4CEBARsystems/Deploy-Laravel-on-Render/blob/main/Dockerfile)
- [apache/000-default.conf](https://github.com/SP4CEBARsystems/Deploy-Laravel-on-Render/tree/main/apache)
  
_Note: if one of these files already exists on your repository then you should replace them_

## Create a `Postgres` database
### 1. Go to your [Render Dashboard](https://dashboard.render.com/)
### 2. Click `Add new`
### 3. Click `Postgres`
### 4. Fill in the folowing details
| Field | Value | Notes |
|---|---|---|
| Name | `my-great-database` | Or anything you like |
| Database | `mydatabase` | Has to be this |
| User | `root` | Has to be this |
| Region | `Frankfurt (EU Central)` | Or where ever is nearest to your users |
| PostgreSQL Version | `16` | Is what I used, but you may use another one |
| Plan Options | `Free` | Only if you like free |
### 5. Click `Create Database`
### 6. Create a text document (or note) on your PC or phone and copy the text below into it. Don't close your database web page.
```.env
APP_URL=https://your-site-name.onrender.com/
DB_HOST=
DB_PORT=5432
DB_PASSWORD=
```
_note: the remaining environment variables can be found in the [.env.example file](https://github.com/SP4CEBARsystems/Deploy-Laravel-on-Render/blob/main/.env.example) I provided_
### 7. Copy the details below from your new database's page next to the same name in your text document
| On Render | In Document |
|---|---|
| Hostname | DB_HOST=`your_hostname_here` |
| Port | DB_PORT=`your_port_number_here` |
| Password | DB_PASSWORD=`your_password_here` |
  
_note: It it important to never leak your credentials by, for example, committing them to a public github repository, in this guide we will avoid that._  
_note: If you want, you may now close the web page as we got all the credentials we need._
## Create a `Web Service`
### 1. Go to your [Render Dashboard](https://dashboard.render.com/)
### 2. Click `Add new`
### 3. Click `Web Service`
### 4. Choose the github repository you want to deploy
### 5. Fill in the folowing details
| Field | Value | Notes |
|---|---|---|
| Name | `my-great-app` | Or anything you like, be sure to copy it as this will be part of the url |
| Language | `Docker` | Has to be this |
| Instance Type | `Free` | Only if you like free |
| Dockerfile Path | `./Dockerfile` | Has to be this |
### 6. Add environment Variables
1. CLick `Add from .env`
2. Paste the text from the text document you filled in
3. Click `Add variables`
4. Modify the `APP_URL` to replace the `your-site-name` with the site name you copied from the `Name` field on top of this page.
### 7. Click `Deploy Web Service`

## Debug
### 1. Wait for your web service to deploy
### 2. What if an error occurs?
#### 1. Ask ChatGPT: tell it that this is a laravel site deployed on Render.com, and send the error to it, as well as the [.env.example](https://github.com/SP4CEBARsystems/Deploy-Laravel-on-Render/blob/main/.env.example), [Dockerfile](https://github.com/SP4CEBARsystems/Deploy-Laravel-on-Render/blob/main/Dockerfile), and [apache/000-default.conf](https://github.com/SP4CEBARsystems/Deploy-Laravel-on-Render/tree/main/apache) files so it knows exactly what you are doing.
#### 2. Look yourself
- Does the error mention some code you wrote?
  - Is there a mismatch in capital letters, like an import of a file `Test.txt` even though the file is named `test.txt`?
  - Your code may have bugs, below is an explanation of how you can run it yourself
### 3. Run the code yourself
#### 1. Open your laravel project in your favorite code editor (you may use [VScode](https://code.visualstudio.com/)).
#### 2. Run the commands below one by one to build and deploy your app
  ```sh
  composer install
  npm install
  npm run build
  cp .env.example .env
  ```
#### 3. Edit the `.env` file you just generated and add the database credentials from your text document right below line 27 that says `DB_USERNAME=root`.
  - If you instead want to use a database on your own computer then you should follow the instructions below, otherwise continue with step 4
    1. [install XAMMP](https://www.apachefriends.org/download.html) and paste the databsase credentials below in your `.env` file
      ```.env
      DB_CONNECTION=mysql
      DB_HOST=127.0.0.1
      DB_PORT=3306
      DB_DATABASE=mydatabase
      DB_USERNAME=root
      DB_PASSWORD=
      ```
    2. open XAMPP
        1. open Apache's `Config` and click the `Apache (httpd.conf)`
        2. search for `DocumentRoot "` and replace both file paths to the `/public` folder of your laravel project
        3. Open MySQL's admin
        4. Click `New` on the top left to create a new schema, name it `mydatabase`
        5. Open Apache's admin
        6. read the [laravel exam guide](https://github.com/SP4CEBARsystems/Deploy-Laravel-on-Render/blob/main/laravel-exam-guide.md) for more details.
#### 4. Continue with running the commands below
  ```sh
  php artisan key:generate
  php artisan migrate:fresh --seed
  php artisan serve
  ```
#### 5. If it says `No connection could be made because the target machine actively refused it` then you probably forgot to turn on your database.

## Need help?
- Ask on [our forums](https://github.com/SP4CEBARsystems/Deploy-Laravel-on-Render/discussions/1).
- Search my [ChatGPT conversation of trying to get it to work](https://chatgpt.com/share/684beaf0-dc14-800c-80de-7ccac783d860) for reference.

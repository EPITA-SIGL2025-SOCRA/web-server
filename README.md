# Web server workshop

This workshop aims students of EPITA (promotion 2025) of the SIGL specialisation in the context of the course of SOCRA.

**Objective**: Expose a minimal web page on internet using the infrastructure provided by teachers.

In this workshop, you'll learn:

- how to configure NGINX
- how to connect to a remote server from your personal computer using SSH protocol

## Step 1: Create your github repository

From now on until the end of the course, you will code on your own github repository.

- Create a new project with the `sotracteur` as name
- Make sure your repository is private
- Once created, add push a README.md file with a title `# Sotracteur` and login of the 2 group members like (replace by your info instead):

```plain
// ./README.md
# Sotracteur

Owner:
- student.name <sutend.name@epita.fr>
- other-sutdent.name <other-student.name@epita.fr>
```

- Add teachers as collaborators:
  - Lucas: [LucasBoisserie](https://github.com/LucasBoisserie)
  - Florent: [ffauchille](https://github.com/ffauchille)

## Step 2: Connect to your remote server

**Objective**: access your remote server from your personal computer.

- Download your SSH key provided by your teachers
- Type the following commands to connect remotely from your terminal:
- Make sure your rights on your private SSH key are not too open (type `$ chmod 400 <path_to_your_ssh_key>` to make sure you have correct rights)

```sh
# From your favourite terminal session

# SSH KEY path can be relative or absolute
# make sure to replace XX by your group number
$ ssh -i <path_to_your_ssh_key> ubuntu@groupXX.socra-sigl.fr

# Accept the prompt message (only happens on first SSH connection)
# You should be connected remotely
```

- Add an `AUTHORS.txt` in sigl's user home with your logins:

```plain
# Inside AUTHORS.txt
* student.name
* other-student.name
```

## Step 3: Deploy a first minimal web application

**Objective**: expose a simple web page accessible by everyone from internet.

### Create a simple `html` page

Let's create a standalone `html` page with a catchy phrase in French for sotracteur (be creative!).

- On your personal computer, in your freshly created `sotrcateur` repository, create a new `index.html` file with your own moto follwing this simple template:

```html
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title>Sotracteur</title>
  </head>
  <body>
    <h1>Sotracteur</h1>
    <p>Mon super slogan attractif!</p>
  </body>
</html>
```

- Make sure your HTML displays correctly on your browser (simply open your html file with your browser)

### Deploy your web page

- Connect to your remote server via ssh (like in the previous step)

- We selected [nginx](https://ubuntu.com/tutorials/install-and-configure-nginx#2-installing-nginx) as a web server to serve your web page.
- It should be already installed, you can verify by typing `command -v nginx` on the remote server
- If `nginx` is not installed, just install it by typing:

```sh
sudo apt update
sudo apt install nginx
```

- Make sure you can see the default `Welcome to nginx!` page from your browser by visiting your address http://groupXX.socra-sigl.fr (replace XX by your group number)

Deploy your index.html file by replacing default's NGINX html file on your remote server:

```sh
# from your local host; make sure it ends with ':'
$ scp -i <path_to_your_ssh_key> <path_to_your_index.html> ubuntu@groupXX.socra-sigl.fr:
```

Connect with ssh to your remote server, and move your `index.html` to default nginx html's folder:

```sh
# see previous step how to connect with ssh to your machine
# Once connected, from your remote server:
$ sudo cp /home/ubuntu/index.html /var/www/html/index.nginx-debian.html
```

You should be all set! You should see your web page at http://groupXX.socra-sigl.fr (replacing XX by your group number)

Congratulation, you just deployed a first minimal version of Sotracteur on internet!

## Challenge 1: Implement load balancing

**Challenge**: Implement load balancing with the following spec:

- When a user requests your applicaiton with the path `/github`, it should proxy the request:
  - once toward Lucas's github page
  - second toward Florent's github page
  - third toward Lucas's and so on...

You can directly update the `nginx` configuration file on the remote server (`/etc/nginx/conf.d/default.conf`).

## Challenge 2: Draw a diagram of the workshop architecture

**Challenge**: draw the full mounty request path, leaving your computer to the machine serving the html file (inclulding what is happening on the machine with the request).

You can use any tool you want, even a sheet of paper!

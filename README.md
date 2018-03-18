This is a collection of code snippets

### Installing Oh-My-Zsh
```
sudo apt-get install zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
chsh -s /bin/zsh
```


### Installing a .deb file via terminal
 ```
 sudo dpkg -i path/to/the/file
 ```
### Install node js
```
sudo apt-get install nodejs-legacy
sudo apt-get install npm
```

### Uninstalling a package
####  This removes  all but config files
```
sudo apt-get remove <package-name>
```
#### This removes everything
```
sudo apt-get purge <package-name>
```

### Getting started with vue.js
#### Using the CDN
```
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.13/dist/vue.js"></script>
```



### Start your first react app
```
npm install create-react-app
create-react-app app-name
cd app-name
npm start
```



### Setup mongodb on ubuntu
```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927

echo "deb http://repo.mongodb.org/apt/ubuntu "$(lsb_release -sc)"/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list

sudo apt-get update

sudo apt-get install -y mongodb-org
```

We have to create a new mongodb systemd service file in the `/lib/systemd/system` directory. Go to that directory and create the new mongodb service file `mongod.service` with nano.

```
sudo nano /etc/systemd/system/mongodb.service
```
Paste the script below
```
    [Unit]
    Description=High-performance, schema-free document-oriented database
    After=network.target

    [Service]
    User=mongodb
    ExecStart=/usr/bin/mongod --quiet --config /etc/mongod.conf

    [Install]
    WantedBy=multi-user.target
```
Now update the systemd service and start mongodb:

```
systemctl daemon-reload
systemctl start mongod
systemctl enable mongod
```


### Setting up nginx
```
sudo apt-get install nginx nginx-full nginx-common
```
then reboot system
Check the firewall and allow nginx HTTP
```
sudo ufw app list
sudo ufw allow 'Nginx HTTP'
sudo ufw status
```
If ufw is inactive enable it
```
sudo ufw enable
```

Whenever you create a new server, just add them into the conf.d directory `/etc/nginx/conf.d/`
```
sudo nano /etc/nginx/conf.d/myWebsite.d
```
Put a specific server config in that file
```
server {
    listen 8080;
    server_name localhost;
    location / {
        root    /path/to/the/folder/;
        index   index.html index.htm;
    }

    #redirect server error pages to the static pages /50x.html
    error_page 500 502 503 504  /50x.html;
    location = /50x.html{
        root html;
    }
}
```

###Setting up rubu on rails
#### Install rvm and ruby first
```
sudo apt-get install software-properties-common
sudo apt-add-repository -y ppa:rael-gc/rvm
sudo apt-get update
sudo apt-get install rvm
rvm install ruby
```
####Install rails using gem 
```
gem install rails
```
####Create an app 
```
rails new <app-name>
```



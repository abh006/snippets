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

### Setting up ruby on rails
#### Install rvm and ruby first
```
sudo apt-get install software-properties-common
sudo apt-add-repository -y ppa:rael-gc/rvm
sudo apt-get update
sudo apt-get install rvm
rvm install ruby
```
#### Install rails using gem 
```
gem install rails
```
#### Create an app 
```
rails new <app-name>
```


### Deploying a python telegram bot in heroku
`https://hackernoon.com/how-to-create-and-deploy-a-telegram-bot-2addd8aec6b4`
this is a very nice article about deployment

navigate to the project folder
```
pip freeze > requirements.txt
```
now create a `Procfile`  ( but for windows it is `Procfile.windows`)
it should contain 
```
web: python my_bot_name.py
```
Also add an __init__.py to the project folder. It can be empty
Push the project to a git repository
Install Heroku CLI
```
npm install heroku -g
```
To verify the installation, try `heroku --version`
Create an account in `heroku.com`
Now follow these steps
```
heroku login
heroku create
git push heroku master
heroku ps:scale web=1
heroku open
```
Now your project runs on heroku server
To check the logs
```
heroku logs --trail
```



### port forwarding using nginx
Create a new config file inside `/etc/nginx/sites-available/`
if the name of th config file is `app`
Then the config file will be
`/etc/nginx/sites-available/app`
```
server{
    listen 80;
    server_name domain.com
    location / {
        proxy_pass "http://0.0.0.0:8080"
    }
}
```
Also create a soft link to `/etc/nginx/sites-enabled/` like:
```
sudo ln /etc/nginx/sites-available/app /etc/nginx/sites-enabled/app
```
Delete any unwanted files inside the `sites-available` and `sites-enabled` folders


If you want to use the forwarded hostname as the proxy header
Add this inside the `location / { .... }`
```
proxy_redirect http://kutt.com/ http://0.0.0.0:8090;
proxy_set_header Host $host;
```

### Backup and Restore postgres database
#### Backup
```
pg_dump <db-name> > output-file.bak
```
If it is from docker container
```
sudo docker container ls    // to get container id
sudo docker exec -it <container-id> pg_dump -U <db-user> <db-name> > bkup.bak
```
#### Restore
Drop the database if it exists and create a new one
```
psql <db-name> < output-file.bak
```
#### It may cause errors like ProgrammingError: permission denied for relation django_migrations
Try this 
```
psql mydatabase -c "GRANT ALL ON ALL TABLES IN SCHEMA public to dbuser;"
psql mydatabase -c "GRANT ALL ON ALL SEQUENCES IN SCHEMA public to dbuser;"
psql mydatabase -c "GRANT ALL ON ALL FUNCTIONS IN SCHEMA public to dbuser;"
```

### Setting up OpenCV in Ubuntu 18.04 ( For C++ )

One of the best documentation for opencv installation goes here: ( It takes several minutes to make and install ).

While installing, keep in mind that, you have to install it globally instead of installing it in any particular folder

https://docs.opencv.org/2.4/doc/tutorials/introduction/linux_install/linux_install.html

After installing that you will be able to include the header file `opencv2/opencv.hpp`

If you are facing any issues in importing them, 
try finding the path in which opencv is installed
if it is in `/usr/local/include/opencv4/`

Then export the CPATH 
```
export CPATH=/usr/local/include/opencv4/
```
Now the issue will be solved



### Installing graphics.h in Ubuntu 18.04 

```
sudo apt-get install build-essential

sudo apt-get install libsdl-image1.2 libsdl-image1.2-dev guile-2.0 \
guile-2.0-dev libsdl1.2debian libart-2.0-dev libaudiofile-dev \
libesd0-dev libdirectfb-dev libdirectfb-extra libfreetype6-dev \
libxext-dev x11proto-xext-dev libfreetype6 libaa1 libaa1-dev \
libslang2-dev libasound2 libasound2-dev
```

Download libgraph file from this link
http://download.savannah.gnu.org/releases/libgraph/libgraph-1.0.2.tar.gz

Extract it and cd into it

```
./configure
make
sudo make install
sudo cp /usr/local/lib/libgraph.* /usr/lib
```

Now you will be able to use graphics.h library

But still might be getting error while using methods like initgraph(), etc.
For that you have to include the header file while compiling also
Like:
```
g++ filename.cpp -lgraph
```

Now you will be able to Use all those methods.
There is a chance for unexpected abortion of the process, while running some programs. If you find any, raise an issue, along with the code and the obtained output :)



### Setup Supervisor and Deploy django server
```
sudo apt-get install supervisor
sudo service supervisor restart
```

Now check where the configuration file is to be added
Open the config file of supervisor at `/etc/supervisor/supervisord.conf`
Look for a line like:
```
[include]
files = /etc/supervisor/conf.d/*.conf
```
It means all the `.conf` files inside that folder will be considered by supervisor.

Now add a new config file in that, for the new server
```
sudo nano /etc/supervisor/conf.d/project-name.conf
```
And in that file 
```
[program:projectname]
command=/home/ubuntu/myprojenv/bin/gunicorn --workers 3 --bind 0.0.0.0:8001 myproject.wsgi
directory=/home/ubuntu/myproject
autostart=true
autorestart=true
stderr_logfile=/var/log/projectname.err.log
stdout_logfile=/var/log/projectname.out.log
```
This will create a new gunicorn process which runs the wsgi on 80001 port

Check the specified log files if required

The `projectname` specified will be used for further usages like starting, stopping or restarting the process.

After creating this file,bring these changes into effect:
```
sudo supervisorctl reread
sudo supervisorctl update
```
For verifying whether everything is fine:
```
ps ax | grep gunicorn
```
There might be several processes running, or go to `0.0.0.0:8001`

You can also try 
```
sudo supervisorctl status myproject
```
All set!!!

Now you can use 
```
sudo supervisorctl start projectname
sudo supervisorctl stop projectname
sudo supervisorctl restart projectname
```
for managing the process

### Installing CARLA
```
sudo apt-get update
sudo apt-get install wget software-properties-common
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key|sudo apt-key add -
sudo apt-add-repository "deb http://apt.llvm.org/xenial/ llvm-toolchain-xenial-7 main"
sudo apt-get update
sudo apt-get install build-essential clang-7 lld-7 g++-7 cmake ninja-build libvulkan1 python python-pip python-dev python3-dev python3-pip libpng16-dev libtiff5-dev libjpeg-dev tzdata sed curl unzip autoconf libtool rsync
pip2 install --user setuptools
pip3 install --user setuptools
```
For Ubuntu 18.04, change `libpng16-dev` to `libpng-dev` from the previous example.

```
sudo update-alternatives --install /usr/bin/clang++ clang++ /usr/lib/llvm-7/bin/clang++ 170
sudo update-alternatives --install /usr/bin/clang clang /usr/lib/llvm-7/bin/clang 170

```
#### Build Unreal Engine
Unreal Engine repositories are set to private. In order to gain access you need to add your GitHub username when you sign up at www.unrealengine.com.
Download and compile Unreal Engine 4.22. Here we will assume you install it at ~/UnrealEngine_4.22, but you can install it anywhere, just replace the path where necessary.
```
git clone --depth=1 -b 4.22 https://github.com/EpicGames/UnrealEngine.git ~/UnrealEngine_4.22
cd ~/UnrealEngine_4.22
./Setup.sh && ./GenerateProjectFiles.sh && make
```
#### Build CARLA
```
git clone https://github.com/carla-simulator/carla
```
Now you need to download the assets package,(note that this package is >3GB)
Optionally you can download aria2 (with `sudo apt-get install aria2`) so the following command will take advantage of it and will run quite faster.
```
./Update.sh
```
For CARLA to find your Unreal Engine's installation folder you need to set the following environment variable
```
export UE4_ROOT=~/UnrealEngine_4.22
```
Now that the environment is set up, you can use make to run different commands and build the different modules
```
make launch     # Compiles the simulator and launches Unreal Engine's Editor.
make PythonAPI  # Compiles the PythonAPI module necessary for running the Python examples.
make package    # Compiles everything and creates a packaged version able to run without UE4 editor.
make help       # Print all available commands.
```
For more info: https://carla.readthedocs.io/en/latest/how_to_build_on_linux/

### Deploying Node JS appication using systemd
**Create a ```systemd``` service file**

```
sudo nano /lib/systemd/system/hello_env.service
```
It should contain the following lines:
```
[Unit]
Description=hello_env.js - making your environment variables rad
Documentation=https://example.com
After=network.target

[Service]
Environment=NODE_PORT=3001
Type=simple
User=ubuntu
ExecStart=/usr/bin/node /home/ubuntu/server.js
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
For more information about what these lines really mean, follow https://nodesource.com/blog/running-your-node-js-app-with-systemd-part-1/

Since we have changed the service files of ```systemd```, we have to reload the daemon.
```
sudo systemctl daemon-reload
```
Now, for starting the service we added, 
```
sudo systemctl start hello_env
```
Just like any other services,
```
sudo systemctl status hello_env   # for getting the status
sudo systemctl stop hello_env     # stop the service
sudo systemctl restart hello_env  # restart the service
sudo systemctl enable hello_env   # wont start immediately but will start once the system boots
sudo systemctl disable hell_env   # similar to enable
```
# Web Hosting On Microsoft Azure

From Html/CSS Website to Python App, Host anything you want.

## Register An Account In Microsoft Azure
Register using your student mail to get $100 Azure Credits + No Credit Card Required: [Click Here To Register](https://azure.microsoft.com/en-us/free/students?wt.mc_id=studentamb_268271).

## Create a Virtual Machines with your choice of specifications.
1. After registering go to search bar and type ***Virtual Machine*** and click on it.
2. Click On Create.
3. Select Azure Virtual Machine
4. Fill Up All the details regarding the VM and resource groups.
5. Create the VM.

## Connect to VM
You can connect to VM by using your Terminal. Or, you can use your preferred SSH client.
1. Open Terminal and connect to your vm using ssh
```bash
ssh username@ip_address
```
2. Are you sure you want to continue connecting (yes/no/[fingerprint])? -> yes
3. Enter your password (Note: Password typed will not be visible to the user.)
4. Voila! You have connected to your virtual machine.

## Setup your server
Update Packages
```
sudo apt update && sudo apt upgrade -y
```

Note: If access denied problem occurs when running any of the command you can always change to root user by:
```
sudo su
```

Install nginx
```
sudo apt install nginx -y
```

Verify Nginx
```bash
systemctl status nginx
```

Now that we have installed webserver, lets have an HTML file served in it:
```bash
mkdir website
sudo nano index.html
```

Paste the following into `HTML` file:
```
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <title>Hello, World!</title>
</head>
<body>
    <h1>Hello, World!</h1>
    <p>We have just configured our Nginx web server on Ubuntu Server in Azure!</p>
</body>
</html>
```
Pretty cool, right?

Set Proper folder access with nginx
```bash
sudo usermod -aG www-data user-name
sudo chgrp -R www-data /home/user-name/website

sudo chmod -R 750 /home/user-name/website
sudo chmod 640 /home/user-name/website/index.html

sudo chmod 711 /home/user-name
```

Verify access
```bash
namei -l /home/user-name/website/index.html
```

Configure nginx configs
```bash
cd /etc/nginx/sites-available
sudo nano default
```

Add the virtual host for own custom domain
```bash
server {
    listen 80;
    listen [::]:80;

    server_name example.com www.example.com;

    root /path/to/website/folder;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Disable access anything other than above server_name and access
```bash
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    server_name _;

    return 404;
}
```

Verify if configuration is set properly
```bash
sudo nginx -t
```

Restart nginx for it to take effect
```bash
sudo systemctl restart nginx
``` 

And it works!!

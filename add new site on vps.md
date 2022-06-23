# Add New Site On VPS
- add new domain in cpanel
  
- Login To Vps
  * ssh user@ip -p 22

- Create domain name
  * edit : /etc/hosts
  * add : ip hostname
  * exp : 000.000.00.000 domain1.domain.com

- Add Site Virtual
``
***

nano /etc/apache2/sites-available/aps.conf
/etc/apache2/sites-available
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://localhost:5000/
    ProxyPassReverse / http://localhost:5000/
    ErrorLog /var/log/apache2/hellomvc-error.log
    CustomLog /var/log/apache2/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/ssl/private/aps.crt
    SSLCertificateKeyFile /etc/ssl/private/aps.key
</VirtualHost>
``

Enable Site
a2ensite aps.conf

Add kestrel service
nano /etc/systemd/system/kestrel-aps.service


[Unit]
Description=Aplikasi Absen Koordinat

[Service]
WorkingDirectory=/var/www/absenimigrasi
ExecStart=/usr/bin/dotnet /var/www/absenimigrasi/AbsenCoordinatWeb.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production

[Install]
WantedBy=multi-user.target




//Enable
sudo systemctl enable kestrel-aps.service
sudo journalctl -fu kestrel-aps.service


- github action
add yml file for .net

name: .NET

on:
  push:
    branches: [ Web ]
  pull_request:
    branches: [ Web ]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Setup .NET
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: 6.0.x
    - name: Restore Dependencies
      run: dotnet restore "AbsenCoordinatWeb/AbsenCoordinatWeb.csproj"

    - name: Build app
      run: dotnet build "AbsenCoordinatWeb/AbsenCoordinatWeb.csproj" -c Release --no-restore

    - name: Publish App
      run: dotnet publish "AbsenCoordinatWeb/AbsenCoordinatWeb.csproj" -c Release -o ./absenimigrasi



    - name: Copy To VPS
      uses: appleboy/scp-action@master
      env:
          HOST: ${{ secrets.HOST }}
          USERNAME: ${{ secrets.USERNAME }}
          PORT: ${{ secrets.PORT }}
          KEY: ${{ secrets.SSHKEY }}
      with:
          source: "./absenimigrasi"
          target: "/var/www"
          
    - name: cmod
      uses: appleboy/ssh-action@master
      with:
          host: ${{ secrets.HOST }}
          USERNAME: ${{ secrets.USERNAME }}
          PORT: ${{ secrets.PORT }}
          KEY: ${{ secrets.SSHKEY }}
          script: chmod -R 777 /var/www/absenimigrasi    

    - name: Run Kestrel Service
      uses: appleboy/ssh-action@master
      with:
          host: ${{ secrets.HOST }}
          USERNAME: ${{ secrets.USERNAME }}
          PORT: ${{ secrets.PORT }}
          KEY: ${{ secrets.SSHKEY }}
          script: systemctl restart kestrel-absenimigrasi.service




Get ssh key private from ~/.ssh


- SSL
* use Let's Encript (Certbot) Apache
sudo certbot --apache






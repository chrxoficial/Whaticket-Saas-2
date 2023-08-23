
# ChatPlus
# Sistema de multiplus atendentes para WhatsApp
  * Tema Dark
  * Sistema Saas
  * Baileys atualizado
  * Sem bugs
  * 
# Instalação direta no LINUX UBUNTU

```
sudo su "Para entrar no modo root"
```
```
timedatectl set-timezone America/Sao_Paulo && apt update && apt upgrade -y && apt install -y libgbm-dev wget unzip fontconfig locales gconf-service libasound2 libatk1.0-0 libc6 libcairo2 libcups2 libdbus-1-3 libexpat1 libfontconfig1 libgcc1 libgconf-2-4 libgdk-pixbuf2.0-0 libglib2.0-0 libgtk-3-0 libnspr4 libpango-1.0-0 libpangocairo-1.0-0 libstdc++6 libx11-6 libx11-xcb1 libxcb1 libxcomposite1 libxcursor1 libxdamage1 libxext6 libxfixes3 libxi6 libxrandr2 libxrender1 libxss1 libxtst6 ca-certificates fonts-liberation libappindicator1 libnss3 lsb-release xdg-utils python2-minimal build-essential redis-server && reboot
```
```
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
```
```
apt-get install -y nodejs
```
```
npm install -g npm@latest
```
```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
```
```
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```
```
sudo apt-get update -y && sudo apt-get -y install postgresql
```
```
sudo timedatectl set-timezone America/Sao_Paulo
```
```
npm install -g pm2
```
```
apt install -y snapd
```
```
snap install core
```
```
snap refresh core
```
```
apt-get remove certbot
```
```
snap install --classic certbot
```
```
ln -s /snap/bin/certbot /usr/bin/certbot
```
```
apt install -y nginx
```
```
rm /etc/nginx/sites-enabled/default
```
```
cat > /etc/nginx/conf.d/deploy.conf << 'END'
client_max_body_size 100M;
END
```
```
sudo su - postgres
```
```
createdb supheratalk;
```
```
psql
```
```
CREATE USER supheratalk SUPERUSER INHERIT CREATEDB CREATEROLE;
ALTER USER supheratalk PASSWORD 'supheratalk';
```
```
\q
```
```
exit
```
```
sudo iptables -F &&
sudo iptables -A INPUT -i lo -j ACCEPT &&
sudo iptables -A OUTPUT -o lo -j ACCEPT &&
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT &&
sudo iptables -A INPUT -p tcp --dport 3000 -j ACCEPT &&
sudo iptables -A INPUT -p tcp --dport 4000 -j ACCEPT &&
sudo iptables -A INPUT -p tcp --dport 5000 -j ACCEPT &&
sudo iptables -A INPUT -p tcp --dport 5432 -j ACCEPT &&
sudo iptables -A INPUT -p tcp --dport 6379 -j ACCEPT &&
sudo service netfilter-persistent save
```
```"Copiar arquivos para instalação e configurar .env"
{"EXEMPLO"
	NODE_ENV=
	BACKEND_URL=https://back.com.br
	FRONTEND_URL=https://front.com.br
	PROXY_PORT=443
	PORT=3000

	DB_DIALECT=postgres
	DB_HOST=localhost
	DB_PORT=5432
	DB_USER=spheratalk
	DB_PASS=spheratalk
	DB_NAME=spheratalk

	JWT_SECRET=kZaOTd+YZpjRUyyuQUpigJaEMk4vcW4YOymKPZX0Ts8=
	JWT_REFRESH_SECRET=dBSXqFg9TaNUEDXVp6fhMTRLBysP+j2DSqf7+raxD3A=

	REDIS_URI=redis://:123456@127.0.0.1:6379
	REDIS_OPT_LIMITER_MAX=1
	REDIS_OPT_LIMITER_DURATION=3000

	USER_LIMIT=10000
	CONNECTIONS_LIMIT=100000
	CLOSED_SEND_BY_ME=true

	GERENCIANET_SANDBOX=false
	GERENCIANET_CLIENT_ID=Client_Id_Gerencianet
	GERENCIANET_CLIENT_SECRET=Client_Secret_Gerencianet
	GERENCIANET_PIX_CERT=certificado-Gerencianet
	GERENCIANET_PIX_KEY=chave pix gerencianet


	VERIFY_TOKEN=whaticket

	FACEBOOK_APP_ID=
	FACEBOOK_APP_SECRET=
}
	cd ../backend
	npm install
	npm run build
	npm run db:migrate
	npm run db:seed
```
```
pm2 start dist/server.js --name back-nome
```
``` 
cat > /etc/nginx/sites-available/back-nome << 'END'
server {
  server_name back.com.br;
  location / {
    proxy_pass http://127.0.0.1:3000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_set_header Host 127.0.0.1;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_cache_bypass $http_upgrade;
  }
    add_header X-Cache $upstream_cache_status;
 }
END
```
```
ln -s /etc/nginx/sites-available/back-name /etc/nginx/sites-enabled

	cd ../frontend
	npm install
	npm run build
```
```
pm2 start server.js --name front-name
```
``` 
cat > /etc/nginx/sites-available/front-name << 'END'
server {
  server_name front.com.br;

  location / {
    proxy_pass http://127.0.0.1:4000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade \$http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host \$host;
    proxy_set_header X-Real-IP \$remote_addr;
    proxy_set_header X-Forwarded-Proto \$scheme;
    proxy_set_header X-Forwarded-For \$proxy_add_x_forwarded_for;
    proxy_cache_bypass \$http_upgrade;
  }
}
END
```
```
ln -s /etc/nginx/sites-available/front-name /etc/nginx/sites-enabled
```
```
pm2 save
```
```
pm2 startup

service nginx restart
service postgresql restart
service redis-server restart

certbot -m deploy@deploy.com \
          --nginx \
          --agree-tos \
          --non-interactive \
          --domains back.com.br,front.com.br
```
		  



--- Anotações
 Alterar postgresql para acessar com PGADMIN
  sudo nano /etc/postgresql/15/main/postgresql.conf
  Alterar alinha para "listen_address = '*' "
  
  sudo nano /etc/postgresql/15/main/pg_hba.conf
  Adicionar a linha "host    all    all    0.0.0.0/0    md5"
  
  sudo service postgresql restart
  ---

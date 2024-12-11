Actualizar paquetes e instalar Node.js y Git
sudo apt-get update
sudo apt-get install -y curl git build-essential
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

Crear un directorio para la aplicación
mkdir /home/usuario/blog-app
cd /home/usuario/blog-app

Inicializar el proyecto Node.js
npm init -y

Instalar las dependencias necesarias
npm install express ejs sequelize sqlite3

Despliegue de la Aplicación
Iniciar la aplicación:
cd /home/usuario/blog-app
node app.js

Configurar el Firewall (Google Cloud):
sudo ufw allow 3000/tcp
Acceder a la aplicación desde tu navegador:
http://130.211.87.70:3000/
ip externa >> curl ifconfig.me
Opcional: Ejecutar como servicio
Instalar pm2:
sudo npm install -g pm2
pm2 start app.js
pm2 startup systemd
# Sigue las instrucciones que pm2 te indicará para systemd

Opcional: Servir con Nginx
Instalar Nginx:
sudo apt-get install nginx -y
Configurar un Server Block / Virtual Host:
Edita o crea el archivo /etc/nginx/sites-available/blog:
server {
    listen 80;
    server_name TU_DOMINIO_O_IP;
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
Habilitar el sitio y reiniciar Nginx:
sudo ln -s /etc/nginx/sites-available/blog /etc/nginx/sites-enabled/
sudo systemctl restart nginx

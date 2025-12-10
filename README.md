#### Project Diagram 
```
                         HTTP/HTTPS Request
+----------------+         (port 80/443)         +------------------+
|                |  ---------------------------> |                  |
|    CLIENT      |                                |      NGINX       |
| (Browser/API)  |  <--------------------------- | (Reverse Proxy)  |
|                |       HTTP/HTTPS Response     |                  |
+----------------+                                +---------+--------+
                                                          |
                                                          | Proxy to upstream
                                                          | (e.g. http://127.0.0.1:8000)
                                                          v
                                                +---------+---------+
                                                |                   |
                                                |     GUNICORN      |
                                                |  (WSGI Server)    |
                                                |  Workers/Processes|
                                                +---------+---------+
                                                          |
                                                          | WSGI call
                                                          v
                                                +---------+---------+
                                                |                   |
                                                |      FLASK        |
                                                |   Application     |
                                                |   (app.py)        |
                                                +-------------------+

```
+++POS Project+++
------------------
+ API
	- Flask
	- Sqlite, Mysql, PostgreSQL (migration)
	- Auth (token base)
+ FrontEnd
	- Angular v20
------------------
+ Setting up
#### ▶ install install app and dependencies
```
sudo apt install nginx mysql-server python3 python3-pip python3-venv pkg-config default-libmysqlclient-dev build-essential -y
```
#### ▶ Configure Secure Mysql and Create user and DB in Mysql for flask 
```
	sudo mysql_secure_installation
```
#### Create Virtual Environment
```
python -m venv venv
# ▶ Activate venv
```
#### ▶ Activate venv
```
source venv/bin/activate
```

#### ▶ Install project Package
```
pip install -r requirements.txt
```


#### ▶ Initialize migrations
```
flask db init
flask db migrate -m "fresh migrate"
flask db upgrade
```
#### ▶ Test Flask Run with Gunicorn
gunicorn --bind 0.0.0.0:5000 app:app

#### ▶ Create Services for Flask Gunicorn
```
sudo nano /etc/systemd/system/flask.service
```
	[Unit]
	Description = Flask App Service
	After=network.target
	[Service]
	User=
	Group=
	WorkingDirectory=/home/ubuntu/POS_FLASK ( please choose your current project Path )
	Environment="PATH=/home/ubuntu/POS_FLASK/venv/bin"
	ExecStart=/home/ubuntu/POS_FLASK/venv/bin/gunicorn --bind 0.0.0.0:5000 app:app
	[Install]
	WantedBy=multi-user.target
```
sudo systemctl deamon-reload
sudo systemctl enable flask.service
sudo systemctl start flask.service
sudo systemctl status flask.service
```
#### ▶ Configure Reverse Proxy with nginx + ssl
```
nano /etc/nginx/site-avaiable/flask.conf
```
	server {
		listen 443 ssl;
		server_name your_dns_name_or_server_ip;
		ssl_certificate your_certificate_file_path;
		ssl_certificate_key your_certificate_key_file_path;
		location / {
			proxy_pass http://127.0.0.1:5000;
			proxy_set_header Host $host;
        	proxy_set_header X-Real-IP $remote_addr;
        	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        	proxy_set_header X-Forwarded-Proto $scheme;
		}
	}
```

sudo ln -s /etc/nginx/site-avaiable/flask.conf /etc/nginx/site-enable/flask.conf
sudo nginx -t
sudo systemctl restart nginx
```
#### ▶ test Access 
```
now you can access your flask app via https://your_dns_name_or_server_ip
ex. https://flask.setecist.uk
```



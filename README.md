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
#### install install app and dependencies
sudo apt install nginx mysql-server python3 python3-pip python3-venv pkg-config default-libmysqlclient-dev build-essential -y 
#### Configure Secure Mysql and Create user and DB in Mysql for flask 
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


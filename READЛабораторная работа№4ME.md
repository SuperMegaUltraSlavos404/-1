import requests
from flask import Flask, render_template, request
import psycopg2

app = Flask(__name__)

conn = psycopg2.connect(database="service_db",
                        user="postgres",
                        password="slava2004",
                        host="localhost",
                        port="5432")
cursor = conn.cursor()

@app.route('/login/', methods=['GET'])
def index():
    return render_template('login.html')


@app.route('/login/', methods=['POST'])
def login():
    username = request.form.get("username")
    password = request.form.get("password")
    cursor.execute("SELECT * FROM service.users WHERE login=%s AND password=%s", (str(username), str(password)))
    records = list(cursor.fetchall())
    if username=='' or password=='' or len(records)==0:
        methods=['GET']
        return render_template('login.html')
    else:
        return render_template('account.html',full_name=f'Имя пользователя-{records[0][1]}, Логин пользователя- {records[0][2]}, Пароль пользователя- {records[0][3]}')        


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Login</title>
</head>
<body>
    <form action=""method="post">
        <p>
                <label for="username">Username</label>
                <input type="text"name="username">
            </p>
            <p>
                <label for="password">Password</label>
                <input type="password"name="password">
            </p>
            <p>
                <input type="submit">
            </p>
    </form>
</body>
</html>


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <form action=""method="post">
        {% if full_name %}
        <p>{{full_name}}</p>
        {% endif %}

            </p>
    </form>
</head>
<body>

</body>
</html>

CREATE DATABAS Eservice_db;
CREATE SCHEMA service;
CREATE TABLE service.users 
(
	id SERIAL NOT NULL, 
	full_name VARCHAR NOT NULL, 
	login VARCHAR NOT NULL,
	password VARCHAR NOT NULL
);

INSERT INTO service.users (full_name, login,password) VALUES('Святослав','1506','2004');
INSERT INTO service.users (full_name, login,password) VALUES('Екатерина','1056','4002');
INSERT INTO service.users (full_name, login,password) VALUES('Константин','1663451','1654');
INSERT INTO service.users (full_name, login,password) VALUES('Елена','9815','694');
INSERT INTO service.users (full_name, login,password) VALUES('Игорь','98463','<12348>');
INSERT INTO service.users (full_name, login,password) VALUES('Тамара','<154412gd>','gjhs64');
INSERT INTO service.users (full_name, login,password) VALUES('Ярослав','<апвы634','756>');
INSERT INTO service.users (full_name, login,password) VALUES('Светлана','15','2006');
INSERT INTO service.users (full_name, login,password) VALUES('Иван','06','2');
INSERT INTO service.users (full_name, login,password) VALUES('Анна','16','4');
select * from service.users;

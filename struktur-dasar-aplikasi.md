# Struktur Dasar Aplikasi

Flask dikenal sebagai framework dengan tingkat fleksibilitas yang tinggi, sehingga tidak ada aturan tetap untuk pengorganisiran struktur direktori nya. Untuk panduan kali ini, struktur direktori akhir akan terlihat seperti dibawah ini:

```text
├── migrations/
|   └── ...
├── templates/
|   └── 404.html
|   └── 500.html
|   └── base.html
|   └── index.html
|   └── navbar.html
|   └── user.html
├── .gitignore
├── README.md
├── app.py
├── forms.py
├── requirements.txt
```

## Inisialisasi

Semua aplikasi harus melewati instance aplikasi yang artinya harus di import object dari module yang akan digunakan. Contoh penulisannya untuk Flask biasanya seperti ini:

```python
# app.py
from flask import Flask
app = Flask(__name__)
```

* Pertama kita mengimpor kelas Flask. Sebuah instance dari kelas ini akan menjadi [WSGI](https://www.fullstackpython.com/wsgi-servers.html).
* Selanjutnya kita membuat instance dari kelas ini. Argumen pertama adalah nama modul atau paket aplikasi. Jika kamu menggunakan modul tunggal \(seperti dalam contoh ini\), Kamu harus menggunakan `__name__` karena tergantung pada apakah itu dimulai sebagai awal aplikasi atau diimpor sebagai modul, namanya akan berbeda menjadi `__main__`. Ini diperlukan agar Flask tahu di mana harus mencari templat, file statis, dan sebagainya. Untuk informasi lebih lanjut, lihat dokumentasi [Flask](https://flask.palletsprojects.com/en/1.1.x/api/#flask.Flask).

Sekarang buatlah sebuah file dengan nama `app.py` di dalam directory flask-web-development. Buka di text editor/IDE favorit kamu dan tuliskan kode diatas.

## Fungsi Routes dan Views

Klien seperti web browser mengirim permintaan ke web server. Contoh, aplikasi Flask perlu mengetahui kode apa yang harus dijalankan untuk setiap URL yang diminta, sehingga ia menyimpan URL Fungsi python. Hubungan antara URL dan fungsi yang menanganinya disebut **route**.

Cara paling mudah untuk menentukan route dalam aplikasi Flask adalah melalui dekorator\(@\)app.route. Contoh seperti ini:

```python
# app.py
@app.route('/')
def index():
    return '<h1>Hello, World!</h1>'
```

dan tambahkan code tersebut di app.py setelah `app = Flask(__name__)` sehingga source code lengkapnya terlihat seperti ini:

```python
# app.py
from flask import Flask
app = Flask(__name__)

@app.route('/')
def index():
    return '<h1>Hello, World!</h1>'
```

## Development Web Server

Yang kita butuhkan adalah perintah untuk menjalan web server di web browser. Pastikan virtual environment kamu sudah aktif kemudian ketikkan dan jalankan perintah ini di terminal kamu untuk menjalankan web server.

**Linux / Mac**

```bash
(env) $ export FLASK_APP=app.py
(env) $ flask run
        * Serving Flask app "app.py"
        * Environment: production
          WARNING: This is a development server. Do not use it in a production deployment.
          Use a production WSGI server instead.
        * Debug mode: off
        * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

**Windows**

```bash
(env) $ set FLASK_APP=app.py
(env) $ flask run
        * Serving Flask app "app.py"
        * Environment: production
          WARNING: This is a development server. Do not use it in a production deployment.
          Use a production WSGI server instead.
        * Debug mode: off
        * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Selamat! Server anda berhasil berjalan. Cobalah untuk akses URL [http://127.0.0.1:5000/](http://127.0.0.1:5000/) sehingga memunculkan tampilan seperti ini di web browser kamu. Jika halaman yang ditampilkan adalah 404 periksalah kembali URL kamu. Untuk mengakhiri sesi pada server saat ini tekan `CTRL + C`

![http://127.0.0.1:5000/](https://lh6.googleusercontent.com/wYMVMBotN1O7lgFbluWp30YBhm3ZZHB1uJxE9OleJFF7cf2hUqrKfwQy2HZjraA6SJucI8TmiHYWcCeuLnt5ZVfw1M1ud5DvXf4A6CpT5d7ap85z37Mlos14h1sgle-HyJHxnRAH)

## Router Dinamis

Route dinamis memungkinkan kita untuk mengirim value dari link atau url ke salah satu route URL. Dan value yang dikirim tersebut dapat digunakan juga di dalam fungsi route tersebut.

```python
# app.py
@app.route('/user/<name>')
def user(name):
    return '<h1>Hello, {}</h1>'.format(name)
```

Ketikan kode tersebut, hingga jadi seperti dibawah ini

```python
# app.py
from flask import Flask
app = Flask(__name__)

@app.route('/')
def index():
    return '<h1>Hello, World!</h1>'

@app.route('/user/<name>')
def user(name):
    return '<h1>Hello, {}</h1>'.format(name)
```

Lalu jalankan kembali server dengan cara

```bash
(env) $ flask run
```

Ketika web server berhasil jalan kembali ketikkan URL ini di web browser kamu [https://127.0.0.1:5000/user/David](https://127.0.0.1:5000/user/David) maka web browser kamu akan menampilkan `Hello, David`

 

![ https://127.0.0.1:5000/user/David](https://lh3.googleusercontent.com/H5lnQIlS134nNf4Fsc0p4VYKKF_RAPn6up1bzrPN9Fy9-Mahpl4oHVou4DUWVAILQyeX1hU-gmZPHf5nwAUMGruPZs07db8u0vQZ_uQ-qIoV5_mtLZZhRF6i7QI1DWvjd4FpCyh7)

## Mode Debug

Debug mode ini berfungsi untuk menampilkan pesan error di web browser dan juga untuk memuat kembali server ketika ada perubahan di program kamu \(_Lazy Loading_\). Biasanya `debug=True` ini digunakan ketika development/pengembangan. Caranya gampang cukup ketikkan perintah ini di terminal:

**Linux/Mac**

```bash
(env) $ export FLASK_DEBUG=1 #1 untuk True
(env) $ export FLASK_ENV=development
(env) $ flask run
        * Serving Flask app “app.py” (Lazy Loading)
        * Environment: development
        * Debug mode: on
        * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
        * Restarting with stat
        * Debugger is active!
        * Debugger PIN: 172-185-267
```

**Windows**

```bash
(env) $ set FLASK_DEBUG=1 #1 untuk True
(env) $ set FLASK_ENV=development
(env) $ flask run
        * Serving Flask app “app.py” (Lazy Loading)
        * Environment: development
        * Debug mode: on
        * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
        * Restarting with stat
        * Debugger is active!
        * Debugger PIN: 172-185-267
```

## Pilihan Perintah Command Line Flask

Perintah flask mendukung banyak opsi. Untuk dapat melihatnya, kamu dapat menjalankan dengan perintah `flask --help` atau hanya `flask` tanpa argumen, maka akan terlihat seperti ini:

```bash
(venv) $ flask --help
Usage: flask [OPTIONS] COMMAND [ARGS]...

  A general utility script for Flask applications.

  Provides commands from Flask, extensions, and the application. Loads the
  application defined in the FLASK_APP environment variable, or from a
  wsgi.py file. Setting the FLASK_ENV environment variable to 'development'
  will enable debug mode.

	$ export FLASK_APP=hello.py
	$ export FLASK_ENV=development
	$ flask run

Options:
  --version  Show the flask version
  --help 	Show this message and exit.

Commands:
  routes  Show the routes for the app.
  run 	Run a development server.
  shell   Run a shell in the app context.
```

Perintah `flask routes` digunakan untuk melihat seluruh endpoint yang telah di tambahkan di flask.

Perintah `flask run` digunakan untuk menjalankan aplikasi yang sudah di inisialisasi di flask menggunakan `FLASK_APP=NamaApp`.

Perintah `flask shell` digunakan untuk memulai Python shell dalam konteks aplikasi. Kamu dapat menggunakan sesi ini untuk menjalankan tugas, pemeliharaan, menguji atau untuk men-debug masalah. Perintah `flask run` berarti menjalankan aplikasi dengan status _development web server_.


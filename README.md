# WeirdWired

## Link to website: https://weird-wired.adaptable.app/ (got disabled ;-;)

## Langkah-langkah pembuatan

### 1. Membuat sebuah proyek Django baru
- Membuat sebuah direktori dengan nama aplikasi yang akan dibuat, yaitu `weird-wired`.
- Membuka direktori weird-wired di command prompt, kemudian menjalankan perintah `python -m venv env` untuk membuat virtual environment untuk project ini.
- Membuat sebuah file txt dengan nama `requirements.txt` yang berisi dependencies sebagai berikut: 
    ```
    django
    gunicorn
    whitenoise
    psycopg2-binary
    requests
    urllib3
    ```
  File ini kemudian disimpan dalam direktori utama weird-wired.

- Mengaktifkan virtual environment dengan command `env\Scripts\activate.bat` pada command prompt kemudian memasang dependencies dengan command `pip install -r requirements.txt`
- Membuat sebuah project django baru bernama `weird_wired` dengan command `django-admin startproject weird_wired .`
- Membuka `settings.py` pada direktori project weird_wired, kemudian mengatur bagian `ALLOWED_HOSTS` menjadi `ALLOWED_HOSTS = ["*"]` supaya aplikasi ini dapat diakses oleh semua host.

  **- Menghubungkan project ke GitHub -**
- Membuat sebuah repositori baru di github dengan nama `weird-wired`
- Menginisiasi git pada repositori lokal weird-wired dengan command `git init`
- Menghubungkan repositori lokal dengan repositori github menggunakan command `git remote add origin https://github.com/pakbepe/test.git`
- Push direktori ke GitHub dengan command `git push -u origin main`

### 2. Membuat aplikasi dengan nama main pada proyek tersebut.
- Membuka direktori utama, yaitu direktori weird-wired, pada command prompt kemudian menjalankan command `python manage.py startapp main` untuk membuat aplikasi baru bernama `main`.
- Mendaftarkan aplikasi `main` ke dalam project dengan cara membuka `settings.py` pada direktori project `weird_wired` kemudian menambahkan `main` ke dalam `INSTALLED_APPS` sebagai berikut:
    ```python
    INSTALLED_APPS = [
        ...,
        'main',
        ...
    ]
    ```

### 3. Melakukan routing pada proyek agar dapat menjalankan aplikasi `main`.
- Membuka file `urls.py` pada direktori project `weird-wired`
- Mengimport fungsi `include`, kemudian menambahkan path dalam `urlpatterns` yang mengarah kepada urls aplikasi `main` sebagai berikut:
    ```python
    from django.urls import path, include

    urlpatterns = [
        ...
        path('main/', include('main.urls')),
        ...
    ]
    ```

### 4. Membuat model pada aplikasi main dengan nama Item dan beberapa atribut
- Membuka file `models.py` yang ada pada direktori aplikasi `main`
- Mengimpor fungsi `models`, kemudian membuat class Item pada file tersebut dan menambahkan atribut `name`, `amount`, dan `description` sebagai berikut:
    ```python
    from django.db import models

    class Item(models.Model):
        name = models.CharField(max_length=255)
        amount = models.IntegerField()
        description = models.TextField()
    ```

### 5. Membuat sebuah fungsi pada views.py untuk dikembalikan ke dalam sebuah template HTML
- Membuat sebuah folder/direktori baru bernama `templates` di dalam direktori aplikasi `main`
- Di dalam direktori `templates`, buat sebuah file bernama `main.html`
- Mengisi `main.html` dengan code sebagai berikut:
    ```html
    <h1>WeirdWired</h1>

    <h4>Name: {{ name }}</h4>
    <h4>Class: {{ class }}</h4>

    ```
- Membuka file `views.py` yang ada dalam direktori `main`
- Mengimpor fungsi `render`, kemudian membuat fungsi bernama `show_main` dalam file tersebut sebagai berikut:
    ```python
    from django.shortcuts import render

    def show_main(request):
        context = {
            'name': 'Evelyn P.H. Silalahi',
            'class': 'PBP A'
        }

        return render(request, "main.html", context)
    ```
  Fungsi `show_main` ini akan terhubung dengan `main.html` yang ada pada direktori `template`, kemudian mengisi bagian {{ name }} dan {{ class }} dengan value yang sesuai pada `context`.

### 6. Membuat sebuah routing pada urls.py aplikasi main untuk memetakan fungsi yang telah dibuat pada views.py.
- Membuat sebuah file bernama `urls.py` di dalam direktori aplikasi `main`
- Mengimport fungsi `path` dari `django.urls` dan mengimport fungsi `show_main` dari `main.views`, kemudian menambahkan `urlpatterns` sebagai berikut:
    ```python
    from django.urls import path
    from main.views import show_main

    app_name = 'main'

    urlpatterns = [
        path('', show_main, name='show_main'),
    ]
    ```

### 7. Melakukan deployment ke Adaptable
- Login ke Adaptable.io menggunakan akun GitHub, kemudian tekan tombol `New App` dan pilih `Connect an Existing Repository`.
- Pilih repositori `weird-wired` sebagai repositori yang akan di-deploy, kemudian branch `main` sebagai deployment branch.
- Pilih `Python App Template` sebagai template deployment, kemudian pilih `PostgreSQL` sebagai tipe database. 
- Pilih versi python yang sesuai dengan versi yang digunakan pada project, kemudian pada bagian `Start Command` masukkan perintah `python manage.py migrate && gunicorn weird_wired.wsgi`
- Masukkan nama `weird-wired` sebagai nama aplikasi sekaligus nama domain situs web aplikasi.
- Klik checkbox `HTTP Listener on PORT`, kemudian klik `Deploy App` untuk memulai proses deployment aplikasi.

## Bagan _request client_ ke web aplikasi berbasis Django
![Request Client Map](https://github.com/evelynphs/weird-wired/blob/main/bagan.png) <br>
  Penjelasan:
  1. Pertama, Django akan menerima `HTTP request` berupa URL, kemudian memeriksa `urls.py` untuk mencari pattern yang sesuai dengan URL request tersebut. Setelah ditemukan, Django akan memanggil fungsi view yang sesuai dengan URL tersebut dan mengirim _request_.
  2. View pada `views.py` akan menghubungi `models.py`, kemudian mencari model yang relevan dengan _request_. Kemudian `models.py` akan mengoperasikan data sesuai dengan _request_, kemudian mengirimkan data tersebut kepada `views.py`.
  3. View akan me-_render_ data ke dalam _template_ yang sesuai pada file HTML.
  4. Tampilan HTML yang sudah di-_render_ kemudian diteruskan kepada browser sebagai `HTTP response` untuk ditampilkan kepada user.


## Mengapa kita menggunakan virtual environment?
Secara *default*, setiap project yang ada dalam suatu komputer akan menggunakan direktori yang sama untuk menyimpan package dan dependencies dari project tersebut. Direktori ini dapat dianggap sebagai *global/base environment*. Hal ini dapat menjadi suatu kendala ketika kita mengerjakan lebih dari satu project dalam suatu komputer, di mana setiap project memiliki dependencies yang berbeda dari satu sama lain, seperti libaries dan modules yang berbeda, atau bahkan versi python yang berbeda. Oleh karena itu, *virtual environment* dapat digunakan untuk membuat *development environment* yang berbeda untuk tiap project, sehingga project-project tersebut akan terpisah dari satu sama lain. 

Ketika diaktifkan, *virtual environment* akan "mengisolasi" sebuah project dari *global/base environment* sehingga *dependencies* untuk project tersebut akan tetap konsisten dan tidak "tercampur" atau "bertabrakan" dengan komponen-komponen lain yang ada pada *global/base environment*. Jadi, ketika kita mengerjakan suatu project dengan *virtual environment*, komputer hanya akan fokus pada komponen-komponen yang diperlukan untuk project tersebut sehingga kita akan lebih mudah untuk mengatur *packages* dan *dependencies*-nya tanpa memengaruhi *global environment*.

## Apakah kita tetap dapat membuat aplikasi web berbasis Django tanpa menggunakan virtual environment?
Bisa, tetapi hal tersebut akan sulit dilakukan apabila kita mengerjakan lebih dari satu project dalam satu komputer. Tanpa *virtual environment*, *dependecies* dan versi python dari project-project yang berbeda bisa saling "bertabrakan". 

Misalnya, dalam suatu komputer yang sama, terdapat project A dan project B. Project A dibuat lebih dahulu menggunakan **Django 4.0**. Beberapa lama kemudian, user menginstall **Django 4.2** untuk mengerjakan project B. Ketika user ingin kembali mengerjakan project A, kemungkinannya adalah banyak error yang akan terjadi karena konflik antara versi Django untuk project A **(4.0)** dengan versi Django yang saat itu terinstall di komputer **(4.2)**. Jika user memutuskan untuk kembali menginstall **Django 4.0** demi mengerjakan project A, maka untuk selanjutnya user akan mengalami kendala ketika ingin mengerjakan project B karena project B bergantung pada **Django 4.2**. Oleh karena itu, virtual environment sebaiknya digunakan untuk menghindari konflik antar versi seperti ini.

## MVC, MVT, MVVM dan perbedaan dari ketiganya
MVC, MVT, dan MVVM adalah contoh dari software architecture pattern yang paling populer di kalangan para developer. Arsitektur ini dibuat dengan tujuan untuk memisahkan beberapa komponen aplikasi supaya lebih mudah untuk di-maintain.

   ### 1. MVC: Model-View-Control
   Arsitektur ini membagi _code_ program ke dalam 3 bagian, yaitu:
   - Model: Komponen yang bertanggungjawab mengatur data aplikasi. Komponen ini digunakan untuk memanipulasi, memodifikasi, dan memproses data pada database.
   - View: Merupakan UI (User Interface) dari aplikasi yang mengatur tampilan yang dapat dilihat oleh user. View akan memvisualisasikan data yang tersimpan dalam model, kemudian mengatur interaksi antara user dengan data tersebut.
   - Control: Komponen yang mengintegrasikan view dan model. Control akan mengatur interaksi antara data dalam model dan proses yang terjadi dalam view.

   ### 2. MVT: Model-View-Template
   Arsitektur ini mirip dengan MVC, tapi tanpa bagian Control. Pada arsitektur ini, bagian Control sudah di-handle oleh framework.
   - Model: Berperan sebagai _interface_ data dan sebagai struktur logika dibalik suatu aplikasi web.
   - View: Berinteraksi dengan model, membawa data dari model, dan me-_render_ template berdasarkan data tersebut. 
   - Template: Komponen yang sepenuhnya berperan untuk mengatur UI (User Interface). Template merupakan kode HTML yang akan me-_render_ data.

   ### 3. MVVM: Model-View-ViewModel
   Arsitektur ini memisahkan _logic_ dari tampilan data (view dan UI) dari _logic_ inti suatu aplikasi. Ketiga bagian dari arsitektur ini yaitu:
   - Model: berperan mengatur abstraksi dari data dan bekerjasama dengan ViewModel untuk mengambil serta menyimpan data.
   - View: merupakan UI (User Interface) yang bertugas menampilkan data kepada user dan menginformasikan ViewModel mengenai apa yang dilakukan oleh user. 
   - ViewModel: berperan sebagai perantara antara model dan view, mengoperasikan data dalam model yang relevan dengan view.

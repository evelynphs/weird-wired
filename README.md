
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
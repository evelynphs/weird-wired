# WeirdWired

### Link to website: https://weird-wired.adaptable.app/ (got disabled ;-;)

<details>
<summary>Tugas 2</summary>
<br>

# Tugas 2
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

## MVC, MVT, MVVM
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
</details>

<details>
<summary>Tugas 3</summary>
<br>

# Tugas 3

## Langkah-langkah pengerjaan:

### 1. Membuat input form untuk menambahkan objek model pada app sebelumnya.
- Membuat file baru `forms.py` pada direktori `main` sebagai tempat untuk membuat struktur form
- Membuat sebuah class untuk form dengan nama `ItemForm`, kemudian menyatakan jenis model yang akan digunakan untuk form ini, yaitu `Item`, lalu menentukan jenis field apa saja yang akan digunakan untuk menginput data pada model, yaitu `"name"`, `"amount"`, dan `"description`. 
    ```python
    from django.forms import ModelForm
    from main.models import Item

    class ItemForm(ModelForm):
        class Meta:
            model = Item
            fields = ["name", "amount", "description"]
    ```

- Mengimport beberapa fungsi pada `views.py` dalam direktori `main`, yaitu fungsi `HttpResponseRedirect`, `ProductForm`, `reverse`, dan `ItemForm`. 
    ```python
    from django.http import HttpResponseRedirect
    from django.http import HttpResponse
    from main.forms import ItemForm
    ```
  Kemudian membuat sebuah fungsi baru yaitu `create_item`, yaitu fungsi yang akan mengambil data dari form dan otomatis menambahkannya pada data models ketika di-submit. Fungsi ini juga akan melakukan _redirect_ ke main page setelah user selesai men-submit form.
    ```python
    def create_item(request):
    form = ItemForm(request.POST or None)

    if form.is_valid() and request.method == "POST":
        form.save()
        return HttpResponseRedirect(reverse('main:show_main'))

    context = {'form': form}
    return render(request, "create_item.html", context)
    ```

- Membuat page HTML untuk tampilan form dengan membuat file baru bernama `create_item.html` pada direktori `main/templates`, kemudian mengisi file tersebut dengan code berikut:
    ```html
    {% extends 'base.html' %} 

    {% block content %}
    <h1>Add New Item</h1>

    <form method="POST">
        {% csrf_token %}
        <table>
            {{ form.as_table }}
            <tr>
                <td></td>
                <td>
                    <input type="submit" value="Add Item"/>
                </td>
            </tr>
        </table>
    </form>

    {% endblock %}
    ```
    Code `{{ form.as_table }}` digunakan untuk menampilkan form yang sudah dibuat pada `forms.py`, kemudian `<input type="submit" value="Add Item"/>` merupakan tombol submit mengirim data dan request kepada fungsi `create_item` pada `views.py`.

- Menambahkan  button `Add New Item` yang apabila di-klik akan me-redirect ke `create_item.html`. Code berikut diletakkan sebelum `{% endblock content %}`.:
    ```html
    <a href="{% url 'main:create_item' %}">
        <button>
            Add New Item
        </button>
    </a>
    ```

- Pada `urls.py` dalam direktori `main`, import fungsi `create_item` dan tambahkan `path('create-item', create_item, name='create_item')` pada `urlpatterns` agar page `create_item.html` dapat diakses dan digunakan sesuai fungsi.


### 2. Menambahkan 5 fungsi views untuk melihat objek yang sudah ditambahkan dalam format HTML, XML, JSON, XML by ID, dan JSON by ID.

- Agar data/objek dapat terlihat dalam bentuk tabel dengan format HTML di main page, tambahkan kode berikut ini pada `main.html` :
    ```html
    <table>
        <tr>
            <th>Name</th>
            <th>Amount</th>
            <th>Description</th>
            <th>Date Added</th>
        </tr>
    
        {% for item in items %}
            <tr>
                <td>{{item.name}}</td>
                <td>{{item.amount}}</td>
                <td>{{item.description}}</td>
                <td>{{item.date_added}}</td>
            </tr>
        {% endfor %}
    </table>
    <br />
    ```

  Kemudian, pada fungsi `show_main` di `views.py`, tambahkan `items = Item.objects.all()` dan `'items': items` agar main page terhubung dengan data yang telah disubmit dari form dan dapat menampilkannya. Posisi penempatannya adalah sebagai berikut:
    ```python
    def show_main(request):
    items = Item.objects.all()

    context = {
        'name': 'Evelyn',
        'class': 'PBP A',
        'items': items ,
    }

    return render(request, "main.html", context)
    ```

- Untuk menampilkan data dalam bentuk XML dan JSON, import fungsi `HttpResponse` dan `serializers` pada `views.py`
    ```python
    from django.http import HttpResponse
    from django.core import serializers
    ```
  Kemudian, tambahkan fungsi `show_xml` dan `show_json` sebagai berikut:
    ```python
    def show_xml(request):
        data = Item.objects.all()
        return HttpResponse(serializers.serialize("xml", data), content_type="application/xml")

    def show_json(request):
        data = Item.objects.all()
        return HttpResponse(serializers.serialize("json", data), content_type="application/json")
    ```
    Variabel `data = Item.objects.all()` digunakan untuk menyimpan seluruh data dari `Item`. `serializers` digunakan untuk men-_translate_ data pada model menjadi format data lain, dalam hal ini format xml dan json.

- Untuk menampilkan data dalam bentuk XML by ID dan JSON by ID, tambahkan fungsi `show_xml_by_id` dan `show_json_by_id` pada `views.py` sebagai berikut:
    ```python
    def show_xml_by_id(request, id):
    data = Item.objects.filter(pk=id)
    return HttpResponse(serializers.serialize("xml", data), content_type="application/xml")

    def show_json_by_id(request, id):
        data = Item.objects.filter(pk=id)
        return HttpResponse(serializers.serialize("json", data), content_type="application/json")
    ```
    Variabel `data = Item.objects.filter(pk=id)` digunakan untuk menyimpan data dari `Item` berdasarkan ID tertentu.

### 3. Membuat routing URL untuk masing-masing views yang telah ditambahkan pada poin 2.
- Buka `urls.py` pada direktori `main`, kemudian import fungsi `show_xml`, `show_json`, `show_xml_by_id`, dan `show_json_by_id` sebagai berikut:
    ```python
    from main.views import show_main, create_item, show_xml, show_json, show_xml_by_id, show_json_by_id
    ```

- Tambahkan path untuk masing-masing fungsi tersebut ke dalam `urlpatterns` sebagai berikut:
    ```python
    ...
    path('xml/', show_xml, name='show_xml'),
    path('json/', show_json, name='show_json'),
    path('xml/<int:id>/', show_xml_by_id, name='show_xml_by_id'),
    path('json/<int:id>/', show_json_by_id, name='show_json_by_id'),
    ...
    ```
    
## Apa perbedaan antara form POST dan form GET dalam Django?
POSt adalah request yang digunakan untuk mengirim data ke server untuk membuat atau meng-_update_ suatu resource. Request ini akan mengirim data berupa input dari user kepada server untuk diolah. Data tersebut diletakkan di dalam _body_ dari request sehingga tidak terlihat oleh user.

GET adalah request yang digunakan untuk membaca atau mengambil data dari suatu resource tertentu dalam server. Berbeda dengan POST, request GET ini mencantumkan data pada URL untuk spesifikasi dari resource yang ingin diambil. Data tersebut sangat mudah terlihat dan karena itu, request GET tidak cocok untuk mengirim data yang bersifat rahasia seperti password atau informasi pribadi.

## Apa perbedaan utama antara XML, JSON, dan HTML dalam konteks pengiriman data?
HTML **tidak** digunakan untuk menyimpan dan mentransmisi data, melainkan digunakan untuk mengatur struktur tampilan sebuah halaman, termasuk mengatur tampilan data-data yang muncul di halaman tersebut. XML dan JSON berbeda dari HTML. XML dan JSON digunakan untuk menyimpan dan mentransmisi data dalam struktur tertentu. Berikut perbedaan antara XML dan JSON:

| XML | JSON |
| :---: | :---: |
|XML menyimpan data dalam struktur _tree_ dengan _namespace_ untuk merepresentasikan masing-masing kategori data.|Menggunakan struktur data seperti _map_ yang menggunakan pasangan _key-value_. |
|Data direpresentasikan dalam bentuk elemen dalam DOM node. |Data direpresentasikan dalam bentuk _object_.|
|Dokumen XML lebih kompleks dan ukuran file nya lebih besar sehingga transmisi datanya cenderung lebih lambat. |Dokumen JSON lebih sederhana sehingga ukuran file nya lebih kecil dan transmisi datanya cenderung lebih cepat.|
|Mendukung semua tipe data yang didukung oleh JSON, dan beberapa tipe data lainnya yang cenderung lebih kompleks seperti _date_, _time_, dan _binary data_. |Hanya mendukung tipe data _string, number, boolean, Null, Array, Object_. |
|Menggunakan XML DOM untuk membaca data dalam bentuk dokumen XML. |Menggunakan JSON.parse() untuk mem-_parse_ data dalam bentuk JSON string.|

## Mengapa JSON sering digunakan dalam pertukaran data antara aplikasi web modern?
- Format data JSON mudah dibaca oleh manusia dan mudah di-_parse_ oleh sistem.
- JSON dapat dikonversi ke dalam bentuk tipe data _JavaScript objects_ sehingga dapat mempermudah pengembangan aplikasi yang menggunakan JavaScript sebagai _scripting language_ utama.
- JSON didukung oleh sebagian besar browser, server web, dan API web modern sehingga dengan menggunakan JSON, pertukaran data antar sistem atau _environment_ yang berbeda akan lebih mudah untuk dilakukan.
- Struktur data JSON cenderung sederhana dan "ringan", sehingga transmisi data dapat dilakukan dengan cepat. Hal ini dapat mempermudah transfer data dalam jumlah besar, meningkatkan kecepatan respon suatu aplikasi web, dan mengurangi _bandwith_. 

## Screenshot dari hasil akses URL pada Postman
### 1. HTML
![HTML response](https://github.com/evelynphs/weird-wired/blob/main/images/html_data.png)

### 2. XML
![XML response](https://github.com/evelynphs/weird-wired/blob/main/images/xml_data.png)

### 3. JSON
![JSON response](https://github.com/evelynphs/weird-wired/blob/main/images/json_data.png)

### 4. XML by ID
![XML response](https://github.com/evelynphs/weird-wired/blob/main/images/xml_id_data.png)

### 5. JSON by ID
![JSON response](https://github.com/evelynphs/weird-wired/blob/main/images/json_id_data.png)

</details>

<details>
<summary>Tugas 4</summary>
<br>

# Tugas 4

## Langkah-langkah pengerjaan:

### 1. Mengimplementasikan fungsi registrasi

- Mengimport `redirect`, `UserCreationForms`, dan `messages` pada `views.py` yang ada pada direktori main.

- Membuat sebuah fungsi baru bernama `register` sebagai berikut:
    ```python
    def register(request):
    form = UserCreationForm()

    if request.method == "POST":
        form = UserCreationForm(request.POST)
        if form.is_valid():
            form.save()
            messages.success(request, 'Your account has been successfully created!')
            return redirect('main:login')
    context = {'form':form}
    return render(request, 'register.html', context)
    ```
    Fungsi ini menggunakan modul bawaan dari Django, yaitu `UserCreationForm`, untuk mempermudah pembuatan fungsi registrasi. Fungsi ini akan melakukan validasi form regitrasi yang telah diisi dengan menggunakan `form.is_valid`, kemudian menggunakan `form.save` untuk menyimpan data yang telah diisi pada form tersebut.

- Membuat sebuah file baru bernama `register.html` pada direktori `main/templates` untuk membuat sebuah tampilan halaman registrasi pada web. Isi dari file tersebut adalah sebagai berikut:
    ```html
    {% extends 'base.html' %}

    {% block meta %}
        <title>Register</title>
    {% endblock meta %}

    {% block content %}  

    <div class = "login">
        
        <h1>Register</h1>  

            <form method="POST" >  
                {% csrf_token %}  
                <table>  
                    {{ form.as_table }}  
                    <tr>  
                        <td></td>
                        <td><input type="submit" name="submit" value="Daftar"/></td>  
                    </tr>  
                </table>  
            </form>

        {% if messages %}  
            <ul>   
                {% for message in messages %}  
                    <li>{{ message }}</li>  
                    {% endfor %}  
            </ul>   
        {% endif %}

    </div>  

    {% endblock content %}
    ```
    Halaman ini akan menerima data berupa informasi registrasi user melalui form, kemudian mengirimkan data tersebut ke server menggunakan request `POST` untuk diproses. 

- Pada `urls.py` dalam direktori `main`, import fungsi `register` yang baru dibuat dengan menggunakan `from main.views import register`. Kemudian, pada `urlpatterns`, tambahkan path menuju halaman register.
    ```python
    ...
    path('register/', register, name='register'),
    ...
    ```
### 2. Mengimplementasi fungsi login

- Mengimport `authenticate` dan `login` pada `views.py` yang ada pada direktori main menggunakan kode `from django.contrib.auth import authenticate, login.`

- Membuat sebuah fungsi baru bernama `login_user` sebagai berikut:
    ```python
    def login_user(request):
    if request.method == 'POST':
        username = request.POST.get('username')
        password = request.POST.get('password')
        user = authenticate(request, username=username, password=password)
        if user is not None:
            login(request, user)
            return redirect('main:show_main')
        else:
            messages.info(request, 'Sorry, incorrect username or password. Please try again.')
    context = {}
    return render(request, 'login.html', context)
    ```
    Fungsi ini akan mengambil data login user berupa username dan password, kemudian mengautentikasinya menggunakan modul `authenticate` bawaan dari Django. Apabila username dan password user tersebut terdaftar, maka fungsi ini akan lanjut menggunakan modul `login` bawaan dari Django untuk memproses login user.

- Membuat sebuah file baru bernama `login.html` pada direktori `main/templates` untuk membuat sebuah tampilan halaman login pada web. Isi dari file tersebut adalah sebagai berikut:
    ```html
    {% extends 'base.html' %}

    {% block meta %}
        <title>Login</title>
    {% endblock meta %}

    {% block content %}

    <div class = "login">

        <h1>Login</h1>

        <form method="POST" action="">
            {% csrf_token %}
            <table>
                <tr>
                    <td>Username: </td>
                    <td><input type="text" name="username" placeholder="Username" class="form-control"></td>
                </tr>
                        
                <tr>
                    <td>Password: </td>
                    <td><input type="password" name="password" placeholder="Password" class="form-control"></td>
                </tr>

                <tr>
                    <td></td>
                    <td><input class="btn login_btn" type="submit" value="Login"></td>
                </tr>
            </table>
        </form>

        {% if messages %}
            <ul>
                {% for message in messages %}
                    <li>{{ message }}</li>
                {% endfor %}
            </ul>
        {% endif %}     
            
        Don't have an account yet? <a href="{% url 'main:register' %}">Register Now</a>

    </div>

    {% endblock content %}
    ```
    Halaman ini akan menerima data berupa username dan password melalui form, kemudian mengirimkan data tersebut ke server menggunakan request `POST` untuk diproses.

- Pada `urls.py` dalam direktori `main`, import fungsi `login_user` yang baru dibuat dengan menggunakan `from main.views import login_user`. Kemudian, pada `urlpatterns`, tambahkan path menuju halaman register.
    ```python
    ...
    path('login/', login_user, name='login'),
    ...
    ```

### 3. Mengimplementasi fungsi logout

- Mengimport `logout` pada `views.py` yang ada pada direktori main menggunakan `from django.contrib.auth import logout`

- Membuat sebuah fungsi baru bernama `logout_user` sebagai berikut:
    ```python
    def logout_user(request):
        logout(request)
        return redirect('main:login')
    ```
    Fungsi ini akan menghapus session dari pengguna yang sedang login menggunakan modul `logout` bawaan dari Django, kemudian langsung membawa user ke halaman login.

- Pada `urls.py` dalam direktori `main`, import fungsi `logout_user` yang baru dibuat dengan menggunakan `from main.views import logout_user`. Kemudian, pada `urlpatterns`, tambahkan path menuju halaman register.
    ```python
    ...
    path('logout/', logout_user, name='logout'),
    ...
    ```

- Pada file `main.html` yang ada dalam direktori `main`, tambahkan kode berikut ini di bagian bawah sebelum `{% endblock content %}`:
    ```html
    ...
    <a href="{% url 'main:logout' %}">
        <button>
            Logout
        </button>
    </a>
    ...
    ```
    Button ini akan memanggil url logout yang kemudian akan memanggil fungsi `logout_user`.

### 4. Membuat restriksi untuk halaman main

- Pada `views.py` yang ada dalam direktori `main`, import `login_required` menggunakan `from django.contrib.auth.decorators import login_required`, kemudian tambahkan kode berikut di atas fungsi `show_main`:
    ```python
    ...
    @login_required(login_url='/login')
    ...
    ```
    Kode ini berfungsi agar halaman main hanya dapat dibuka ketika user sudah melakukan login.

### 5. Menerapkan cookies pada halaman main

- Mengimport `HttpResponseRedirect`, `reverse`, dan `datetime` pada `views.py` yang ada pada direktori `main`. 
    ```python
    import datetime
    from django.http import HttpResponseRedirect
    from django.urls import reverse
    ```
- Pada fungsi `login_user`, ganti blok kode pada `if user is not None` menjadi kode sebagai berikut:
    ```python
    if user is not None:
        login(request, user)
        response = HttpResponseRedirect(reverse("main:show_main")) 
        response.set_cookie('last_login', str(datetime.datetime.now()))
        return response
    ```
    Fungsi ini akan melakukan login, kemudian membuat sebuah cookie last_login yang menyimpan data user yang paling terakhir login saat itu dan menambahkannya ke dalam response.

- Pada fungsi `show_main`, tambahkan `'last_login': request.COOKIES['last_login']` pada `context` sebagai berikut:
    ```python
    context = {
    'name': 'Pak Bepe',
    'class': 'PBP A',
    'products': products,
    'last_login': request.COOKIES['last_login'], #kode yang ditambahkan
    }
    ```
    Penambahan ini bertujuan untuk menambahkan informasi cookie last_login yang nantinya akan ditampilkan di `main.html`.

- Pada fungsi `logout_user`, ubah blok kodenya menjadi:
    ```python
    def logout_user(request):
        logout(request)
        response = HttpResponseRedirect(reverse('main:login'))
        response.delete_cookie('last_login')
        return response
    ```
    Penambahan ini bertujuan untuk menghapus cookie last_login saat user melakukan logout dan mengarahkan user kembali ke halaman login.

- Pada `main.html` dalam direktori `main/templates`, tambahkan kode berikut untuk menampilkan data _last login_ dari user yang sedang login:
    ```html
    ...
    <h5>Sesi terakhir login: {{ last_login }}</h5>
    ...
    ```

### 6. Menghubungkan model Item dengan User

- Pada `models.py` yang ada dalam direktori `main`, import `User` dengan menggunakan kode `from django.contrib.auth.models import User`

- Pada model `Item`, tambahkan kode untuk meng-assign `ForeignKey` sebagai berikut:
    ```python
    class Item(models.Model):
        user = models.ForeignKey(User, on_delete=models.CASCADE)
        ...
    ```
    Penggunaan ForeignKey ini ditujukan untuk mengasosiasikan sebuah object Item dengan satu orang User, semacam menunjukkan "kepemilikan" dari item tersebut.

- Pada `views.py` yang ada dalam direktori `main`, ubah blok kode pada fungsi `create_item` menjadi sebagai berikut:
    ```python
    def create_item(request):
        form = ItemForm(request.POST or None)

        if form.is_valid() and request.method == "POST":
            item = form.save(commit=False)
            item.user = request.user
            item.save()
            return HttpResponseRedirect(reverse('main:show_main'))

        context = {'form': form}
        return render(request, "create_item.html", context)
    ```
    Fungsi ini akan mengambil data item dari form yang dikirim melalui request `POST`, kemudian memproses dan menandakan "kepemilikan" dari item tersebut. Kode `item.user = request.user` menandakan bahwa user yang terasosiasikan dengan item tersebut adalah user yang sedang login (user yang mengirim request).

- Mengubah fungsi `show_main` menjadi sebagai berikut:
    ```python
    def show_main(request):
        #items = Item.objects.all()  <-- dihapus
        items = Item.objects.filter(user=request.user) #diganti dengan ini
        
        total_item = 0

        for item in items:
            total_item += item.amount

        context = {
            #'name': 'Evelyn',  <-- dihapus
            'name': request.user.username, #diganti dengan ini
            'class': 'PBP A',
            'items': items ,
            'total_item': total_item ,
        }

        return render(request, "main.html", context)
    ```
    Kode `items = Item.objects.filter(user=request.user)` berfungsi agar halaman main hanya akan menampilkan item-item yang terasosiasikan dengan user yang sedang login. Kode `'name': request.user.username` berfungsi agar nama yang ditampilkan merupakan username yang sedang login. 

- Lakukan `python manage.py makemigrations` dan `python manage.py migrate` untuk menyimpan perubahan model yang telah dilakukan.

## Apa itu Django UserCreationForm, dan jelaskan apa kelebihan dan kekurangannya?
UserCreationForm adalah sebuah modul bawaan dari Django yang dapat digunakan untuk meng-_handle_ pendaftaran user baru dalam suatu aplikasi web. Modul ini menyediakan tiga field data akan di-_assign_ ke seorang user, yaitu field username, password1, dan password2 (field password2 ini digunakan untuk konfirmasi terhadap isi dari password1). Kelebihan dari UserCreationForm ini adalah praktis untuk digunakan sebagai sistem registrasi sehingga developer tidak perlu membuat sistem regitrasi user dari _scratch_. UserCreationForm juga sudah disertai dengan validasi username dan password sehingga developer tidak harus mengatur validasi ini secara manual. Kekurangannya adalah field yang disediakan pada modul ini sangat terbatas. Jika kita ingin menambahkan field, kita harus meng-_extend_ UserCreationForm tersebut dengan sebuah class berisi form yang sesuai dengan field yang ingin ditambahkan. 

## Apa perbedaan antara autentikasi dan otorisasi dalam konteks Django, dan mengapa keduanya penting?
Autentikasi adalah suatu proses validasi untuk memferivikasi identitas dari user yang sedang berinteraksi dengan sistem. Proses ini memastikan siapa yang sedang berinteraksi dengan sistem berdasarkan data-data yang dapat divalidasi seperti username, password, PIN, dll.

Berbeda dengan autentikasi, otorisasi dijalankan setelah proses autentikasi. Otorisasi adalah proses di mana sistem akan memberikan batasan akses yang sesuai kepada user yang sudah lolos proses autentikasi. Sistem akan menentukan apakah user tersebut dapat mengakses suatu sumber tertentu.

Baik autentikasi maupun otorisasi merupakan hal yang penting untuk menjaga keamanan data dalam suatu sistem, terutama data yang bersifat pribadi atau rahasia. Tanpa keduanya, suatu sistem akan rentan untuk mengalami kebocoran data akibat akses data tanpa izin dari pihak yang semestinya tidak memiliki hak akses. Kebocoran data dapat mengakibatkan adanya orang lain yang menggunakan data korban tanpa izin untuk hal-hal yang berbahaya tanpa sepengetahuan korban. Misalnya, pembobolan rekening, terorisme, dan penyamaran kejahatan atas nama korban seperti pembelian barang ilegal atas nama korban.

## Apa itu cookies dalam konteks aplikasi web, dan bagaimana Django menggunakan cookies untuk mengelola data sesi pengguna?
Cookies merupakan sejumlah kecil data yang digunakan oleh browser untuk mengidentifikasi seorang user. Cookies akan membantu browser untuk "mengingat" seorang user dengan cara menyimpan beberapa data terkait apa yang dilakukan oleh user tersebut, seperti misalnya kapan terkahir kali user tersebut melakukan login, riwayat pencarian user, preferensi user terkait konten yang sering dicarinya, alamat email user, dan lain-lain. Setiap cookie disimpan dalam bentuk pasangan _key-value_.

Pada django, kita bisa melakukan _set_ pada cookies dengan menggunakan method `set_cookie()` dan kita bisa mendapatkan value dari sebuah cookie dengan menggunakan method `request.COOKIES['key']`. Cara kerja cookies pada django adalah sebagai berikut:
- Ketika HTTP request dikirimkan kepada server oleh browser, server akan memberikan response beserta dengan cookie.
- Cookie tersebut diterima oleh browser dan disimpan.
- Sekarang, setiap kali HTTP request dikirimkan kepada server, browser akan mengirimkan kembali cookie tersebut kepada server hingga cookie tersebut "kadaluarsa".
- Saat cookie sudah "kadaluarsa", cookie tersebut akan dihapus dari browser.  

## Apakah penggunaan cookies aman secara default dalam pengembangan web, atau apakah ada risiko potensial yang harus diwaspadai?
Dalam pengembangan web, cookies tidak selalu aman. Ada beberapa risiko serangan terdahap cookies yang harus diwaspadai. Terkadang, cookies menyimpan beberapa informasi pribadi yang cukup sensitif. Apabila cookies tidak di-_handle_ dengan baik, maka cookies akan rawan terhadap akses tanpa izin yang dilakukan oleh pihak tak bertanggungjawab untuk mencuri data di dalamnya. Salah satu contoh serangan yang harus diwaspadai adalah _cookie poisoning_. _Cookie poisoning_ adalah suatu tindakan memanipulasi cookie agar pelaku bisa mendapatkan akses tanpa otorasasi ke akun seorang user, kemudian mencuri identitas atau data-data milik user tersebut.

</details>

<details>
<summary>Tugas 5</summary>
<br>

# Tugas 5

## Jelaskan manfaat dari setiap element selector dan kapan waktu yang tepat untuk menggunakannya.

### 1. Element Selector
   Selector ini akan memilih semua elemen yang memiliki _tag_ yang sama dan mengubah propertinya berdasarkan _style_ yang diterapkan. Cocok digunakan ketika kita ingin mengubah properti dari semua element dengan _tag_ yang sama secara sekaligus tanpa membeda-bedakan tiap elemen tersebut.

### 2. Class Selector
   Selector ini akan memilih semua elemen dalam suatu class yang sama dan mengubah propertinya berdasarkan _style_ yang diterapkan. Cocok digunakan ketika kita hanya ingin mengubah properti dari sekelompok elemen tertentu yang dikelompokkan dalam sebuah class. _Style_ yang diterapkan pada class ini tidak akan mempengaruhi elemen lain yang berada di luar class.

### 3. ID Selctor
   Selector ini akan memilih elemen dengan ID tertentu dan mengubah propertinya berdasarkan _style_ yang diterapkan. Cocok digunakan ketika kita hanya ingin mengubah properti dari satu buah elemen yang kita pilih secara spesifik. Satu elemen dapat memiliki satu buah ID yang unik, satu ID yang sama tidak dapat diberikan pada lebih dari satu elemen. Kita dapat memberikan suatu ID khusus pada elemen-elemen yang ingin dipilih dan CSS akan mengenali elemen-elemen tersebut berdasarkan ID yang kita berikan.

## Jelaskan HTML5 Tag yang kamu ketahui.

   1. ```<nav>``` : digunakan untuk mendefinisikan link navigasi yang mengarah pada halaman-halaman lain dalam suatu web.
   2. ```<audio>``` : digunakan untuk meng-_embed_ suatu konten berupa audio seperti MP3, WAV, atau OGG dengan source tertentu.
   3. ```<video>``` : digunakan untuk meng-_embed_ suatu konten berupa video seperti MP4, WebM, atau OGG dengan source tertentu.
   4. ```<header>``` : menandakan header dari suatu halaman web berupa suatu _container_ yang biasanya berisi informasi terkait "pengenalan" dari konten-konten yang terkait.
   5. ```<footer>``` : menandakan header dari suatu halaman web berupa suatu _container_ yang biasanya berisi informasi tambahan terkait web tersebut.

## Jelaskan perbedaan antara margin dan padding.
   Margin dan padding sama-sama merupakan cara mengatur ruang atau jarak terhadap suatu elemen. Perbedaannya adalah margin akan mengatur ruang di **sisi luar** elemen, sedangkan padding akan mengatur ruang di **sisi dalam** elemen. Margin biasanya digunakan ketika kita ingin mengatur jarak antara suatu elemen dengan elemen lain. Padding digunakan ketika kita ingin mengatur jarak atau ruang antara konten di dalam suatu elemen dengan "pinggiran" dari elemen tersebut.

## Jelaskan perbedaan antara framework CSS Tailwind dan Bootstrap. Kapan sebaiknya kita menggunakan Bootstrap daripada Tailwind, dan sebaliknya?
   Bootstrap merupakan framework yang menyediakan template, class CSS, dan komponen yang telah dirancang sebelumnya dan "siap pakai" untuk membuat design tampilan UI. Berbeda dengan bootstrap, Tailwind merupakan framework yang menyediakan kelas-kelas utilisasi untuk design tampilan UI, di mana penggunaannya masih memerlukan kustomisasi dari developer yang menggunakannya. Bootstrap cocok digunakan ketika kita ingin mengutamakan stabilitas komponen dan kemudahan penggunakan, karena komponen-komponen yang kita gunakan dari bootstrap akan disesuaikan dengan design yang sudah dirancang "dari sananya", di mana design ini konsisten untuk seluruh komponen. Tailwind cocok digunakan jika kita ingin mengutamakan kustomisasi design dengan kombinasi class utilitas yang spesifik, di mana class-class utilitas ini telah disediakan oleh Tailwind. 

## Langkah pengerjaan
   Menambahkan tag ```<style>``` di bawah ```{% block content %}``` pada setiap file html di dalam direktori ```main/templates```, kemudian membuat design tampilan halaman web di dalam tag ```<style>``` tersebut menggunakan css sesuai keinginan.

</details>

<details>
<summary>Tugas 6</summary>
<br>

# Tugas 6

## Perbedaan antara asynchronous programming dengan synchronous programming.
   Pada synchronus programming, program akan dijalankan secara berurutan, yang berarti apabila suatu fungsi/proses akan dijalankan, maka program harus menunggu fungsi/proses sebelumnya selesai dijalankan. Sedangkan para asynchronus programming, program dapat menjalankan lebih dari satu fungsi/proses secara bersamaan, yang berarti suatu fungsi/proses dapat dijalankan tanpa harus menunggu fungsi/proses sebelumnya selesai.  

## Paradigma event-driven programming
   Merupakan suatu paradigma di mana suatu proses akan dijalankan oleh program sebagai respons dari suatu kejadian atau event tertentu yang sedang terjadi. Event yang dimaksud antara lain ketika suatu button di-klik, ketika suatu button di-hover, ketika key tertentu pada keyboard ditekan, ketika suatu fungsi dipanggil oleh fungsi lain, dll.

   Salah satu contoh penerapannya pada tugas ini adalah function yang memunculkan modal "Add New Item" ketika button "Add Item" di-klik. Button "Add Item" dihubungkan dengan function openModal() dengan menggunakan `document.getElementById("open-modal-button").onclick = openModal`.

## Penerapan asynchronous programming pada AJAX
   AJAX menerapkan asynchronus programming ketika mengirim data ke server maupun ketika meminta/request data dari server. Dengan AJAX, halaman web dapat memperbaharui data secara asynchronus dan request yang diterima oleh server akan diproses pada _background_. Tahapan cara kerja AJAX adalah sebagai berikut:

   1. Browser akan membuat sebuah JavaScript call yang kemudian mengaktifkan XMLHttpRequest.
   2. Pada background, browser akan mengirimkan HTTP request ke server.
   3. Server akan menerima request, mengambil data, dan mengirimkan kembali data tersebut ke browser.
   4. Browser akan menerima data dari server, kemudian memproses data tersebut menggunakan JavaScript, dan data tersebut langsugn ditampilkan pada halaman web tanpa harus _reload_ terlebih dahulu.

## Perbandingan Fetch API dan library jQuery
   Fetch API cenderung lebih ringan dan fleksibel, Fetch API hanya fokus pada pembuatan HTTP request dan penanganan respons, serta lebih fleksibel dalam meng-_handle_ berbagai jenis data. Fetch API sendiri juga merupakan bagian dari JavaScript yang tidak memerlukan library atau dependencies tambahan. Berbeda dengan Fetch API, library jQuery merupakan library JavaScript yang menyediakan berbagai fitur dan utility untuk meng-_handle_ data. jQuery menyediakan banyak fitur untuk hal-hal yang dilakukan dalam web development di luar pembuatan HTTP request. Kode yang digunakan dalam jQuery cenderung lebih sederhana dan mudah dibaca. Baik Fetch API maupun jQuery memiliki kelebihannya masing-masing dan digunakan sesuai kebutuhan yang berbeda. Jika kita ingin membuat aplikasi web yang cenderung mementingkan "keringanan" dalam membuat HTTP request, kita bisa menggunakan Fetch API. Sedangkan jika kita ingin membuat sebuah browser dengan banyak fitur dan ingin menggunakan library yang menyediakan beragam fitur untuk _handle_ data di luar pembuatan HTTP request, kita bisa menggunakan library jQuery.

## Langkah-langkah pengerjaan

### 1. Ubah kode cards data item agar dapat mendukung AJAX GET.
- Pada `views.py` dalam direktori `main`, buat sebuah fungsi dengan nama `get_item_json` sebagai berikut:
    ```python
    def get_item_json(request):
        item = Item.objects.filter(user=request.user)
        return HttpResponse(serializers.serialize('json', item))
    ```
   Kemudian, pada `urls.py`, import `get_item_json` dan tambahkan routing untuk fungsi tersebut pada `urlpatterns` sebagai berikut: `path('get-item/', get_item_json, name='get_item_json'),`

- Pada `main.html` dalam direktori `main/templates`, tambahkan `<div class="card-container" id="card-container"></div>` sebagai struktur tempat menyusun cards. Kemudian, buat block `<script>` dan buat function `getItems()` dalam block tersebut sebagai berikut:
    ```javascript
    <script>
    async function getItems() {
        return fetch("{% url 'main:get_item_json' %}").then((res) => res.json())
    }
    </script>
    ```

- Menambahkan fungsi `refreshItems()` pada block `<script>` untuk menampilkan card setiap item sebagai berikut:
    ```javascript
    ...
    async function refreshItems() {
        document.getElementById("card-container").innerHTML = ""
        const items = await getItems()
        let htmlString = ``
        items.forEach((item) => {
            htmlString += `\n<div class="cards">
                <div class="front">
                    <div class="front-content">
                        <div class="title"><h2>${item.fields.name}</h2></div>
                        <div class="amount"><h3>Amount: ${item.fields.amount}</h3></div>
                        <div class="date_added"><p>Added:<br>${item.fields.date_added}</p></div>
                    </div>
                </div>
                <div class="back">
                    <div class="desc"><p>${item.fields.description}</p></div>
                </div>
                </div>`
        })
        
        document.getElementById("card-container").innerHTML = htmlString
    }

    refreshItems()
    ...
    ```

### 2. Membuat modal dengan form untuk menambahkan item
- Pada `views.py` dalam direktori `main`, import `from django.views.decorators.csrf import csrf_exempt`.
- Buat sebuah fungsi baru bernama `add_item_ajax` yang dapat menerima request, sebagai berikut:
    ```python
    def add_item_ajax(request):
        if request.method == 'POST':
            name = request.POST.get("name")
            amount = request.POST.get("amount")
            description = request.POST.get("description")
            date_added = request.POST.get("date_added")
            user = request.user

            new_item = Item(name=name, amount=amount, description=description, date_added = date_added, user=user)
            new_item.save()

            return HttpResponse(b"CREATED", status=201)

        return HttpResponseNotFound()
    ```
- Tambahkan dekorator `@csrf_exempt` di atas fungsi `add_item_ajax`. Kemudian, pada `urls.py`, import `add_item_ajax` dan tambahkan routing untuk fungsi tersebut pada `urlpatterns` sebagai berikut: `path('create-ajax/', add_item_ajax, name='add_item_ajax')`.

- Pada `main.html`, buat sebuah modal untuk menambahkan item sebagai berikut:
    ```html
    <div class="modal-cont" id="modal-cont">
    <div class="modal" id="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h1>Add New Item</h1>
            </div>
            <div class="modal-body">
                <form id="form" onsubmit="return false;">
                    {% csrf_token %}
                    <div class="mb-3">
                        <label for="name" class="col-form-label">Name:</label>
                        <input type="text" class="form-control" id="name" name="name"></input>
                    </div>
                    <div class="mb-3">
                        <label for="price" class="col-form-label">Amount:</label>
                        <input type="number" class="form-control" id="amount" name="amount"></input>
                    </div>
                    <div class="mb-3">
                        <label for="description" class="col-form-label">Description:</label>
                        <textarea class="form-control" id="description" name="description"></textarea>
                    </div>
                </form>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" id="close-modal">Close</button>
                <button type="button" class="btn btn-primary" id="button_add" data-bs-dismiss="modal">Add Item</button>
            </div>
        </div>
    </div>
    </div>
    ```
- Tambahkan button untuk menampilan modal sebagai berikut:
    ```html
    <div class="add-btn-cont">
        <button class="add-item-button" id="open-modal-button">Add Item</button>
    </div>
    ```
- Pada block `<script>` yang ada dalam `main.html`, tambahkan fungsi dengan nama `addItem` sebagai berikut:
    ```javascript
    function addItem() {
        fetch("{% url 'main:add_item_ajax' %}", {
            method: "POST",
            body: new FormData(document.querySelector('#form'))
        }).then(refreshItems)

        document.getElementById("form").reset()
        modal.classList.remove("open-modal")
        return false
    }
    ```
    Kemudian, tambahkan fungsi `openModal()` dan `closeModal()` untuk menampilkan dan menutup modal sebagai berikut:
    ```javascript
    let modal = document.getElementById("modal-cont");
    
    function openModal(){
        modal.classList.add("open-modal");
    }

    function closeModal(){
        document.getElementById("form").reset()
        modal.classList.remove("open-modal");
    }
    ```
- Sambungkan button "Add Item" dan button-button pada modal dengan fungsi yang sesuai sebagai berikut:
    ```javascript
    document.getElementById("open-modal-button").onclick = openModal
    document.getElementById("close-modal").onclick = closeModal
    document.getElementById("button_add").onclick = addItem
    ```

### 3. Melakukan perintah collectstatic
- Pada `settings.py` dalam direktori project `weird_wired`, tambahkan `import os` dan `STATIC_ROOT = os.path.join(BASE_DIR, 'static')`
- Jalankan perintah `python manage.py collectstatic` pada terminal (pastikan path terminal berada pada path direktori utama project).

</details>
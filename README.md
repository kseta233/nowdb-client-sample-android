
This tutorial is for using nowdb in android, based on
https://github.com/kseta233/nowdb-client

## [Android](http://developer.android.com/training/index.html)

Secara Konseptual peranan nowdb dan mobile apps di android sbb:
```sequence
UI->Networking Lib: event
Networking Lib->nowdb: http call
nowdb->nowdb: validate data
nowdb->Networking Lib: http response
Networking Lib->UI: display data
```
untuk Android, tutorial akan menggunakan Retrofit untuk networking libnya

Untuk tutorial mengkonsumsi API nowdb pada android, kita akan menggunakan base project sederhana yang sudah dibuat sbb:

**Package**:
[![projectBaseAndroid.png](https://s22.postimg.org/dwi09psq9/project_Base_Android.png)](https://postimg.org/image/a04odq7ql/)

- **constant**, untuk menampung data setting constant string untuk RestAPI
- **model**, untuk menampung hasil balikan dari response API
- **service**, untuk membuat APIInterface retrofit
- **util**, untuk membuat fungsi2 statis
- **view**, untuk menampung class Activity/Fragment yang digunakan dan bertanggung jawab atas interaksi dengan pengguna

**Gradle**

[![gradleAndroid.png](https://s18.postimg.org/9r7f8j02x/gradle_Android.png)](https://postimg.org/image/rh93tkdnp/)

Karena NowDB Pure merupakan API provider, maka kita tinggal mengkonsumsinya menggunakan Retrofit

### [Retrofit](http://square.github.io/retrofit/)

Retrofit adalah Rest Client untuk android dan java dari squareup. Pada tutorial ini kita akan membuat aplikasi android untuk menjelaskan penggunaan HTTP GET REQUEST menggunakan library retrofit. ([source](http://teknorial.com/tutorial-parsing-data-json-dengan-retrofit/))

Tutorial ini tidak akan membahas banyak soal retrofit. Retrofit yang digunakan adalah Retrofit 2.0

ketahui lebih lanjut mengenai retrofit di http://square.github.io/retrofit/

### Mengkonsumsi API Now DB di Android
pada sesi ini, akan dijelaskan step by step dari


1. [melakukan setup NowDB](#setup-nowdb)
2. [menambahkan data (Create)](#create-versi-android)
3. [mengupdate data (Update)](#update-versi-android)
4. [membaca data (Read)](#read-versi-android)
5. [menghapus data (Delete)](#delete-versi-android)


#### Setup NowDB [<i class="icon-right"></i> ](#mengkonsumsi-api-now-db-di-android)

##### **Daftar NowDB**
Buat/daftar akun kamu ke [nowdb.net](http://nowdb.net/)

[![nowdbRegister.png](https://s18.postimg.org/gwwqtn1yh/nowdb_Register.png)](https://postimg.org/image/j1h3uq3l1/)

> Note: Kembali ke Tutorial iOS klik disini [klik disini](#ios)


##### **Membuat Project**

[![nowdbAddProject.png](https://s22.postimg.org/hkm5lw3cx/nowdb_Add_Project.png)](https://postimg.org/image/wtc2znx19/)

Project pada nowdb, dapat diartikan seperti DB yang dipakai.

1. Klik Project
2. Klik Create New Project. Untuk tutorial ini project dinamakan "nownews"
3. Setelah Project terbentuk, maka Project akan terdapat di list project

> Note: Kembali ke Tutorial iOS klik disini [klik disini](#ios)


##### **Membuat Collection**

Jika Project merupakan DB, maka collection dapat diartikan sebagai tabel yang akan digunakan pada aplikasi

1. klik project yang sudah dibuat pada langkah sebelumnya,
2. klik Create New Collection. Untuk tutorial ini field dinamakan "news"
3. Collection yang baru dibuat, akan terdapat pada list collection

Layaknya tabel pada DB umumnya, maka tabel mempunyai field, sebelum dapat digunakan. Untuk  membuat Field, ikuti langkah berikut

1. Pada list Collection yang baru dibuat, klik field
2. Create New Field
> Note: Kembali ke Tutorial iOS klik disini [klik disini](#ios)


### **Mengaktifkan API now DB**
Sebelum dapat menggunakan API Now DB, maka Project harus diaktifkan terlebih dahulu

1. Pada list project, pilih project yang akan diaktifkan API nya
2. Klik API configuration
3. Pada halaman ini, kita akan mendaftarkan aplikasi kita
4. Setelah mendaftarkan aplikasi, maka kita akan mendapatkan app id
5. Untuk sebuah appid, kita dapat mengatur API/endpoint yang aktif dengan menekan tombol API. Tutorial ini menggunakan API v2
6. API yang aktif baru dapat digunakan oleh user

Tujuan step ini adalah mendapatkan app id dan token, yang selalu digunakan untuk memanggil API now DB

> Note: Kembali ke Tutorial iOS klik disini [klik disini](#ios)


#### **Mempersiapkan Koding Android**

**CONSTANT**
Class ini hanya digunakan sebagai constant untuk merangkai base API dan menyimpan App ID serta Token yang sudah digenerate dari nowdb
```java
public class RestAPI {
private RestAPI(){
}

public static final String BASE_API_URL = "http://io.nowdb.net";
public static final String BASE_API_VERSION = "v2";
public static final String API_URL = BASE_API_URL + "/" + BASE_API_VERSION + "/";

//SPECIAL FOR NOWDB
public static final String NOWDB_TOKEN = "538d67698d909eca7dfb20a3";
public static final String NOWDB_APPID = "57ca510c1f6d04fe523f718a";
public static final String NOWDB_PROJECT= "testnowdb";
public static final String DUMMY_ID = "57ca77851f6d0401533f71a8";

//FOR COLLECTIONS
public class Collection{
public static final String TODO = "todo";
}
}
```
**MODEL**
model response yang harus disiapkan menggunakan http://www.jsonschema2pojo.org/ setelah mendapatkan testing response dengan cara testing seperti di doc.nowdb.net, kita kemudian akan membuat class model dari JSONSchema POJO,

penampakannya kira-kira sbb:
``` java
public class Todo {

@SerializedName("action")
@Expose
private String action;
@SerializedName("time")
@Expose
private String time;
@SerializedName("mark")
@Expose
private String mark;
@SerializedName("id")
@Expose
private String id;
//constructor ...
//setter getter ...
//override toString ...
@Override
public String toString() {
return "[Todo = id:"+id+" , action:"+action+" , time: "+time+" , mark: "+mark+" ]";
}
}
```
**SERVICE**
Pada package service buatlah interface untuk http retrofit
```java
import retrofit2.Call;
import retrofit2.Response;
import retrofit2.http.DELETE;
import retrofit2.http.Field;
import retrofit2.http.GET;
import retrofit2.http.POST;
import retrofit2.http.PUT;
import retrofit2.http.Path;

public interface NowDbAPIInterface {
//@TODO: create NOWDBInterface
//Create : Make Create Interface

//Read : Make Read Interface

//Update : Make Update Interface

//Delete : Make Delete Interface
}
```
> Note : Apabila ingin melakukan costumisasi, bisa dilakukan di override callbacknya Namun tidak akan dibahas pada tutorial ini.

**VIEW**
Pada package view, di class yang terdapat Activity, lakukan inisialisasi retrofit, dengan membuat method

``` java

import butterknife.ButterKnife;
import okhttp3.OkHttpClient;
import okhttp3.logging.HttpLoggingInterceptor;
import retrofit2.Call;
import retrofit2.Callback;
import retrofit2.Response;
import retrofit2.Retrofit;
import retrofit2.converter.gson.GsonConverterFactory;

public class MainActivity extends AppCompatActivity{
private Retrofit retrofit;
protected void onCreate(Bundle savedInstanceState) {
...
initializeRetrofit();
...
}

/**
* method to initialize retrofit in current activity
*/
private void initializeRetrofit(){
HttpLoggingInterceptor logging = new HttpLoggingInterceptor();
logging.setLevel(HttpLoggingInterceptor.Level.BODY);
OkHttpClient.Builder httpClient = new OkHttpClient.Builder();
httpClient.addInterceptor(logging);  // <-- for loggin purpose

//initialize retrofit
retrofit = new Retrofit.Builder()
.baseUrl(RestAPI.API_URL)
.addConverterFactory(GsonConverterFactory.create())
.client(httpClient.build())
.build();

}
}

```

> Note: cara inisialisasi bisa memakai design pattern lain, untuk tutorial ini, dibuat sesimple mungkin untuk melakukan simulasi cara memakai nowdb


#### **Create versi Android**[<i class="icon-right"></i> ](#mengkonsumsi-api-now-db-di-android)

siapkan interface berikut di file NowDBAPIInterface
``` java
//POST : io.nowdb.net/v2/insert
@POST("insert")
Call<Todo> inserTodo(
@Field("token") String token,
@Field("project") String project,
@Field("collection") String collection,
@Field("appid") String appid,

@Field("action") String action,
@Field("time") long milis,
@Field("mark") boolean mark
);
```
> Note : Karena nowdb menggunakan request body yang dibaca, maka semua disatukan ke request body, tidak di header

Kemudian di implementasi UI, panggil method ini dengan data yang sudah ada
``` java
public void postData(String action, long milis,boolean mark){
//call interface
NowDbAPIInterface apiService = retrofit.create(NowDbAPIInterface.class);
//implement method
Call<Todo> result = apiService.inserTodo(
RestAPI.NOWDB_TOKEN,
RestAPI.NOWDB_PROJECT,
RestAPI.Collection.TODO,
RestAPI.NOWDB_APPID,

action,
milis,
mark
);
//get result callback
result.enqueue(
new Callback<Todo>() {
@Override
public void onResponse(Call<Todo> call, Response<Todo> response) {

}

@Override
public void onFailure(Call<Todo> call, Throwable t) {

}
}
);
}
```

#### **Update versi Android**[<i class="icon-right"></i> ](#mengkonsumsi-api-now-db-di-android)

untuk update nowDB menggunakan method put.
Seperti Create, kita akan membuat interface dan implementasi di view
``` java
@PUT("update_id/token/{token}/project/{project}/collection/{collection}/appid/{appid}/id/{id}")
Call<Response> updateTodo(
@Path("token") String token,
@Path("project") String project,
@Path("collection") String collection,
@Path("appid") String appid,
@Path("id") String id,

// untuk perubahan objectnya
@Field("action") String action,
@Field("time") long milis,
@Field("mark") boolean mark
);
```

Kemudian di implementasi UI, panggil method ini dengan data yang sudah ada
``` java
public void updateData(String id, String action, long milis,boolean mark){
//call interface
NowDbAPIInterface apiService = retrofit.create(NowDbAPIInterface.class);
//implement method
Call<Todo> result = apiService.updateTodo(
RestAPI.NOWDB_TOKEN,
RestAPI.NOWDB_PROJECT,
RestAPI.Collection.TODO,
RestAPI.NOWDB_APPID,
id,
action,
milis,
mark
);
//get result callback
result.enqueue(
// do something to result
);
}
```


#### **Read versi Android**[<i class="icon-right"></i> ](#mengkonsumsi-api-now-db-di-android)

untuk read, akan dibagi menjadi 2, read 1 object dan read banyak object.
Saat mengambil read object dan list object, hendaknya kita berhati-hati sedikit. Karena kembalian dari nowdb selalu array, meskipun objectnya hanya 1

##### **Read One**

Untuk read 1 object, kita harus siapkan penampung hasil list karena sifat nowdb
``` java
@GET("select_id/token/{token}/project/{project}/collection/{collection}/appid/{appid}/id/{id}")
Call<List<Todo>> getTodo(
@Path("token") String token,
@Path("project") String project,
@Path("collection") String collection,
@Path("appid") String appid,
@Path("id") String id
);
```
> Note : Karena nowdb menggunakan request body yang dibaca, maka semua disatukan ke request body, tidak di header

Kemudian di implementasi UI, panggil method ini dengan data yang sudah ada
``` java
public void getData(){
//call interface
Call<List<Todo>> result = apiService.getAllTodo(
RestAPI.NOWDB_TOKEN,
RestAPI.NOWDB_PROJECT,
RestAPI.Collection.TODO,
RestAPI.NOWDB_APPID
);

//get result callback
result.enqueue(
// DO SOMETHING to result
);
}
```

##### **Read All**

siapkan interface berikut di file NowDBAPIInterface
``` java
@GET("select_all/token/{token}/project/{project}/collection/{collection}/appid/{appid}")
Call<List<Todo>> getAllTodo(
@Path("token") String token,
@Path("project") String project,
@Path("collection") String collection,
@Path("appid") String appid
);
```
> Note : Karena nowdb menggunakan request body yang dibaca, maka semua disatukan ke request body, tidak di header

Kemudian di implementasi UI, panggil method ini dengan data yang sudah ada
``` java
public void getAllData(){
//call interface
public void getData(){
//call interface
Call<List<Todo>> result = apiService.getAllTodo(
RestAPI.NOWDB_TOKEN,
RestAPI.NOWDB_PROJECT,
RestAPI.Collection.TODO,
RestAPI.NOWDB_APPID
);

//get result callback
result.enqueue(
// DO SOMETHING to result
);
}
```

#### **Delete versi Android**[<i class="icon-right"></i> ](#mengkonsumsi-api-now-db-di-android)

siapkan interface berikut di file NowDBAPIInterface
``` java
@DELETE("remove_id/token/{token}/project/{project}/collection/{collection}/appid/{appid}/id/{id}")
Call<Response> deleteTodo(
@Path("token") String token,
@Path("project") String project,
@Path("collection") String collection,
@Path("appid") String appid,
@Path("id") String id
);
```
> Note : Karena nowdb menggunakan request body yang dibaca, maka semua disatukan ke request body, tidak di header

Kemudian di implementasi UI, panggil method ini dengan data yang sudah ada
``` java
public void postData(String id){
//call interface
NowDbAPIInterface apiService = retrofit.create(NowDbAPIInterface.class);
//implement method
Call<Todo> result = apiService.deleteTodo(
RestAPI.NOWDB_TOKEN,
RestAPI.NOWDB_PROJECT,
RestAPI.Collection.TODO,
RestAPI.NOWDB_APPID,
id
);
//get result callback
result.enqueue(
// do something with callback
);
}
```

### **Source Code**

seluruh Source Code yang ada di tutorial ini, dapat diakses di :

https://github.com/kseta233/nowdb-client-sample-android



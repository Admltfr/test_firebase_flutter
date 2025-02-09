## Daftar Isi

- [Cloud Storage](#cloud-storage)
  - [Stream](#stream)
  - [Snapshot](#snapshot)
  - [Stream Builder](#stream-builder)
  - [Firebase](#firebase)
    - [Cloud Firestore](#firestore)
      - [Read Data](#read)
      - [Add Data](#add)
      - [Update Data](#update)
      - [Delete Data](#delete)

## Cloud Storage

- **Cloud Storage** merupakan layanan penyimpanan data yang dapat diakses melalui internet dan biasanya disimpan di server pihak ketiga.
- **Cloud Storage** sangat berguna sebagai tempat penyimpanan data atau bahkan database terutama yang memiliki kapasitas yang besar karena batas penyimpanannya bisa tak terbatas dan lebih aman, namun hanya bisa diakses melalui internet.

### Stream

- **Stream** adalah fungsi asinkron yang dieksekusi berurutan layaknya sungai, yang sedikit berbeda dari Future yang hanya bisa mengembalikan satu data saja, Stream akan selalu mengalirkan data tersebut dan akan selalu mengubah datanya setiap kali ada perubahan hingga bisa dibilang dapat update data secara Real-Time.

### Snapshot

- **Snapshot** merupakan data yang dialirkan oleh stream yang memiliki 2 bagian, yaitu ConnectionState dan data yang dimana connection state menyatakan status koneksi dan data sebagai data yang terkandung didalamnya

### StreamBuilder

- **StreamBuilder** merupakan widget di framework flutter yang digunakan membuat suatu widget dengan data yang didapat dari snapshot yang dijalankan menggunakan Stream, widget akan selalu berubah setiap kali data berubah.
- Untuk Implementasinya dapat dilihat pada bagian [Read](#read)

### Firebase

- **Firebase** adalah platform pengembangan aplikasi dari google yang biasanya digunakan untuk pengelolaan dan pertukaran data, dan beberapa fitur lainnya.
- **Firebase** memiliki sistem penyimpanan awan/cloud storage yaitu **Cloud Firestore** yang akan digunakan pada project ini [https://firebase.google.com/docs/cli].

#### Cloud Firestore

- **Cloud Firestore** merupakan fitur yang ditawarkan oleh firebase untuk membuat semacam basis data/database NoSql yang dapat dikelola sendiri oleh user pemiliki database dan layanan ini juga kompatibel dengan format JSON.
- Seperti biasa, semua sistem pengelolaan data selalu memiliki 4 fungsi utama yaitu
  - Create/Add
    - Yaitu fungsi yang digunakan untuk membuat/menambahkan data
  - Read/View
    - Yaitu fungsi yang digunakan untuk membaca/melihat data
  - Update/Edit
    - Yaitu fungsi yang digunakan untuk membuat perubahan/pembaharuan pada data
  - Delete
    - Yaitu fungsi yang digunakan untuk menghapus data
- **Cloud Firestore** dapat diintegrasikan ke framework flutter dengan beberapa prosedur yang harus dilakukan [https://firebase.google.com/docs/flutter/setup?platform=ios].
- Disini istilah collection ibaratnya seperti suatu table dalam database, dan document seperti entitas / data didalamnya.

##### Read

- **Read** merupakan fungsi untuk membaca data, dan untuk flutter sendiri terdapat suatu widget khusus yang digunakan untuk membaca data dari database yaitu StreamBuilder seperti yang telah dijelaskan pada bagian awal.

```dart
StreamBuilder(
	stream:  FirebaseFirestore.instance
		.collection('collection')
		.snapshots(),
	builder: (context, snapshot) { return Widget()}
)
```

> Ini merupakan sintaks yang digunakan untuk mendapatkan data dari Document firestore milik saya

- Selanjutnya setelah membaca dan mengekstrak datanya dapat digunakan

```dart
snapshot.data!.docs[0].data()
```

> method data() digunakan untuk mengubah format datanya dari JsonQuery menjadi tipe data asli data, dan berhubung datanya selalu berbentuk list sehingga digunakan perulangan didalam StreamBuildernya.

##### Add

- **Add** merupakan fungsi untuk menambahkan data

```dart
void  handleCreateTodo() async {
	final  newTodo  = {
		'databool':  false,
		'datastring':  _textEditingController.text,
		'datatime':  DateTime.now()
	};
	final  db  =  FirebaseFirestore.instance;
	await  db.collection('collection').add(newTodo);
	_textEditingController.text  =  '';
}
```

> Kode diatas merupakan suatu contoh penambahan data menggunakan firestore

- Seperti yang terlihat terdapat beberapa sintaks Asynchronous Programming yaitu async-await atau bisa dibilang suatu fungsi ini sudah pasti harus menunggu koneksi dari cloud storage terlebih dahulu yaitu firestore
- Penambahan data secara default biasanya akan menginisiasi semua field data pada document hingga perlu diperhatikan penamaan dan isi default dari datanya.
- Untuk informasi lebih lanjut bisa lihat disini [https://firebase.google.com/docs/firestore/manage-data/add-data].

##### Update

- **Update** merupakan fungsi yang digunakan untuk mengubah/memperbaharui data

```dart
void  handleToggleTodo(String  id, bool  status) async {
	final  editTodo  = {'databool':  !status};
	final  db  =  FirebaseFirestore.instance;
	await  db.collection('collection').doc(id).update(editTodo);
}
```

> Diatas merupakan contoh kode untuk melakukan update data pada cloud firestore terlihat ada beberapa perbedaan dari Adddata karena disini memerlukan penanda id untuk data yang akan diupdate serta data baru yang terupdate, untuk variasi kode bisa dilihat di [https://firebase.google.com/docs/firestore/manage-data/add-data#update-data];

##### Delete

- **Delete** merupakan fungsi untuk menghapus data

```dart
void  handleDeleteTodo(String  id) async {
	final  db  =  FirebaseFirestore.instance;
	await  db.collection('todos').doc(id).delete();
}
```

> Diatas terlihat fungsi delete merupakan fungsi dengan sintaks paling simpel, yang hanya memiliki parameter id, namun ini hanya berlaku untuk penghapusan collection untuk variasi lainnya dapat dilihat pada dokumentasi [https://firebase.google.com/docs/firestore/manage-data/delete-data].

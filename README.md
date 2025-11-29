# Pertemuan 10
Halo, sebelumnya nama saya Ahmad Rizky Pramudia Pratama dengan NIM 312410272 Kelas TI.24.A4
Disini saya akan mengimplementasikan sebuah database tentang data barang dengan menggunakan MySQL

Pertama kita buat dahulu file untuk databasenya nanti, saya namakan lab8_php_database, untuk lokasi
filenya terletak di xampp/htdocs/lab8_php_database

<img src="Lab8/Langkah 1.png" alt="Tutorial" width="400">

Setelah itu, kalian perlu buka xampp dulu, lalu Start saja bagian MySQL dan Apache nya sampe seperti ini

<img src="Lab8/Langkah 2.png" alt="Tutorial" width="400">

lalu kalian jalankan web ini
``` http://localhost:8080/phpmyadmin ```

<img src="Lab8/Langkah 3.png" alt="Tutorial" width="400">

Kemudian kalian ke menu SQL, lalu tulis perintah ini

<img src="Lab8/Langkah 4.png" alt="Tutorial" width="400">

Maka otomatis kalian akan membuat database baru, yaitu latihan1;
Kemudian kalian pilih database latihan1;, lalu jalankan SQL dan tulis perintah ini

<img src="Lab8/Langkah 5.png" alt="Tutorial" width="400">

Maka kalian akan membuat tabel data barang di database latihan1;
Kemudian kalian pilih lagi database latihan1; > tabel data barang
Lalu kalian tulis perintah ini

<img src="Lab8/Langkah 6.png" alt="Tutorial" width="400">

Dan kalian sudah membuat data barang kalian tentang handphone
Kalian bisa cek datanya di tabel data barang bagian jelajahi
Disini kalian bisa menambah/menghapus data yang ada disini

<img src="Lab8/Langkah 7.png" alt="Tutorial" width="400">

Berhubung kita sudah membuat database nya di localhost/phpadmin
Selanjutnya disini kita kembali ke file lab8_php_database
Lalu buat file baru yang bernama ``` koneksi.php ```
Fungsinya untuk mengkoneksikan file antara lab8_php_database dengan phpmyadmin
Lalu isi dengan kodingan berikut

<img src="Lab8/Langkah 8.png" alt="Tutorial" width="400">

Setelah itu kalian buka web ``` http://localhost:8080/lab8_php_database/ ```
Jika muncul tampilan berikut, maka koneksinya sudah berhasil

<img src="Lab8/Langkah 9.png" alt="Tutorial" width="400">

Kemudian disini kalian buat file baru di tempat lab8_php_database dengan nama ``` index.php ```
Lalu kalian isi kode berikut

```
<?php
include("koneksi.php");

// query untuk menampilkan data
$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);

?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Data Barang</title>
    <style>
    table {
      border-collapse: collapse;
      width: 100%;
    }
    th, td {
      border: 1px solid black;
      padding: 8px;
      text-align: left;
    }
  </style>
</head>
<body>
    <div class="container">
        <h1>Data Barang</h1>
        <a href="tambah.php">Tambah Barang</a>
        <div class="main">
            <table>
            <tr>
                <th>Gambar</th>
                <th>Nama Barang</th>
                <th>Katagori</th>
                <th>Harga Jual</th>
                <th>Harga Beli</th>
                <th>Stok</th>
                <th>Aksi</th>
            </tr>
            <?php if($result): ?>
            <?php while($row = mysqli_fetch_array($result)): ?>
            <tr>
                <td>
    <img src="gambar/<?= $row['gambar']; ?>" 
         alt="<?= $row['nama']; ?>" 
         width="100">
</td>
                <td><?= $row['nama'];?></td>
                <td><?= $row['kategori'];?></td>
                <td><?= $row['harga_beli'];?></td>
                <td><?= $row['harga_jual'];?></td>
                <td><?= $row['stok'];?></td>
                <td>
                <a href="ubah.php?id=<?= $row['id_barang']; ?>">Ubah</a> |
                <a href="hapus.php?id=<?= $row['id_barang']; ?>"
                   onclick="return confirm('Yakin ingin menghapus data ini?');">
                   Hapus
                </a>
            </td>
            </tr>
            <?php endwhile; else: ?>
            <tr>
                <td colspan="7">Belum ada data</td>
            </tr>
            <?php endif; ?>
            </table>
        </div>
    </div>
</body>
</html>
```

Lalu save phpnya, kemudian refresh web lab8 nya, maka tampilannya seperti ini

<img src="Lab8/Langkah 11.png" alt="Tutorial" width="400">

Setelah itu kalian buat file php baru di lab8_php_database dengan nama ``` tambah.php ```
Lalu kalian isi kodenya berikut

```
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

if (isset($_POST['submit']))
{
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];
    $gambar = null;
    if ($file_gambar['error'] == 0)
    {
        $filename = str_replace(' ', '_',$file_gambar['name']);
        $destination = dirname(__FILE__) .'/gambar/' . $filename;
        if(move_uploaded_file($file_gambar['tmp_name'], $destination))
        {
            $gambar = 'gambar/' . $filename;;
        }
    }
    $sql = 'INSERT INTO data_barang (nama, kategori,harga_jual, harga_beli,
stok, gambar) ';
    $sql .= "VALUE ('{$nama}', '{$kategori}','{$harga_jual}',
'{$harga_beli}', '{$stok}', '{$gambar}')";
    $result = mysqli_query($conn, $sql);
    header('location: index.php');
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Tambah Barang</title>
    <style>
    form {
  max-width: 400px;
}

form label {
  display: inline-block;
  width: 100px;
  margin-bottom: 8px;
  vertical-align: middle;
}

form input[type="text"],
form input[type="number"],
form select {
  width: 200px;
  padding: 4px 6px;
  margin-bottom: 8px;
  vertical-align: middle;
}

form input[type="file"] {
  vertical-align: middle;
  margin-bottom: 8px;
}

form button {
  background-color: blue;
  color: white;
  border: none;
  padding: 5px 12px;
  cursor: pointer;
  border-radius: 3px;
  font-size: 14px;
}

  </style>
</head>
<body>
<div class="container">
    <h1>Tambah Barang</h1>
    <div class="main">
        <form method="post" action="tambah.php"
enctype="multipart/form-data">
            <div class="input">
                <label>Nama Barang</label>
                <input type="text" name="nama" />
            </div>
            <div class="input">
                <label>Kategori</label>
                <select name="kategori">
                    <option value="Komputer">Komputer</option>
                    <option value="Elektronik">Elektronik</option>
                    <option value="Hand Phone">Hand Phone</option>
                </select>
            </div>
            <div class="input">
                <label>Harga Jual</label>
                <input type="text" name="harga_jual" />
            </div>
            <div class="input">
                <label>Harga Beli</label>
                <input type="text" name="harga_beli" />
            </div>
            <div class="input">
                <label>Stok</label>
                <input type="text" name="stok" />
            </div>
            <div class="input">
                <label>File Gambar</label>
                <input type="file" name="file_gambar" />
            </div>
            <div class="submit">
                <input type="submit" name="submit" value="Simpan" />
            </div>
        </form>
    </div>
</div>
</body>
</html>
```

Kodingan diatas sudah termasuk css nya
Jadi tampilannya seperti ini

<img src="Lab8/Langkah 12.png" alt="Tutorial" width="400">

Selanjutnya disini tambah php lagi di file lab8_php_database dengan nama ``` ubah.php ```
Kemudian kalian isi dengan kode berikut

```
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

if (isset($_POST['submit']))
{
    $id = $_POST['id'];
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['kategori'];
    $harga_beli = $_POST['kategori'];
    $stok = $_POST['kategori'];
    $file_gambar = $_POST['kategori'];
    $gambar = null;

    if ($file_gambar['error'] == 0)
    {
        $filename = str_replace(' ', '_', $file_gambar['name']);
        $destination = dirname(__FILE__) . '/gambar/' . $filename;
        if (move_uploaded_file($file_gambar['tmp_name'], $destination))
        {
            $gambar = 'gambar/' . $filename;;
        }
    }

    $sql = 'UPDATE data_barang SET ';
    $sql .= "nama = '{$nama}', kategori = '{$kategori}', ";
    $sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', stok = '{$stok}' ";
    if (!empty($gambar))
        $sql .= ", gambar = '{$gambar}' ";
    $sql .= "WHERE id_barang = '{$id}'";
    $result = mysqli_query($conn, $sql);

    header('location: index.php');
}

$id = $_GET['id'];
$sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
if (!$result) die('Error: Data tidak tersedia');
$data = mysqli_fetch_array($result);

function is_select($var, $val) {
    if ($var == $val) return 'selected="selected"';
    return false;
}

?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Ubah Barang</title>
    <style>
        body {
    font-family: Arial, sans-serif;
    background: #ffffff;
    margin: 0;
    padding: 20px;
}

.container {
    width: 500px;
    margin: 0 auto;
}

h2 {
    margin-bottom: 20px;
    font-size: 26px;
    color: #000000;
}

label {
    display: block;
    margin-bottom: 5px;
    font-size: 16px;
}

input[type="text"],
input[type="number"],
select,
input[type="file"] {
    width: 300px;
    padding: 6px;
    font-size: 14px;
    margin-bottom: 12px;
    border: 1px solid #888;
    border-radius: 2px;
}

input[type="submit"],
button {
    background-color: #007bff;
    color: white;
    border: none;
    padding: 7px 20px;
    font-size: 14px;
    cursor: pointer;
    border-radius: 2px;
}

input[type="submit"]:hover,
button:hover {
    background-color: #005fcc;
}
    </style>
</head>
<body>
<div class="container">
    <h1>Ubah Barang</h1>
    <div class="main">
        <form method="post" action="ubah.php" enctype="multipart/form-data">
            <div class="input">
                <label>Nama Barang</label>
                <input type="text" name="nama" value="<?php echo $data['nama'];?>" />
            </div>
            <div class="input">
                <label>Kategori</label>
                <select name="kategori">
                    <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Komputer">Komputer</option>
                    <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Elektronik">Elektronik</option>
                    <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Handphone">Handphone</option>
                </select>
            </div>
            <div class="input">
                <label>Harga Jual</label>
                <input type="text" name="harga_jual" value="<?php echo $data['harga_jual'];?>" />
            </div>
            </div>
            <div class="input">
                <label>Harga Beli</label>
                <input type="text" name="harga_beli" value="<?php echo $data['harga_beli'];?>" />
            </div>
            </div>
            <div class="input">
                <label>Stok</label>
                <input type="text" name="stok" value="<?php echo $data['stok'];?>" />
            </div>
            </div>
            <div class="input">
                <label>File Gambar</label>
                <input type="text" name="file gambar" />
            </div>
            </div>
            <div class="submit">
            <input type="hidden" name="id" value="<?php echo $data['id_barang'];?>" />
                <input type="submit" name="submit" value="Simpan" />
            </div>
        </form>
    </div>
</div>
</body>
</html>
```

Sama seperti kode tambah, ini juga sudah termasuk css
Maka tampilannya jadi seperti ini

<img src="Lab8/Langkah 14.png" alt="Tutorial" width="400">

Selanjutnya kita buat file baru di lab8_php_database dengan nama ``` hapus.php ```
Dan isi kodenya seperti berikut

```
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
?>
```

Nah disini ada beberapa hal yang kurang
Yaitu gambar handphone, akses ubah dan hapus filenya

Pertama untuk memunculkan gambar handphone, kalian buat file di lab8_php_database dengan nama gambar
Lalu isi file gambar sesuai tipe filenya
Cara memunculkan gambar di webnya adalah dengan menyesuaikan nama gambarnya sesuai yang ada di tabel phpmyadmin

<img src="Lab8/Langkah 7.png" alt="Tutorial" width="400">

<img src="Lab8/Langkah 13.png" alt="Tutorial" width="400">

Bisa kalian lihat, kalau disitu nama filenya sama dengan tabel gambar di phpmyadmin, agar langsung kedetect dan munculkan gambar
Dan formatnya juga harus sesuai, kalau jpg ya jpg, png ya png (digambar sebenernya saya edit formatnya png, tapi lupa screenshot)

Lalu selanjutnya memunculkan akses ubah dan hapus file
kalau kalian lihat kode di ``` index.php ```
Disitu akan ada kode yang sudah saya letakkan ini

```
<td>
        <a href="ubah.php?id=<?= $row['id_barang']; ?>">Ubah</a> |
        <a href="hapus.php?id=<?= $row['id_barang']; ?>"
           onclick="return confirm('Yakin ingin menghapus data?');">
           Hapus
        </a>
    </td>
```

Itu adalah kode untuk memberikan akses berupa Ubah dan Hapus file di ``` index.php ```
Bisa dicek ulang saja diatas

Setelah menambahkan gambar dan akses ubah hapus
Maka tampilannya jadi seperti ini

<img src="Lab8/Langkah 15.png" alt="Tutorial" width="400">

Jadi seperti itulah penggunaan MySQL dan Apache untuk pembuatan database
Yang nanti bisa ditampilkan melalui XAMPP dan web
Sekian terimakasih

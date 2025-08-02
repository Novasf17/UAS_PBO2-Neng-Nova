# UAS_PBO2-Neng-Nova
Aplikasi KasirProperti
code nya
package utils;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class Koneksi {
    public static Connection connect() {
        try {
            String url = "jdbc:mysql://localhost:3306/kasir_properti";
            String user = "root";
            String pass = "";
            return DriverManager.getConnection(url, user, pass);
        } catch (SQLException e) {
            System.out.println("Koneksi gagal: " + e.getMessage());
            return null;
        }
    }
}

// model/User.java
package model;

public class User {
    private String username;
    private String password;

    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }

    public String getUsername() { return username; }
    public String getPassword() { return password; }
}


// model/Properti.java

package model;

public abstract class Properti {
    protected String nama;
    protected double harga;

    public Properti(String nama, double harga) {
        this.nama = nama;
        this.harga = harga;
    }

    public abstract String getJenis();

    public String getNama() { return nama; }
    public double getHarga() { return harga; }
}


// model/Rumah.java
package model;

public class Rumah extends Properti {
    public Rumah(String nama, double harga) {
        super(nama, harga);
    }

    @Override
    public String getJenis() {
        return "Rumah";
    }
}

// model/Apartemen.java

package model;

public class Apartemen extends Properti {
    public Apartemen(String nama, double harga) {
        super(nama, harga);
    }

    @Override
    public String getJenis() {
        return "Apartemen";
    }
}


package model;

public class Kosan extends Properti {
    public Kosan(String nama, double harga) {
        super(nama, harga);
    }

    @Override
    public String getJenis() {
        return "Kosan";
    }
}


package model;

public class Penyewa {
    private String nama;
    private String noTelp;

    public Penyewa(String nama, String noTelp) {
        this.nama = nama;
        this.noTelp = noTelp;
    }

    public String getNama() { return nama; }
    public String getNoTelp() { return noTelp; }
}


// model/Transaksi.java

package model;

public class Transaksi {
    private Penyewa penyewa;
    private Properti properti;
    private int durasi;

    public Transaksi(Penyewa penyewa, Properti properti, int durasi) {
        this.penyewa = penyewa;
        this.properti = properti;
        this.durasi = durasi;
    }

    public double hitungTotal() {
        return properti.getHarga() * durasi;
    }

    public String detail() {
        return penyewa.getNama() + " menyewa " + properti.getJenis() +
               " - " + properti.getNama() + " selama " + durasi +
               " hari. Total: Rp " + hitungTotal();
    }
}


// controller/UserController.java

package controller;

import model.User;
import utils.Koneksi;

import java.sql.*;

public class UserController {
    public boolean login(String username, String password) {
        try (Connection conn = Koneksi.connect()) {
            String sql = "SELECT * FROM user WHERE username=? AND password=?";
            PreparedStatement pst = conn.prepareStatement(sql);
            pst.setString(1, username);
            pst.setString(2, password);
            ResultSet rs = pst.executeQuery();
            return rs.next();
        } catch (SQLException e) {
            System.out.println("Error login: " + e.getMessage());
            return false;
        }
    }

    public boolean register(User user) {
        try (Connection conn = Koneksi.connect()) {
            String sql = "INSERT INTO user (username, password) VALUES (?, ?)";
            PreparedStatement pst = conn.prepareStatement(sql);
            pst.setString(1, user.getUsername());
            pst.setString(2, user.getPassword());
            pst.executeUpdate();
            return true;
        } catch (SQLException e) {
            System.out.println("Error register: " + e.getMessage());
            return false;
        }
    }
}

// view/Main.java

package view;

import controller.UserController;
import model.User;

import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        UserController userController = new UserController();

        System.out.println("1. Register");
        System.out.println("2. Login");
        System.out.print("Pilih: ");
        int pilihan = input.nextInt(); input.nextLine();

        if (pilihan == 1) {
            System.out.print("Username: ");
            String username = input.nextLine();
            System.out.print("Password: ");
            String password = input.nextLine();
            User user = new User(username, password);
            if (userController.register(user)) {
                System.out.println("Register berhasil!");
            } else {
                System.out.println("Register gagal.");
            }
        } else if (pilihan == 2) {
            System.out.print("Username: ");
            String username = input.nextLine();
            System.out.print("Password: ");
            String password = input.nextLine();
            if (userController.login(username, password)) {
                System.out.println("Login berhasil!");
                // Arahkan ke dashboard atau menu lain
            } else {
                System.out.println("Login gagal!");
            }
        }
    }
}

BAB I – PENDAHULUAN
1.1 Latar Belakang
Penyewaan properti seperti rumah, apartemen, dan kos-kosan semakin berkembang seiring dengan meningkatnya kebutuhan tempat tinggal sementara. Oleh karena itu, dibutuhkan sebuah aplikasi kasir yang mampu mencatat transaksi penyewaan properti secara efisien dan akurat. Aplikasi ini dibangun menggunakan Java dengan JavaFX sebagai GUI dan MySQL sebagai penyimpanan datanya.
1.2 Tujuan
•	Membuat aplikasi kasir penyewaan properti berbasis JavaFX dan database MySQL.
•	Menerapkan prinsip OOP dalam pengembangan program.
•	Membantu admin dalam mengelola data properti, penyewa, dan transaksi.
1.3 Manfaat
•	Mempermudah pencatatan penyewaan properti
•	Menjadi media pembelajaran penerapan OOP di dunia nyata
•	Menjadi portofolio proyek untuk pengembangan selanjutnya

BAB II – LANDASAN TEORI
2.1 Java dan JavaFX
Java adalah bahasa pemrograman berorientasi objek. JavaFX adalah library GUI modern untuk Java, mendukung tampilan interaktif dan fleksibel.
2.2 MySQL dan JDBC
MySQL adalah sistem manajemen database relasional. Java menggunakan JDBC (Java Database Connectivity) untuk menghubungkan dan melakukan query ke MySQL.
2.3 Konsep OOP
•	Encapsulation: Menyembunyikan data dengan private field dan public getter/setter.
•	Inheritance: Pewarisan antar class.
•	Polymorphism: Satu method bisa memiliki banyak bentuk (overriding).
•	Abstraction: Class abstrak dan method abstrak untuk struktur yang konsisten.
BAB III – PERANCANGAN DAN IMPLEMENTASI
3.1 Rancangan Database
Nama Database: kasir_properti
Tabel:
•	user: id, username, password
•	rumah: id, nama, harga
•	apartemen: id, nama, harga
•	kosan: id, nama, harga
•	penyewa: id, nama, no_telp
•	transaksi: id, penyewa_id, jenis_properti, properti_id, tanggal, durasi, total_harga
3.2 Struktur Project Java (NetBeans)
src/
├── controller/
│   ├── UserController.java
│   ├── PropertiController.java
│   └── TransaksiController.java
├── model/
│   ├── User.java
│   ├── Penyewa.java
│   ├── Rumah.java
│   ├── Apartemen.java
│   ├── Kosan.java
│   └── Transaksi.java
├── view/
│   ├── Login.fxml
│   ├── Register.fxml
│   ├── Dashboard.fxml
│   └── Main.java
└── utils/
    └── Koneksi.java

Login/Register
User.java (Model + Encapsulation):
java
SalinEdit
public class User {
    private String username;
    private String password;

    public User(String username, String password){
        this.username = username;
        this.password = password;
    }

    public String getUsername() { return username; }
    public String getPassword() { return password; }
} 
public class UserController {
    public boolean login(String username, String password) {
        try (Connection conn = Koneksi.connect()) {
            String sql = "SELECT * FROM user WHERE username=? AND password=?";
            PreparedStatement pst = conn.prepareStatement(sql);
            pst.setString(1, username);
            pst.setString(2, password);
            ResultSet rs = pst.executeQuery();
            return rs.next();
        } catch (SQLException e) {
            System.out.println("Error login: " + e.getMessage());
            return false;
        }
    }
}

Abstract Class: Properti.java
java
SalinEdit
public abstract class Properti {
    protected String nama;
    protected double harga;

    public Properti(String nama, double harga) {
        this.nama = nama;
        this.harga = harga;
    }

    public abstract String getJenis();
    public double getHarga() { return harga; }
}
Rumah.java (Inheritance + Polymorphism)
java
SalinEdit
public class Rumah extends Properti {
    public Rumah(String nama, double harga) {
        super(nama, harga);
    }

    @Override
    public String getJenis() {
        return "Rumah";
    }
}

Transaksi.java
java
SalinEdit
public class Transaksi {
    private Penyewa penyewa;
    private Properti properti;
    private int durasi;

    public Transaksi(Penyewa penyewa, Properti properti, int durasi) {
        this.penyewa = penyewa;
        this.properti = properti;
        this.durasi = durasi;
    }

    public double hitungTotal() {
        return properti.getHarga() * durasi;
    }

    public String detail() {
        return penyewa.getNama() + " menyewa " + properti.getJenis() + " seharga " +
               properti.getHarga() + " selama " + durasi + " hari.";
    }
}

 


 


Diagram Kelas (UML) – Format Teks
pgsql
SalinEdit
+------------------+
|   <<abstract>>   |
|    Properti      |
+------------------+
| -nama: String    |
| -harga: double   |
+------------------+
| +getJenis():String [abstract] |
| +getHarga():double            |
+------------------+

     ▲         ▲         ▲
     |         |         |
+---------+ +------------+ +--------+
| Rumah   | | Apartemen  | | Kosan  |
+---------+ +------------+ +--------+
| +getJenis(): String    |
+------------------------+

+--------------------+
|     Penyewa        |
+--------------------+
| -nama: String      |
| -noTelp: String    |
+--------------------+
| +getNama(): String |
| +getNoTelp(): String |
+--------------------+

+--------------------+
|     User           |
+--------------------+
| -username: String  |
| -password: String  |
+--------------------+
| +getUsername()     |
| +getPassword()     |
+--------------------+

+------------------------------+
|         Transaksi           |
+------------------------------+
| -penyewa: Penyewa           |
| -properti: Properti         |
| -durasi: int                |
+------------------------------+
| +hitungTotal(): double      |
| +detail(): String           |

    
CREATE DATABASE kasir_properti;
USE kasir_properti;

-- Tabel User (login/register)
CREATE TABLE user (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    password VARCHAR(100) NOT NULL
);

-- Tabel Penyewa
CREATE TABLE penyewa (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100),
    no_telp VARCHAR(20)
);

-- Tabel Rumah
CREATE TABLE rumah (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100),
    harga DOUBLE
);

-- Tabel Apartemen
CREATE TABLE apartemen (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100),
    harga DOUBLE
);

-- Tabel Kosan
CREATE TABLE kosan (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100),
    harga DOUBLE
);

-- Tabel Transaksi
CREATE TABLE transaksi (
    id INT AUTO_INCREMENT PRIMARY KEY,
    penyewa_id INT,
    jenis_properti ENUM('RUMAH', 'APARTEMEN', 'KOSAN'),
    properti_id INT,
    tanggal DATE,
    durasi INT,
    total_harga DOUBLE,
    FOREIGN KEY (penyewa_id) REFERENCES penyewa(id)
);


BAB IV – KESIMPULAN DAN SARAN
4.1 Kesimpulan
Aplikasi kasir penyewaan properti ini berhasil dibuat menggunakan Java dengan JavaFX dan database MySQL. Aplikasi ini mencakup fitur register, login, pengelolaan properti dan transaksi penyewaan. Konsep OOP diterapkan secara utuh dalam class dan objek.


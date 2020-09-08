<h1 align="center">
    MultiPermition
</h1>

**Contoh Multi Check Permissions.** Request permition secara bersamaan, Zein sarankan untuk requestnya dijalankan di activity yang pertama aktif, disini Zein masukan ke MainActivity :

#
**Step 1.**

**Manifest.** Tambahkan permition ke file manifest. Zein sarankan untuk menambahkan requestLegacyExternalStorage=true jika android kamu sudah android 10.

```xml
<manifest >

    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />

    <application
        android:requestLegacyExternalStorage="true">

    </application>

</manifest>
```

#
**Step 2.**
\
Tambahkan array permition yang dibutuhkan :

**First Activity.** letakan permition pada saat awal activity dimulai, disini Zein meletakannya di `MainActivity`.

```java
public class MainActivity extends AppCompatActivity {

    String[] permissions = new String[]{
        Manifest.permission.READ_EXTERNAL_STORAGE,
        Manifest.permission.WRITE_EXTERNAL_STORAGE
    };
    
    ...

}
```

#
**Step 3.**
\
Tambahkan function untuk mengecek permition apps apakah semua permition sudah diberikan izin, Jika belum diberikan izin maka akan keluar popup. Silahkan berikan izin dengan menekan `allow` :

```java
public class MainActivity extends AppCompatActivity {
    
    ...

    int MULTIPLE_PERMISSIONS = 1;
    private boolean checkPermissions() {
        int result;
        List<String> listPermissionsNeeded = new ArrayList<>();

        for (String p : permissions) {
            result = ContextCompat.checkSelfPermission(getApplicationContext(), p);
            if (result != PackageManager.PERMISSION_GRANTED) {
                listPermissionsNeeded.add(p);
            }
        }
        if (!listPermissionsNeeded.isEmpty()) {
            ActivityCompat.requestPermissions(this, listPermissionsNeeded.toArray(new String[0]), MULTIPLE_PERMISSIONS);
            return false;
        }
        return true;
    }
    
    ...

}
```

#
**Step 4.**
\
Jika semua akses sudah diberikan maka aplikasi dapat menjalankan proses untuk membuat directory :

```java
public class MainActivity extends AppCompatActivity {
    
    ...

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        if (requestCode == MULTIPLE_PERMISSIONS) {
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {

                //Do Someting ...
                //aksi setelah semua permition diberikan

            } else {
                StringBuilder perStr = new StringBuilder();
                for (String per : permissions) {
                    perStr.append("\n").append(per);
                }
            }
        }
    }
    
    ...

}
```

#
**Step 5.** 
\
Buat dan panggil function `onSuccessCheckPermitions` untuk membuat folder :

```java
public class MainActivity extends AppCompatActivity {
    
    ...

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        if (requestCode == MULTIPLE_PERMISSIONS) {
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                //tambahkan ini
                onSuccessCheckPermitions();
            } else {
                StringBuilder perStr = new StringBuilder();
                for (String per : permissions) {
                    perStr.append("\n").append(per);
                }
            }
        }
    }
    
    ...

}
```

#
**Step 6.**
\
Jika `onRequestPermissionsResult` sudah mendapat permition yang dibutuhkan, maka kita akan membuat dan menjalankan function `onSuccessCheckPermitions`. **Cukup 1 kali penggunaan saja di FirstActivity(Acctivity yang pertama berjalan)**:

```java
public class MainActivity extends AppCompatActivity {
    
    ...

    private void onSuccessCheckPermitions() {

        Toast.makeText(this, "All Granted", Toast.LENGTH_SHORT).show();
        //letakan action kamu disini
        
    }

}
```

**notes.** 
  - Cara ini akan saya pakai disetiap lib yang saya buat dan membutuhkan permition.
  - Di function `onSuccessCheckPermitions` kita bisa deklarasi apa saja yang kita mau.
  
#
**Step 7.**
\
Tambahkan function `checkPermissions` di `onCreate` agar setiap activity dijalankan maka akan selalu mengecek apakah izin sudah diberikan :

```java
public class MainActivity extends AppCompatActivity {

    ...

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        ...

        if (checkPermissions()) {
            Toast.makeText(this, "Izin sudah diberikan", Toast.LENGTH_SHORT).show();
            onSuccessCheckPermitions();
        } else {
            Toast.makeText(this, "Berikan izin untuk melanjutkan ke tahap berikutnya", Toast.LENGTH_SHORT).show();
        }
    }
    
    ...

}
```

#
**Step 8.**
\
Fullcode akan tampak seperti ini :

```java
public class MainActivity extends AppCompatActivity {

    String[] permissions = new String[]{
        Manifest.permission.READ_EXTERNAL_STORAGE,
        Manifest.permission.WRITE_EXTERNAL_STORAGE
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        if (checkPermissions()) {
            Toast.makeText(this, "Izin sudah diberikan", Toast.LENGTH_SHORT).show();
            onSuccessCheckPermitions();
        } else {
            Toast.makeText(this, "Berikan izin untuk melanjutkan ke tahap berikutnya", Toast.LENGTH_SHORT).show();
        }
    }

    int MULTIPLE_PERMISSIONS = 1;
    private boolean checkPermissions() {
        int result;
        List<String> listPermissionsNeeded = new ArrayList<>();

        for (String p : permissions) {
            result = ContextCompat.checkSelfPermission(getApplicationContext(), p);
            if (result != PackageManager.PERMISSION_GRANTED) {
                listPermissionsNeeded.add(p);
            }
        }
        if (!listPermissionsNeeded.isEmpty()) {
            ActivityCompat.requestPermissions(this, listPermissionsNeeded.toArray(new String[0]), MULTIPLE_PERMISSIONS);
            return false;
        }
        return true;
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        if (requestCode == MULTIPLE_PERMISSIONS) {
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                onSuccessCheckPermitions();
            } else {
                StringBuilder perStr = new StringBuilder();
                for (String per : permissions) {
                    perStr.append("\n").append(per);
                }
            }
        }
    }

    private void onSuccessCheckPermitions() {

        Toast.makeText(this, "All Granted", Toast.LENGTH_SHORT).show();
        //letakan action kamu disini
        
    }
}
```

#
**Step 9.**
\
Jika sukses maka akan tampil seperti ini :
|![](https://github.com/gzeinnumer/MultiPermition/blob/master/assets/example1.jpg)|![](https://github.com/gzeinnumer/MultiPermition/blob/master/assets/example2.jpg)|![](https://github.com/gzeinnumer/MultiPermition/blob/master/assets/example3.jpg)|
|--|--|--|
|Request Permition 1 |Request Permition 2 |Jika sudah diizinkan maka akan keluar Toast `AllGranted` |

---

```
Copyright 2020 M. Fadli Zein
```

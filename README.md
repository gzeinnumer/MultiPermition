<p align="center">
  <img src="https://bordencom.com/wp-content/uploads/2016/03/Do-You-Have-Permission.png" width="400"/>
</p>

<h1 align="center">
    MultiPermition
</h1>

**Example Multi Check Permissions.** Request Permissions at same time, I recommend to use this step on your `First Activity`, now i use it on `MainActivity` :

#### Step 1.
**Manifest.** add permission ke file manifest. I recommend to add `requestLegacyExternalStorage=true` if you using Android 10.

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
#### Step 2.
Add array permission that you need :

**First Activity.** put request permission at your first activity, now i use it on `MainActivity`.

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
#### Step 3.
Add function `checkPermissions` to check permission on app, if permission not granted than popup will show and ask to `Allow`. Please click `allow` :

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
#### Step 4.
You can check all permition are granted or not with function `onRequestPermissionsResult`, if granted you can run your code :

```java
public class MainActivity extends AppCompatActivity {
    
    ...

    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
        if (requestCode == MULTIPLE_PERMISSIONS) {
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {

                //Do Someting ...
                //aksi setelah semua permission diberikan

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
#### Step 5.
Make and call function `onSuccessCheckPermitions` to run your code :

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
#### Step 6.
Add action to function `onSuccessCheckPermitions` :

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
  - I always use this method to get permition for all Libray that i made.
  - You can modif body of function `onSuccessCheckPermitions`.
  
#
#### Step 7.
Add function `checkPermissions` in `onCreate` to check permission ever time app running :

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
#### Step 8.
Fullcode will be like this :

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
#### Step 9.
Preview :
|![](https://github.com/gzeinnumer/MultiPermition/blob/master/assets/example1.jpg)|![](https://github.com/gzeinnumer/MultiPermition/blob/master/assets/example2.jpg)|![](https://github.com/gzeinnumer/MultiPermition/blob/master/assets/example3.jpg)|
|--|--|--|
|Request Permission 1 |Request Permission 2 |Iff all permission granted Toast `AllGranted` will show |

---

```
Copyright 2020 M. Fadli Zein
```

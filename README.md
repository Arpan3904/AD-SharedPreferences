# AD-SharedPreferences
## activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    >

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="username"
            android:id="@+id/t1"
            android:padding="40px"

            />
        <EditText

            android:layout_width="300px"
            android:layout_height="wrap_content"
            android:id="@+id/e1"
            android:layout_toRightOf="@+id/t1"

            />
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="password"
            android:layout_below="@+id/t1"
            android:id="@+id/t2"
            android:padding="40px"
            />
        <EditText
            android:layout_width="300px"
            android:layout_height="wrap_content"
            android:id="@+id/e2"
            android:layout_below="@+id/e1"
            android:layout_toRightOf="@+id/t2"
            />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/b"
        android:layout_below="@+id/t2"
        android:text="Login"
        />

</RelativeLayout>
```
## MainActivity.java
```java

package com.example.shared_preferences;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import android.widget.Toast;

public class MainActivity extends AppCompatActivity {
    EditText e1, e2;
    SharedPreferences pref;
    Button b;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        b = findViewById(R.id.b);
        e1 = findViewById(R.id.e1);
        e2 = findViewById(R.id.e2);

        pref = getSharedPreferences("user_details", MODE_PRIVATE);
        Intent i = new Intent(MainActivity.this, Validate.class);
        if(pref.contains("uname")&&pref.contains("pwd"))
        {
            startActivity(i);
        }

        b.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                String uname = e1.getText().toString();
                String pwd = e2.getText().toString();

                if (uname.equals("arpan") && pwd.equals("arpan")) {
                    SharedPreferences.Editor editor = pref.edit();
                    editor.putString("uname", uname);
                    editor.putString("pwd", pwd);
                    editor.commit();
                    startActivity(i);
                } else {
                    Toast.makeText(MainActivity.this, "Credentials Invalid.", Toast.LENGTH_SHORT).show();
                }
            }
        });
    }
}
```
## activity_validate.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    >

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello Admin"
        android:id="@+id/t1"
        android:padding="100px"/>



    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/b"
        android:text="Log Out"
        />

</LinearLayout>
```
## Validate.java
```java
package com.example.shared_preferences;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

public class Validate extends AppCompatActivity {
    TextView t1;
    Button b;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_validate);
        b=findViewById(R.id.b);
        t1=findViewById(R.id.t1);

        SharedPreferences pref =getSharedPreferences("user_details",MODE_PRIVATE);
        Intent i=new Intent(Validate.this,MainActivity.class);
        t1.setText("hello "+pref.getString("uname",null));
        b.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {

                SharedPreferences.Editor editor= pref.edit();
                editor.clear();
                editor.commit();
                startActivity(i);
            }
        });


    }
}
```



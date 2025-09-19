## Home page xml
```javascript


<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center"
    tools:context=".HomePage">


    <TextView
        android:id="@+id/tvWelcome"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Welcome!"
        android:textSize="20sp"
        android:padding="10dp"/>

    <Button
        android:id="@+id/btnSettings"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Go to Settings"/>

</LinearLayout>



```


## Home page Home.java File

```javascript
package com.example.practice;

import android.content.Intent;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.widget.Button;
import android.widget.TextView;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.appcompat.app.AppCompatDelegate;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class HomePage extends AppCompatActivity {
    TextView tvWelcome;
    Button btnSettings;

    private static final String PREF_NAME = "MySettings";
    private static final String KEY_DARK_MODE = "dark_mode";
    private static final String KEY_LANGUAGE = "language";
    private static final String KEY_FONT_SIZE = "font_size";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);

        // Load Dark Mode before setting layout
        SharedPreferences prefs = getSharedPreferences(PREF_NAME, MODE_PRIVATE);
        boolean isDarkMode = prefs.getBoolean(KEY_DARK_MODE, false);
        if (isDarkMode) {
            AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_YES);
        } else {
            AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_NO);
        }


        setContentView(R.layout.activity_home_page);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });

        tvWelcome=findViewById(R.id.tvWelcome);
        btnSettings=findViewById(R.id.btnSettings);




        // Go to Settings Page
        btnSettings.setOnClickListener(v -> {
            Intent intent = new Intent(HomePage.this, MainActivity.class);
            startActivity(intent);
        });






    }//========================


    @Override
    protected void onResume() {
        super.onResume();

        SharedPreferences prefs = getSharedPreferences(PREF_NAME, MODE_PRIVATE);

        // Language apply
        String language = prefs.getString(KEY_LANGUAGE, "English");
        if (language.equals("English")) {
            tvWelcome.setText("Welcome!");
        } else {
            tvWelcome.setText("স্বাগতম!");
        }

        // Font size apply
        int fontIndex = prefs.getInt(KEY_FONT_SIZE, 1);
        applyFontSize(fontIndex);
    }


    private void applyFontSize(int index) {
        switch (index) {
            case 0: // Small
                tvWelcome.setTextSize(14f);
                break;
            case 1: // Medium
                tvWelcome.setTextSize(18f);
                break;
            case 2: // Large
                tvWelcome.setTextSize(24f);
                break;
        }
    }



}


```

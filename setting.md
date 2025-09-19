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

## setting Page xml
```javascript

<?xml version="1.0" encoding="utf-8"?>
<SearchView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    tools:context=".MainActivity"
    android:orientation="vertical"
    android:padding="16dp"
    >


    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="20dp">

        <!-- Dark Mode Switch -->
        <Switch
            android:id="@+id/switchDarkMode"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Enable Dark Mode"/>

        <!-- Language Selection -->
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Select Language"
            android:paddingTop="20dp"
            android:textSize="18sp"/>

        <RadioGroup
            android:id="@+id/radioGroupLanguage"
            android:layout_width="match_parent"
            android:layout_height="wrap_content">

            <RadioButton
                android:id="@+id/radioEnglish"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="English"/>

            <RadioButton
                android:id="@+id/radioBangla"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="বাংলা"/>
        </RadioGroup>

        <!-- Font Size Selection -->
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Select Font Size"
            android:paddingTop="20dp"
            android:textSize="18sp"/>

        <Spinner
            android:id="@+id/spinnerFontSize"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:entries="@array/font_sizes"/>

        <!-- Demo Text -->
        <TextView
            android:id="@+id/tvDemo"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="This is a demo text"
            android:paddingTop="30dp"/>
    </LinearLayout>




</SearchView>



````

## setting page java file

```javascript

package com.example.practice;

import android.content.SharedPreferences;
import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.Button;
import android.widget.CompoundButton;
import android.widget.EditText;
import android.widget.LinearLayout;
import android.widget.RadioButton;
import android.widget.RadioGroup;
import android.widget.Spinner;
import android.widget.Switch;
import android.widget.TextView;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.appcompat.app.AppCompatDelegate;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {

    Switch switchDarkMode;
    RadioGroup radioGroupLanguage;
    RadioButton radioEnglish, radioBangla;
    Spinner spinnerFontSize;
    TextView tvDemo;


    private static final String PREF_NAME = "MySettings";
    private static final String KEY_DARK_MODE = "dark_mode";
    private static final String KEY_LANGUAGE = "language";
    private static final String KEY_FONT_SIZE = "font_size";





    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);

        SharedPreferences sharedPreferences=getSharedPreferences(PREF_NAME,MODE_PRIVATE);
        boolean darkMode=sharedPreferences.getBoolean(KEY_DARK_MODE,false);

        if(darkMode){
            AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_YES);
        }else {
            AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_NO);
        }



        setContentView(R.layout.activity_main);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });

        //Find id
        switchDarkMode=findViewById(R.id.switchDarkMode);
        radioGroupLanguage=findViewById(R.id.radioGroupLanguage);
        radioEnglish=findViewById(R.id.radioEnglish);
        radioBangla=findViewById(R.id.radioBangla);
        spinnerFontSize=findViewById(R.id.spinnerFontSize);
        tvDemo=findViewById(R.id.tvDemo);


        tvDemo.setText("Maruf Hasan");


        String savedLanguage = sharedPreferences.getString(KEY_LANGUAGE, "English");
        if (savedLanguage.equals("English")) {
            radioEnglish.setChecked(true);
        } else {
            radioBangla.setChecked(true);
        }


        switchDarkMode.setChecked(darkMode);
        switchDarkMode.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(@NonNull CompoundButton buttonView, boolean isChecked) {
                SharedPreferences sharedPreferences=getSharedPreferences(PREF_NAME,MODE_PRIVATE);
                SharedPreferences.Editor editor=sharedPreferences.edit();
                editor.putBoolean(KEY_DARK_MODE,isChecked);
                editor.apply();


                if(isChecked){
                    AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_YES);
                }else {
                    AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_NO);
                }
            }
        });

        int savedFontIndex = sharedPreferences.getInt(KEY_FONT_SIZE, 1);
        spinnerFontSize.setSelection(savedFontIndex);
        applyFontSize(savedFontIndex);

        radioGroupLanguage.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(@NonNull RadioGroup group, int checkedId) {

                SharedPreferences.Editor editor=sharedPreferences.edit();

                if(checkedId==R.id.radioEnglish){
                    editor.putString(KEY_LANGUAGE, "English");
                }else {
                    editor.putString(KEY_LANGUAGE, "Bangla");
                }

                editor.apply();



            }
        });


        spinnerFontSize.setOnItemSelectedListener(new android.widget.AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(android.widget.AdapterView<?> parent, android.view.View view, int position, long id) {
                SharedPreferences.Editor editor = sharedPreferences.edit();
                editor.putInt(KEY_FONT_SIZE, position);
                editor.apply();

                Toast.makeText(MainActivity.this, ""+position, Toast.LENGTH_SHORT).show();

                applyFontSize(position);
            }

            @Override
            public void onNothingSelected(android.widget.AdapterView<?> parent) { }
        });











    }//===================================

    private void applyFontSize(int index) {
        switch (index) {
            case 0: // Small
                tvDemo.setTextSize(14f);
                break;
            case 1: // Medium
                tvDemo.setTextSize(18f);
                break;
            case 2: // Large
                tvDemo.setTextSize(24f);
                break;
        }
    }






}

```

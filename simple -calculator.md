
<img width="300" height="700" alt="Simple-calculator" src="https://github.com/user-attachments/assets/69c09728-90e3-4afb-a2cd-a14c20aba550" />


## Calculator Java android - XML file

```javascript

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    android:orientation="vertical"
    android:padding="16dp"
    >

    <EditText
        android:id="@+id/number1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter first Number"
        android:inputType="number"
        />
    <EditText
        android:id="@+id/number2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter second Number"
        android:inputType="number"
        />
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:weightSum="4"
        android:layout_marginTop="12dp"
        >

        <Button
            android:id="@+id/btnAdd"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="+"/>

        <Button
            android:id="@+id/btnSubtract"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="-"
            android:layout_marginStart="8dp"/>

        <Button
            android:id="@+id/btnMultiply"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="×"
            android:layout_marginStart="8dp"/>

        <Button
            android:id="@+id/btnDivide"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="÷"
            android:layout_marginStart="8dp"/>

    </LinearLayout>


    <TextView
        android:id="@+id/tvResult"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Result"
        android:textSize="24sp"
        android:paddingTop="16dp"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:weightSum="2">

        <Button
            android:id="@+id/btnEquals"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="="/>

        <Button
            android:id="@+id/btnClear"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="Clear"
            android:layout_marginStart="8dp"/>
    </LinearLayout>


    <!-- History Section -->
    <TextView
        android:id="@+id/tvHistoryTitle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="History:"
        android:textSize="20sp"
        android:paddingTop="16dp"
        android:paddingBottom="8dp"/>

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:background="#EEEEEE"
        android:padding="8dp">

        <LinearLayout
            android:id="@+id/layoutHistory"
            android:orientation="vertical"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"/>
    </ScrollView>


</LinearLayout>}

```

## java file

```javascript

package com.example.practice;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.LinearLayout;
import android.widget.TextView;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {

    EditText number1,number2;
    Button btnAdd,btnSubtract,btnMultiply,btnDivide,btnEquals,btnClear;
    TextView tvResult;
    LinearLayout layoutHistory;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });

        number1=findViewById(R.id.number1);
        number2=findViewById(R.id.number2);
        btnAdd=findViewById(R.id.btnAdd);
        btnSubtract=findViewById(R.id.btnSubtract);
        btnMultiply=findViewById(R.id.btnMultiply);
        btnDivide=findViewById(R.id.btnDivide);
        btnEquals=findViewById(R.id.btnEquals);
        btnClear=findViewById(R.id.btnClear);
        tvResult=findViewById(R.id.tvResult);
        layoutHistory=findViewById(R.id.layoutHistory);


        //we are last operation chosen

        final String[] lastOp={""};

        btnAdd.setOnClickListener(v->{ performedOperation("+");});
        btnSubtract.setOnClickListener(v->{ performedOperation("-");});
        btnMultiply.setOnClickListener(v->{ performedOperation("*");});
        btnDivide.setOnClickListener(v->{ performedOperation("/");});


        // Clear button - reset everything
        btnClear.setOnClickListener(v -> {
            number1.setText("");
            number2.setText("");
            tvResult.setText("Result: ");
            layoutHistory.removeAllViews();
            Toast.makeText(MainActivity.this, "Cleared", Toast.LENGTH_SHORT).show();
        });







    }//=========================================================================

    private void performedOperation(String operation){

        String s1=number1.getText().toString().trim();
        String s2=number2.getText().toString().trim();


        if(s1.isEmpty() || s2.isEmpty()){
            Toast.makeText(this, "Please Enter Both number", Toast.LENGTH_SHORT).show();
            return;
        }



        double num1,num2;
        try {
            num1=Double.parseDouble(s1);
            num2=Double.parseDouble(s2);

        }catch (NumberFormatException e){
            Toast.makeText(MainActivity.this, "Invalid number format", Toast.LENGTH_SHORT).show();
            return;
        }

        double result;
        String historyEntry;

        switch (operation){
            case "+":
                result=num1+num2;
                historyEntry = num1 + " + " + num2 + " = " + result;
                break;
            case "-":
                result=num1-num2;
                historyEntry = num1 + " - " + num2 + " = " + result;
                break;
            case "*":
                result=num1*num2;
                historyEntry = num1 + " × " + num2 + " = " + result;
                break;
            case "/":
                if(num2==0){
                    Toast.makeText(MainActivity.this, "0 is no division", Toast.LENGTH_SHORT).show();
                    return;
                }else {
                    result=num1/num2;
                }
                historyEntry = num1 + "  ÷ " + num2 + " = " + result;
                break;
            default:
                Toast.makeText(this, "Unknown operation", Toast.LENGTH_SHORT).show();
                return;

        }


        String resultText="";
        if(result==Math.rint(result)){
            resultText = String.format("Result: %.0f", result);
        } else {
            resultText = String.format("Result: %s", result);
        }

        tvResult.setText(resultText);


        // Add to history
        TextView tvHistoryItem = new TextView(this);
        tvHistoryItem.setText(historyEntry);
        tvHistoryItem.setTextSize(16);
        layoutHistory.addView(tvHistoryItem);


    }





}

```

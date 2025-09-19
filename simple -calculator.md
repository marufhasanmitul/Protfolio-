
<img width="720" height="1640" alt="Simple-calculator" src="https://github.com/user-attachments/assets/69c09728-90e3-4afb-a2cd-a14c20aba550" />


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

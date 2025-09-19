<p align="center" width="100%">
    <img width="300" height="500" alt="adnote" src="https://github.com/user-attachments/assets/02cb11ba-66b2-4a33-80b8-02b80d9d96e8" />
    <img width="300" height="500" alt="home_page" src="https://github.com/user-attachments/assets/9c2bb6fd-a908-407d-bc52-7d4c5767be9d" />
</p>





# SharedPreferences Use NotePad Apps

## MainActivity XML
```javascript

<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:padding="10dp"
        />

    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:id="@+id/fabAdd"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@android:drawable/ic_input_add"
        android:layout_alignParentBottom="true"
        android:layout_alignParentEnd="true"
        android:layout_margin="20dp"/>

</RelativeLayout>

```

## MainActivity.java

```javascript

package com.example.noteapp;

import android.content.Intent;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;
import androidx.recyclerview.widget.LinearLayoutManager;
import androidx.recyclerview.widget.RecyclerView;

import com.google.android.material.floatingactionbutton.FloatingActionButton;
import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;

import java.lang.reflect.Type;
import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {
    FloatingActionButton fabAdd;
    RecyclerView recyclerView;

    ArrayList<String> notes;
    NoteAdapter adapter;

    private static final String PREF_NAME = "NotesPref";
    private static final String KEY_NOTES = "notes";
    private static final int REQUEST_CODE = 1;

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

        fabAdd=findViewById(R.id.fabAdd);
        recyclerView=findViewById(R.id.recyclerView);
        notes=new ArrayList<>();

        notes=loadNotes();

        adapter=new NoteAdapter(notes, new NoteAdapter.OnNoteClickListener() {
            @Override
            public void onNoteClick(int position) {
                // Edit Note
                Intent intent = new Intent(MainActivity.this, AddEditNoteActivity.class);
                intent.putExtra("note", notes.get(position));
                intent.putExtra("position", position);
                startActivityForResult(intent, REQUEST_CODE);
            }

            @Override
            public void onNoteLongClick(int position) {
                // Delete Note
                notes.remove(position);
                saveNotes();
                adapter.notifyDataSetChanged();
                Toast.makeText(MainActivity.this, "Note Deleted", Toast.LENGTH_SHORT).show();
            }
        });


        recyclerView.setLayoutManager(new LinearLayoutManager(MainActivity.this));
        recyclerView.setAdapter(adapter);





        fabAdd.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(MainActivity.this, AddEditNoteActivity.class);
                startActivityForResult(intent, REQUEST_CODE);
            }
        });




    }//====================================================

    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        if(requestCode==REQUEST_CODE && resultCode==RESULT_OK && data != null){
            String note=data.getStringExtra("note");
            int position=data.getIntExtra("position",-1);

            Log.d("custom_string",""+position);

            if(position >=0){
               notes.set(position,note);
               adapter.notifyDataSetChanged();
            }else {
                notes.add(note);
            }

            saveNotes();

        }
    }

    //======================================================

    private void saveNotes() {
        SharedPreferences prefs = getSharedPreferences(PREF_NAME, MODE_PRIVATE);
        SharedPreferences.Editor editor = prefs.edit();

        Gson gson = new Gson();
        String json = gson.toJson(notes);
        editor.putString(KEY_NOTES, json);
        editor.apply();
    }


    private ArrayList<String>loadNotes(){
        SharedPreferences prefs=getSharedPreferences(PREF_NAME,MODE_PRIVATE);
        String json=prefs.getString(KEY_NOTES,null);

        Log.d("custom_Sting",""+json);

        Gson gson=new Gson();
        Type type = new TypeToken<ArrayList<String>>() {}.getType();


        if(json !=null){
            return gson.fromJson(json,type);
        }else {
            return new ArrayList<>();
        }


    }





}

```

## addnaote.xml
```javascript

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:layout_margin="20dp"
    tools:context=".AddEditNoteActivity">


    <EditText
        android:id="@+id/etNote"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Write your note here"
        android:minLines="5"
        android:gravity="top"
        android:background="@android:drawable/editbox_background"/>

    <Button
        android:id="@+id/btnSave"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Save Note"
        android:layout_marginTop="20dp"/>

</LinearLayout>

```

## add_note.java

```javascript

package com.example.noteapp;

import android.content.Intent;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class AddEditNoteActivity extends AppCompatActivity {

    EditText etNote;
    Button btnSave;
    int position = -1;





    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_add_edit_note);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });


        etNote=findViewById(R.id.etNote);
        btnSave=findViewById(R.id.btnSave);

        Intent intent = getIntent();
        if (intent.hasExtra("note")) {
            etNote.setText(intent.getStringExtra("note"));
            position = intent.getIntExtra("position", -1);

        }


        btnSave.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String note = etNote.getText().toString().trim();
                Intent resultIntent = new Intent();
                resultIntent.putExtra("note", note);
                resultIntent.putExtra("position", position);
                setResult(RESULT_OK, resultIntent);
                finish();
            }
        });




    }
}

```

## NoteAdapter.java

```javascript
package com.example.noteapp;

import android.annotation.SuppressLint;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

import androidx.annotation.NonNull;
import androidx.recyclerview.widget.RecyclerView;

import java.util.List;

public class NoteAdapter extends RecyclerView.Adapter<NoteAdapter.NoteViewHolder> {

    private List<String> notes;
    private OnNoteClickListener listener;



    public interface OnNoteClickListener{
        void onNoteClick(int position);
        void onNoteLongClick(int position);
    }

    public NoteAdapter(List<String> notes,OnNoteClickListener listener) {
        this.notes = notes;
        this.listener=listener;
    }

    public class NoteViewHolder extends RecyclerView.ViewHolder{
        TextView tvNote;
        public NoteViewHolder(@NonNull View itemView) {
            super(itemView);
            tvNote = itemView.findViewById(R.id.tvNote);
        }
    }

    @NonNull
    @Override
    public NoteViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.note_item, parent, false);
        return new NoteViewHolder(view);
    }

    @Override
    public void onBindViewHolder(@NonNull NoteViewHolder holder, @SuppressLint("RecyclerView") int position) {
        String note = notes.get(position);
        holder.tvNote.setText(note);
        holder.itemView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                listener.onNoteClick(position);
            }
        });

        holder.itemView.setOnLongClickListener(new View.OnLongClickListener() {
            @Override
            public boolean onLongClick(View v) {
                listener.onNoteLongClick(position);
                return true;
            }
        });


    }

    @Override
    public int getItemCount() {
        return notes.size();
    }







}


```

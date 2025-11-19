```
package com.example.recycler;

import android.os.Bundle;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;
import androidx.recyclerview.widget.GridLayoutManager;
import androidx.recyclerview.widget.LinearLayoutManager;
import androidx.recyclerview.widget.RecyclerView;

import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {
    ArrayList<ContactModel> arrContacts = new ArrayList<>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            RecyclerView recyclerView = findViewById(R.id.recyclercontact);
            recyclerView.setLayoutManager(new LinearLayoutManager(this));
            arrContacts.add(new ContactModel(R.drawable.a1,"A","9980904513"));
            arrContacts.add(new ContactModel(R.drawable.a2,"B","9980904512"));
            arrContacts.add(new ContactModel(R.drawable.a3,"C","9980904514"));
            arrContacts.add(new ContactModel(R.drawable.a4,"D","9980904515"));
            arrContacts.add(new ContactModel(R.drawable.b5,"E","9980904516"));
            arrContacts.add(new ContactModel(R.drawable.a6,"F","9980904517"));
            arrContacts.add(new ContactModel(R.drawable.a2,"G","9980904512"));
            arrContacts.add(new ContactModel(R.drawable.a3,"H","9980904514"));
            arrContacts.add(new ContactModel(R.drawable.a4,"I","9980904515"));
            arrContacts.add(new ContactModel(R.drawable.b5,"J","9980904516"));
            arrContacts.add(new ContactModel(R.drawable.a6,"K","9980904517"));
            arrContacts.add(new ContactModel(R.drawable.a2,"L","9980904512"));
            arrContacts.add(new ContactModel(R.drawable.a3,"M","9980904514"));
            arrContacts.add(new ContactModel(R.drawable.a4,"N","9980904515"));
            arrContacts.add(new ContactModel(R.drawable.b5,"M","9980904516"));
            arrContacts.add(new ContactModel(R.drawable.a6,"O","9980904517"));
            RecyclerContactAdapter adapter = new RecyclerContactAdapter(this,arrContacts);
            recyclerView.setAdapter(adapter);

            return insets;
        });
    }

}
```

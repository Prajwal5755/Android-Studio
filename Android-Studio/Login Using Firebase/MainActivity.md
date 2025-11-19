```
package com.example.login;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import com.google.firebase.auth.FirebaseAuth;
import com.google.firebase.auth.FirebaseUser;

 public class MainActivity extends AppCompatActivity {
    FirebaseAuth auth;
    Button button;
    TextView textView;
    FirebaseUser user;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            auth= FirebaseAuth.getInstance();//unique id is created
            button=findViewById(R.id.logout);
            textView = findViewById(R.id.user_details);
            user=auth.getCurrentUser();// current user details
            if (user == null){
                Intent intent = new Intent(getApplicationContext(), Login.class);
                startActivity(intent);
                finish(); // jump to login page
            }else {
               textView.setText(user.getEmail());// display email in mainactivity
            }
            button.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                   FirebaseAuth.getInstance().signOut();
                    Intent intent = new Intent(getApplicationContext(), Login.class);
                    startActivity(intent);
                    finish();// jump to login if signout
                }
            });
            return insets;
        });
    }

}
```

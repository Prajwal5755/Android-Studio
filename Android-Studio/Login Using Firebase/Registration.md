```
package com.example.login;

import android.content.Intent;
import android.os.Bundle;
import android.text.TextUtils;
import android.view.View;
import android.widget.Button;
import android.widget.ProgressBar;
import android.widget.TextView;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import com.google.android.gms.tasks.OnCompleteListener;
import com.google.android.gms.tasks.Task;
import com.google.android.material.textfield.TextInputEditText;
import com.google.firebase.auth.AuthResult;
import com.google.firebase.auth.FirebaseAuth;
import com.google.firebase.auth.FirebaseUser;

public class Registration extends AppCompatActivity {
    TextInputEditText editTextEmail, editTextPassword;
    Button buttonReg;
    FirebaseAuth mAuth;
    ProgressBar progressBar;
    TextView loginNow;

    @Override
    public void onStart() {
        super.onStart();
        FirebaseUser currentUser = mAuth.getCurrentUser();
        if (currentUser != null) {
            Toast.makeText(getApplicationContext(), "Login Successful", Toast.LENGTH_SHORT).show();
            Intent intent = new Intent(getApplicationContext(), MainActivity.class);
            startActivity(intent);
            finish();
        }
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_registration);

        // Initialize mAuth here, at the top of onCreate()
        mAuth = FirebaseAuth.getInstance();
        progressBar = findViewById(R.id.progressBar);

        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);

            loginNow = findViewById(R.id.loginNow);
            loginNow.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    Intent intent = new Intent(getApplicationContext(), Login.class);
                    startActivity(intent);
                    finish();
                }
            });

            editTextEmail = findViewById(R.id.email);
            editTextPassword = findViewById(R.id.password);
            buttonReg = findViewById(R.id.register_btn);

            buttonReg.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    progressBar.setVisibility(View.VISIBLE);// loading while fetching
                    String email, password;
                    email = String.valueOf(editTextEmail.getText());
                    password = String.valueOf(editTextPassword.getText());// user input store in string
                    if (TextUtils.isEmpty(email)) {
                        Toast.makeText(Registration.this, "Enter email", Toast.LENGTH_SHORT).show();
                        progressBar.setVisibility(View.GONE);
                        return;
                    }
                    if (TextUtils.isEmpty(password)) {
                        Toast.makeText(Registration.this, "Enter password", Toast.LENGTH_SHORT).show();
                        progressBar.setVisibility(View.GONE);
                        return;
                    }
                    mAuth.createUserWithEmailAndPassword(email, password)
                            .addOnCompleteListener(new OnCompleteListener<AuthResult>() {//to handle asyncronised tasks
                                @Override
                                public void onComplete(@NonNull Task<AuthResult> task) {
                                    progressBar.setVisibility(View.GONE);//after successful register  progress bar is gone
                                    if (task.isSuccessful()) {
                                        Toast.makeText(Registration.this, "Account Created", Toast.LENGTH_SHORT).show();// diplay account created
                                        // Auto Login and move to main activity.
                                        mAuth.signInWithEmailAndPassword(email, password)
                                                .addOnCompleteListener(authTask -> {
                                                    if (authTask.isSuccessful()) {
                                                        Intent intent = new Intent(getApplicationContext(), MainActivity.class);
                                                        startActivity(intent);
                                                        finish();
                                                    } else {
                                                        Toast.makeText(Registration.this, "Failed to Auto Login", Toast.LENGTH_SHORT).show();
                                                    }

                                                });

                                    } else {
                                        Toast.makeText(Registration.this, "Authentication failed. " + task.getException(), Toast.LENGTH_SHORT).show();
                                    }
                                }
                            });
                }
            });
            return insets;
        });
    }

}
```

---
title: Android入门
date: 2023-11-02 21:09:03
tags: Android
---

# Android入门

```java
package com.zypc.javaandroiddemo;

import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.view.Gravity;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button button=(Button)findViewById(R.id.button); //按钮ID为"button"
        EditText editTextText=(EditText)findViewById(R.id.editTextText); //输入框ID为"editTextText"
        button.setOnClickListener(new OnClickListener() {
            @Override
            public void onClick(View view) {
                String gotText=editTextText.getText().toString();
                if(gotText.equals("aaa")){
                    Toast toast=Toast.makeText(MainActivity.this,"Flag正确！", Toast.LENGTH_SHORT);
                    toast.setGravity(Gravity.BOTTOM|Gravity.CENTER,0,0);
                    toast.show();
                }
            }
        });
    }
}
```


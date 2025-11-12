<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Base.Theme.Utshikma" parent="Theme.Material3.DayNight.NoActionBar">
        <item name="colorPrimaryVariant">#3700B3</item>
        <item name="colorOnPrimary">#FFFFFF</item>
        <item name="colorSecondary">#03DAC5</item>
    </style>

    <style name="Theme.Utshikma" parent="Base.Theme.Utshikma" />
</resources>

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity"
    android:background="@android:color/white">

    <!-- Header -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="#6200EE"
        android:padding="16dp">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Praktikum - UTS"
            android:textColor="@android:color/white"
            android:textStyle="bold"
            android:textSize="18sp" />
    </LinearLayout>

    <!-- Isi Utama -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:orientation="horizontal"
        android:padding="16dp">

        <!-- Kolom Kiri -->
        <LinearLayout
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="1"
            android:orientation="vertical">

            <EditText
                android:id="@+id/edtNilai1"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:hint="Masukkan Nilai"
                android:inputType="numberDecimal"
                android:minHeight="48dp" />

            <EditText
                android:id="@+id/edtNilai2"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginTop="8dp"
                android:hint="Masukkan Nilai"
                android:inputType="numberDecimal"
                android:minHeight="48dp" />

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginTop="12dp"
                android:text="Hasil"
                android:textStyle="bold"
                android:textSize="16sp" />

            <EditText
                android:id="@+id/edtHasil"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:enabled="false"
                android:hint="Hasil Perhitungan"
                android:minHeight="48dp" />
        </LinearLayout>

        <!-- Kolom Kanan -->
        <RadioGroup
            android:id="@+id/radioGroup"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginStart="16dp"
            android:orientation="vertical">

            <RadioButton
                android:id="@+id/rbTambah"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:minHeight="48dp"
                android:text="Tambah" />

            <RadioButton
                android:id="@+id/rbKurang"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:minHeight="48dp"
                android:text="Kurang" />

            <RadioButton
                android:id="@+id/rbKali"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:minHeight="48dp"
                android:text="Kali" />

            <RadioButton
                android:id="@+id/rbBagi"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:minHeight="48dp"
                android:text="Bagi" />
        </RadioGroup>
    </LinearLayout>

    <!-- Tombol CLEAR -->
    <Button
        android:id="@+id/btnClear"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="end"
        android:layout_margin="16dp"
        android:minHeight="48dp"
        android:minWidth="48dp"
        android:backgroundTint="#6200EE"
        android:paddingHorizontal="24dp"
        android:paddingVertical="12dp"
        android:text="CLEAR"
        android:textAllCaps="true"
        android:textColor="@android:color/white"
        android:textStyle="bold" />

</LinearLayout>


package com.example.utshikma;

import androidx.appcompat.app.AppCompatActivity;
import androidx.core.content.ContextCompat;
import android.os.Bundle;
import android.text.Editable;
import android.text.TextWatcher;
import android.widget.Button;
import android.widget.EditText;
import android.widget.RadioButton;
import android.widget.RadioGroup;
import android.content.res.ColorStateList;
import android.graphics.Color;

public class MainActivity extends AppCompatActivity {

    EditText edtNilai1, edtNilai2, edtHasil;
    RadioGroup radioGroup;
    RadioButton rbTambah, rbKurang, rbKali, rbBagi;
    Button btnClear;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        edtNilai1 = findViewById(R.id.edtNilai1);
        edtNilai2 = findViewById(R.id.edtNilai2);
        edtHasil = findViewById(R.id.edtHasil);
        radioGroup = findViewById(R.id.radioGroup);
        rbTambah = findViewById(R.id.rbTambah);
        rbKurang = findViewById(R.id.rbKurang);
        rbKali = findViewById(R.id.rbKali);
        rbBagi = findViewById(R.id.rbBagi);
        btnClear = findViewById(R.id.btnClear);

        // 💠 Buat warna hanya aktif saat dipilih (checked)
        int[][] states = new int[][]{
                new int[]{android.R.attr.state_checked},   // ketika dipilih
                new int[]{}                               // default
        };
        int[] colors = new int[]{
                Color.parseColor("#03DAC5"),  // biru toska saat dipilih
                Color.GRAY                    // abu-abu saat belum dipilih
        };
        ColorStateList colorStateList = new ColorStateList(states, colors);

        rbTambah.setButtonTintList(colorStateList);
        rbKurang.setButtonTintList(colorStateList);
        rbKali.setButtonTintList(colorStateList);
        rbBagi.setButtonTintList(colorStateList);

        edtNilai1.addTextChangedListener(textWatcher);
        edtNilai2.addTextChangedListener(textWatcher);

        radioGroup.setOnCheckedChangeListener((group, checkedId) -> hitung());

        btnClear.setOnClickListener(v -> {
            edtNilai1.setText("");
            edtNilai2.setText("");
            edtHasil.setText("");
            radioGroup.clearCheck();
        });
    }

    private final TextWatcher textWatcher = new TextWatcher() {
        @Override
        public void beforeTextChanged(CharSequence s, int start, int count, int after) {}

        @Override
        public void onTextChanged(CharSequence s, int start, int before, int count) {
            hitung();
        }

        @Override
        public void afterTextChanged(Editable s) {}
    };

    private void hitung() {
        String s1 = edtNilai1.getText().toString();
        String s2 = edtNilai2.getText().toString();

        if (s1.isEmpty() || s2.isEmpty()) {
            edtHasil.setText("");
            return;
        }

        double n1 = Double.parseDouble(s1);
        double n2 = Double.parseDouble(s2);
        double hasil = 0;

        if (rbTambah.isChecked()) hasil = n1 + n2;
        else if (rbKurang.isChecked()) hasil = n1 - n2;
        else if (rbKali.isChecked()) hasil = n1 * n2;
        else if (rbBagi.isChecked()) hasil = (n2 != 0) ? n1 / n2 : 0;

        edtHasil.setText(String.valueOf(hasil));
    }
}

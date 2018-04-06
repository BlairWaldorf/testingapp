# testingapp

package jeffersonmendes.buttonscreenoff;

import android.content.Intent;
import android.net.Uri;
import android.provider.Settings;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.RadioButton;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    RadioButton rb1;
    Boolean pass;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    @Override
    protected void onResume( ) {
        super.onResume();

        rb1 = findViewById(R.id.rb1);
        rb2 = findViewById(R.id.rb2);
        rb3 = findViewById(R.id.rb3);

        rb1.setChecked(false);
        rb2.setChecked(false);
        rb3.setChecked(false);

        rb1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                setScreenTimeout(15000);
                if(pass) {
                    rb1.setChecked(true);
                    rb2.setChecked(false);
                    rb3.setChecked(false);

                    Toast.makeText(MainActivity.this, "15 seconds screen off timeout.", Toast.LENGTH_SHORT).show();
                    pass = false;
                }else{
                    rb1.setChecked(false);
                    rb2.setChecked(false);
                    rb3.setChecked(false);
                }
            }
        });
    }

        private void setScreenTimeout(int milliseconds){
        boolean value = false;
            if (android.os.Build.VERSION.SDK_INT >= android.os.Build.VERSION_CODES.M) {
                value = Settings.System.canWrite(getApplicationContext());
            if(value){
                Settings.System.putInt(getContentResolver(), Settings.System.SCREEN_OFF_TIMEOUT, milliseconds);
                pass = true;
            } else {
                Intent intent = new Intent(Settings.ACTION_MANAGE_WRITE_SETTINGS);
                intent.setData(Uri.parse("package:" + getApplicationContext().getPackageName()));
            }
        } else {
                Settings.System.putInt(getContentResolver(), Settings.System.SCREEN_OFF_TIMEOUT, milliseconds);
                pass = true;
            }
    }
}


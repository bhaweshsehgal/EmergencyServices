# EmergencyServices
package com.example.emergencyservices;

import java.util.List;

import android.net.Uri;
import android.os.Bundle;
import android.app.Activity;
import android.content.Intent;
import android.content.SharedPreferences;
import android.content.pm.PackageManager;
import android.content.pm.ResolveInfo;
import android.telephony.SmsManager;
import android.view.Menu;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

public class Fire extends Activity {

	public static final String PREFS_NAME="CONTACTS";
	public String numberOne,numberTwo,numberThree;
	
	//TO CALL THE FIRE STATION
	public void fireCall(View view){
		Uri number = Uri.parse("tel:102");
	    Intent callIntent = new Intent(Intent.ACTION_CALL, number);
	    	
	    PackageManager packageManager = getPackageManager();
	    List<ResolveInfo> activities = packageManager.queryIntentActivities(callIntent, 0);
	    boolean isIntentSafe = activities.size() > 0;
	    	
	    if(isIntentSafe)
	    	startActivity(callIntent);
	}
		
	
	
	//TO SEND DISTRESS MESSAGE FOR FIRE
	/*public void fireMessage(){
		
		SharedPreferences settings = getSharedPreferences(PREFS_NAME,0);
		String numberOne=settings.getString("numberOne", "911");
		String numberTwo=settings.getString("numberTwo", "911");
		String numberThree=settings.getString("numberThree", "911");
		
		String message="The sender has invoked"+
		" fire emergency";
		
		 PendingIntent pi = PendingIntent.getActivity(this, 0,
		            new Intent(this, Fire.class), 0);
		
		SmsManager smsManager = SmsManager.getDefault();
		smsManager.sendTextMessage(numberOne, null, message, pi, null);
		smsManager.sendTextMessage(numberTwo, null, message, pi, null);
		smsManager.sendTextMessage(numberThree, null, message, pi, null);
	}*/
		
		
	//TO FIND THE NEAREST FIRE STATION
	public void fireMap(View view){
		Uri location = Uri.parse("geo:0,0?q=Fire+Station");
		Intent mapIntent = new Intent(Intent.ACTION_VIEW, location);

		// Verify it resolves
		PackageManager packageManager = getPackageManager();
		List<ResolveInfo> activities = packageManager.queryIntentActivities(mapIntent, 0);
		boolean isIntentSafe = activities.size() > 0;
			  
		// Start an activity if it's safe
		if (isIntentSafe) {
		    startActivity(mapIntent);
		}
	}
		
	
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_fire);
		
		SharedPreferences settings = getSharedPreferences(PREFS_NAME,0);
		numberOne=settings.getString("numberOne", "911");
		numberTwo=settings.getString("numberTwo", "911");
		numberThree=settings.getString("numberThree", "911");
		
		Button send = (Button) findViewById(R.id.sendFire);
        send.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                
 
                try {
                    String message = "The sender has invoked"+
                    		" police emergency";
                	SmsManager smsManager = SmsManager.getDefault();
                	smsManager.sendTextMessage(numberOne, null, message, null, null);
                    smsManager.sendTextMessage(numberTwo, null, message, null, null);
                    smsManager.sendTextMessage(numberThree, null, message, null, null);
                    Toast.makeText(getApplicationContext(), "SMS Sent!",
                            Toast.LENGTH_LONG).show();
                } catch (Exception e) {
                    Toast.makeText(getApplicationContext(),
                            "SMS faild, please try again later!",
                            Toast.LENGTH_LONG).show();
                    e.printStackTrace();
                }
            }
        });
	}

	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
		// Inflate the menu; this adds items to the action bar if it is present.
		getMenuInflater().inflate(R.menu.fire, menu);
		return true;
	}

}

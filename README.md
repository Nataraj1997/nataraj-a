
package com.sourcey.materiallogindemo;

import android.app.ProgressDialog;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.util.Log;

import android.content.Intent;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

import butterknife.ButterKnife;
import butterknife.InjectView;

public class LoginActivity extends AppCompatActivity {
    private static final String TAG = "LoginActivity";
    private static final int REQUEST_SIGNUP = 0;

    @InjectView(R.id.input_email) EditText _emailText;
    @InjectView(R.id.input_password) EditText _passwordText;
    @InjectView(R.id.btn_login) Button _loginButton;
    @InjectView(R.id.link_signup) TextView _signupLink;
    
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login);
        ButterKnife.inject(this);
        
        _loginButton.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View v) {
                login();
            }
        });

        _signupLink.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View v) {
                // Start the Signup activity
                Intent intent = new Intent(getApplicationContext(), SignupActivity.class);
                startActivityForResult(intent, REQUEST_SIGNUP);
            }
        });
    }

    public void login() {
        Log.d(TAG, "Login");
        
        if (!validate()) {
            onLoginFailed();
            return;
        }

        _loginButton.setEnabled(false);

        final ProgressDialog progressDialog = new ProgressDialog(LoginActivity.this,
                R.style.AppTheme_Dark_Dialog);
        progressDialog.setIndeterminate(true);
        progressDialog.setMessage("Authenticating...");
        progressDialog.show();

        String email = _emailText.getText().toString();
        String password = _passwordText.getText().toString();

        // TODO: Implement your own authentication logic here.

        new android.os.Handler().postDelayed(
                new Runnable() {
                    public void run() {
                        // On complete call either onLoginSuccess or onLoginFailed
                        onLoginSuccess();
                        // onLoginFailed();
                        progressDialog.dismiss();
                    }
                }, 3000);
    }


    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        if (requestCode == REQUEST_SIGNUP) {
            if (resultCode == RESULT_OK) {

                // TODO: Implement successful signup logic here
                // By default we just finish the Activity and log them in automatically
                this.finish();
            }
        }
    }

    @Override
    public void onBackPressed() {
        // disable going back to the MainActivity
        moveTaskToBack(true);
    }

    public void onLoginSuccess() {
        _loginButton.setEnabled(true);
        finish();
    }

    public void onLoginFailed() {
        Toast.makeText(getBaseContext(), "Login failed", Toast.LENGTH_LONG).show();

        _loginButton.setEnabled(true);
    }

    public boolean validate() {
        boolean valid = true;

        String email = _emailText.getText().toString();
        String password = _passwordText.getText().toString();

        if (email.isEmpty() || !android.util.Patterns.EMAIL_ADDRESS.matcher(email).matches()) {
            _emailText.setError("enter a valid email address");
            valid = false;
        } else {
            _emailText.setError(null);
        }

        if (password.isEmpty() || password.length() < 4 || password.length() > 10) {
            _passwordText.setError("between 4 and 10 alphanumeric characters");
            valid = false;
        } else {
            _passwordText.setError(null);
        }

        return valid;
    }
}
<activity android:name=".SearchActivity"
    android:screenOrientation="landscape">
    <intent-filter>
        <action android:name="android.intent.action.MAIN"/>
        <category android:name="android.intent.category.LAUNCHER"/>
    </intent-filter>
</activity>
 
<activity android:name=".PlayerActivity"
    android:screenOrientation="landscape" />
<resources>
    <string name="app_name">SimplePlayer</string>
    <string name="search">Search</string>
    <string name="failed">Failed to initialize Youtube Player</string>
</resources>
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >
     
    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="@string/search"
        android:id="@+id/search_input"
        android:singleLine="true"
        />
     
    <ListView
        android:id="@+id/videos_found"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:dividerHeight="5dp"
        />
         
</LinearLayout>
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp"
    >
     
    <ImageView
        android:id="@+id/video_thumbnail"
        android:layout_width="128dp"
        android:layout_height="128dp"
        android:layout_alignParentLeft="true"
        android:layout_alignParentTop="true"
        android:layout_marginRight="20dp"
        />
     
    <TextView android:id="@+id/video_title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@+id/video_thumbnail"
        android:layout_alignParentTop="true"
        android:layout_marginTop="5dp"
        android:textSize="25sp"
        android:textStyle="bold"
        />
     
    <TextView android:id="@+id/video_description"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@+id/video_thumbnail"
        android:layout_below="@+id/video_title"
        android:textSize="15sp"       
        />
 
</RelativeLayout>
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >
     
    <com.google.android.youtube.player.YouTubePlayerView       
        android:id="@+id/player_view"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
     
</LinearLayout>
package com.hathi.simpleplayer;
 
public class VideoItem {
    private String title;
    private String description;
    private String thumbnailURL;
    private String id;
     
    public String getId() {
        return id;
    }
     
    public void setId(String id) {
        this.id = id;
    }
     
    public String getTitle() {
        return title;
    }
     
    public void setTitle(String title) {
        this.title = title;
    }
     
    public String getDescription() {
        return description;
    }
     
    public void setDescription(String description) {
        this.description = description;
    } 
     
    public String getThumbnailURL() {
        return thumbnailURL;
    }
     
    public void setThumbnailURL(String thumbnail) {
        this.thumbnailURL = thumbnail;      
    }
         
}
package com.hathi.simpleplayer;
 
public class YoutubeConnector {
    private YouTube youtube; 
    private YouTube.Search.List query;
     
    // Your developer key goes here
    public static final String KEY 
        = "AIzaSQZZQWQQWMGziK9H_qRxz8g-V6eDL3QW_Us";
     
    public YoutubeConnector(Context context) { 
        youtube = new YouTube.Builder(new NetHttpTransport(), 
                new JacksonFactory(), new HttpRequestInitializer() {            
            @Override
            public void initialize(HttpRequest hr) throws IOException {}
        }).setApplicationName(content.getString(R.string.app_name)).build();
         
        try{
            query = youtube.search().list("id,snippet");
            query.setKey(KEY);          
            query.setType("video");
            query.setFields("items(id/videoId,snippet/title,snippet/description,snippet/thumbnails/default/url)");                              
        }catch(IOException e){
            Log.d("YC", "Could not initialize: "+e);
        }
    }
}
public List<VideoItem> search(String keywords){
    query.setQ(keywords);       
    try{
        SearchListResponse response = query.execute();
        List<SearchResult> results = response.getItems();
         
        List<VideoItem> items = new ArrayList<VideoItem>();
        for(SearchResult result:results){
            VideoItem item = new VideoItem();
            item.setTitle(result.getSnippet().getTitle());
            item.setDescription(result.getSnippet().getDescription());
            item.setThumbnailURL(result.getSnippet().getThumbnails().getDefault().getUrl());
            item.setId(result.getId().getVideoId());
            items.add(item);            
        }
        return items;
    }catch(IOException e){
        Log.d("YC", "Could not search: "+e);
        return null;
    }       
}
public class SearchActivity extends Activity {
 
    private EditText searchInput;
    private ListView videosFound;
     
    private Handler handler;
     
    @Override
    protected void onCreate(Bundle savedInstanceState) {    
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_search);
         
        searchInput = (EditText)findViewById(R.id.search_input);
        videosFound = (ListView)findViewById(R.id.videos_found); 
         
        handler = new Handler();
         
        searchInput.setOnEditorActionListener(new TextView.OnEditorActionListener() {           
            @Override
            public boolean onEditorAction(TextView v, int actionId, KeyEvent event) {           
                if(actionId == EditorInfo.IME_ACTION_DONE){
                    searchOnYoutube(v.getText().toString());
                    return false;
                }
                return true;
            }
        });
                 
    }
}
private List<VideoItem> searchResults;
 
private void searchOnYoutube(final String keywords){
        new Thread(){
            public void run(){
                YoutubeConnector yc = new YoutubeConnector(SearchActivity.this);
                searchResults = yc.search(keywords);                
                handler.post(new Runnable(){
                    public void run(){
                        updateVideosFound();
                    }
                });
            }
        }.start();
    }
private void updateVideosFound(){
    ArrayAdapter<VideoItem> adapter = new ArrayAdapter<VideoItem>(getApplicationContext(), R.layout.video_item, searchResults){
        @Override
        public View getView(int position, View convertView, ViewGroup parent) {
            if(convertView == null){
                convertView = getLayoutInflater().inflate(R.layout.video_item, parent, false);
            }
            ImageView thumbnail = (ImageView)convertView.findViewById(R.id.video_thumbnail);
            TextView title = (TextView)convertView.findViewById(R.id.video_title);
            TextView description = (TextView)convertView.findViewById(R.id.video_description);
             
            VideoItem searchResult = searchResults.get(position);
             
            Picasso.with(getApplicationContext()).load(searchResult.getThumbnailURL()).into(thumbnail);
            title.setText(searchResult.getTitle());
            description.setText(searchResult.getDescription());
            return convertView;
        }
    };          
     
    videosFound.setAdapter(adapter);
}
private void addClickListener(){
    videosFound.setOnItemClickListener(new AdapterView.OnItemClickListener() {
 
        @Override
        public void onItemClick(AdapterView<?> av, View v, int pos,
                long id) {              
            Intent intent = new Intent(getApplicationContext(), PlayerActivity.class);
            intent.putExtra("VIDEO_ID", searchResults.get(pos).getId());
            startActivity(intent);
        }
         
    });
}
public class PlayerActivity extends YouTubeBaseActivity implements OnInitializedListener {
     
    private YouTubePlayerView playerView;
     
    @Override
    protected void onCreate(Bundle bundle) {
        super.onCreate(bundle);
         
        setContentView(R.layout.activity_player);
 
        playerView = (YouTubePlayerView)findViewById(R.id.player_view);
        playerView.initialize(YoutubeConnector.KEY, this);             
    }
 
    @Override
    public void onInitializationFailure(Provider provider,
            YouTubeInitializationResult result) {
        Toast.makeText(this, getString(R.string.failed), Toast.LENGTH_LONG).show();
    }
 
    @Override
    public void onInitializationSuccess(Provider provider, YouTubePlayer player,
            boolean restored) {
        if(!restored){          
            player.cueVideo(getIntent().getStringExtra("VIDEO_ID"));
        }
    }
}
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:background="#000000"
    tools:context=".MainActivity" >
 
    <VideoView
        android:id="@+id/myVideo"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:layout_centerInParent="true" />
         
</RelativeLayout>
import android.net.Uri;
import android.widget.MediaController;
import android.widget.VideoView;
@Override
protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
}
String vidAddress = "https://archive.org/download/ksnn_compilation_master_the_internet/ksnn_compilation_master_the_internet_512kb.mp4";
Uri vidUri = Uri.parse(vidAddress);
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#000000"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context=".MainActivity" >
 
    <SurfaceView
        android:id="@+id/surfView"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
 
</RelativeLayout>
@Override
public void surfaceChanged(SurfaceHolder arg0, int arg1, int arg2, int arg3) {
    // TODO Auto-generated method stub
}
 
@Override
public void surfaceCreated(SurfaceHolder arg0) {
//setup
}
 
@Override
public void surfaceDestroyed(SurfaceHolder arg0) {
    // TODO Auto-generated method stub
}
 
@Override
public void onPrepared(MediaPlayer mp) {
//start playback
}
private MediaPlayer mediaPlayer;
private SurfaceHolder vidHolder;
private SurfaceView vidSurface;
String vidAddress = "https://archive.org/download/ksnn_compilation_master_the_internet/ksnn_compilation_master_the_internet_512kb.mp4";
try {
    mediaPlayer = new MediaPlayer();
    mediaPlayer.setDisplay(vidHolder);
    mediaPlayer.setDataSource(vidAddress);
    mediaPlayer.prepare();
    mediaPlayer.setOnPreparedListener(this);
    mediaPlayer.setAudioStreamType(AudioManager.STREAM_MUSIC);
} 
catch(Exception e){
    e.printStackTrace();
}

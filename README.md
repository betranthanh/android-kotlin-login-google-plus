# Android Kotlin Login Google Plus

[![Watch the video](https://i.imgur.com/forSFU8.png)](https://youtu.be/RpHQBpkFbJs)

### 1. Create config file 
```google-services.json```
https://developers.google.com/mobile/add?platform=android

### 2. Add the Google Services plugin
>##### 1 Add the dependency to your project-level build.gradle:
```java
classpath 'com.google.gms:google-services:3.0.0'
```
>##### 2 Add the plugin to your app-level build.gradle:
```java
apply plugin: 'com.google.gms.google-services'
``` 
>##### 3 Add Google Play Services
```java
dependencies {
    compile 'com.google.android.gms:play-services-auth:9.8.0'
}
```

### 3. Let start to add some code

>##### 1. Sign in
```java
val gso = GoogleSignInOptions.Builder(GoogleSignInOptions.DEFAULT_SIGN_IN)
        .requestEmail()
        .build()

mGoogleApiClient = GoogleApiClient.Builder(this)
        .enableAutoManage(this, this)
        .addApi(Auth.GOOGLE_SIGN_IN_API, gso)
        .build()

btnLogin?.setOnClickListener(View.OnClickListener {
    var signInIntent = Auth.GoogleSignInApi.getSignInIntent(mGoogleApiClient)
    startActivityForResult(signInIntent, RC_SIGN_IN)
})

override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
    super.onActivityResult(requestCode, resultCode, data)

    if (requestCode == RC_SIGN_IN) {
        var result = Auth.GoogleSignInApi.getSignInResultFromIntent(data)
        updateUI(result.isSuccess)
    }
}
```

>##### 2. Sign out
```java
btnLogout?.setOnClickListener(View.OnClickListener {
    Auth.GoogleSignInApi.signOut(mGoogleApiClient).setResultCallback(
            object : ResultCallback<Status> {
                override fun onResult(status: Status) {
                    updateUI(status.isSuccess)
                }

            }
    )
})
```

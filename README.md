<h1 align="center"> Android-NewPermissionRequest </h1>

<p align="center">
As of June 2021, there have been some changes related to user permissions on Android. The methods 'startActivityForResult', 'onActivityResult' and on Permission Result' have been deprecated. we will look at how to request user permissions with newly added methods in this repo.
</p>

<p align="center">
  <img src='https://github.com/ayhanunal/Android-NewPermissionRequest/blob/main/ss/1.jpg' width=200 heihgt=300> 
  <img src='https://github.com/ayhanunal/Android-NewPermissionRequest/blob/main/ss/2.jpg' width=200 heihgt=300>
  <img src='https://github.com/ayhanunal/Android-NewPermissionRequest/blob/main/ss/3.jpg' width=200 heihgt=300>
</p>

### Manifest
Add the permission you want to use to your `Manifest` file
```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"></uses-permission>
```

### MainActivity.kt
Create variables activityResultLauncher and permissionLauncher to define later
```kotlin
private lateinit var activityResultLauncher: ActivityResultLauncher<Intent>
private lateinit var permissionLauncher: ActivityResultLauncher<String>
```

Create a function where we initialize variables
```kotlin
private fun registerLauncher(){
    activityResultLauncher = registerForActivityResult(ActivityResultContracts.StartActivityForResult()){ result ->
        if (result.resultCode == AppCompatActivity.RESULT_OK){
            storage_permission_text.text = "Read External Storage: True"
        }
    }
    permissionLauncher = registerForActivityResult(ActivityResultContracts.RequestPermission()){ result ->
        if (result){
            //permission granted
            storage_permission_text.text = "Read External Storage: True"
        }else{
            //permission denied
            Toast.makeText(applicationContext, "Permisson needed!", Toast.LENGTH_LONG).show()
        }
    }

}
```
Call the registerLauncher function under `onCreate`
```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    registerLauncher()
}
```

Then let's ask the user for permission
```kotlin
permission_request_button.setOnClickListener {
    if (ContextCompat.checkSelfPermission(applicationContext, android.Manifest.permission.READ_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED){
        if (ActivityCompat.shouldShowRequestPermissionRationale(this, android.Manifest.permission.READ_EXTERNAL_STORAGE)) {
            permissionLauncher.launch(android.Manifest.permission.READ_EXTERNAL_STORAGE)
        }else{
            permissionLauncher.launch(android.Manifest.permission.READ_EXTERNAL_STORAGE)
        }

    }else{
        storage_permission_text.text = "Read External Storage: True"
    }
}
```





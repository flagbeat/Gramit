# Gramit

[ ![Download](https://api.bintray.com/packages/flagbeatinc/Gramit/com.flagbeat.gramit/images/download.svg?version=1.1.3) ](https://bintray.com/flagbeatinc/Gramit/com.flagbeat.gramit/1.1.3/link)

> A library to simplify runtime permission handling in Android

## Demo Screens
<img src="https://flagbeat.github.io/Gramit/Screenshots/gramit_overview.gif" height="500">	<img src="https://flagbeat.github.io/Gramit/Screenshots/1_gramit_permission_dialogue.png" height="500">	<img src="https://flagbeat.github.io/Gramit/Screenshots/2_gramit_permission_dialogue.png" height="500">	<img src="https://flagbeat.github.io/Gramit/Screenshots/3_gramit_permission.png" height="500">	<img src="https://flagbeat.github.io/Gramit/Screenshots/4_gramit_mandatory_permission_dialogue.png" height="500">	<img src="https://flagbeat.github.io/Gramit/Screenshots/5_gramit_mandatory_permission_open_settings.png" height="500">

## Features
- Out of the box permission handling with less boilerplate code.
- Configurable permission dialogue to explain why you need permissions.
- Permission dialogue shows carousel having cards with thumbnail and message related to a particular permission.
- Enable IsMendatory flag to force the user for granting a mendatory permission.
- In case of mendatory permission, even if a user chooses "Never ask again", the library shows a blocking dialogue that takes the user to the Phone settings > Permission section.
- Returns accepted, denied and declined (Never ask again) permissions and a grant code.

## Installation
#### Gradle
Add this Gradle dependency to your **app/build.gradle** file
```
implementation "com.flagbeat.gramit:gramit:1.1.3"
```
#### Maven
```
<dependency>
	<groupId>com.flagbeat.gramit</groupId>
	<artifactId>gramit</artifactId>
	<version>1.1.3</version>
	<type>pom</type>
</dependency>
```
## Usage
1. Create a list of the required permissions 
```java
List<Permission> permissions = new ArrayList<>();
// Read Log Permission
permissions.add(
    Permission.builder()
              .permissionName(Manifest.permission.READ_CALL_LOG)
              .popupMessage("Add your call rationale here")
              .isMandatory(false)
              .popupThumbnailResourceId(R.drawable.default_permission_icon)
              .build()
);
// Read Contact Permission
permissions.add(
    Permission.builder()
              .permissionName(Manifest.permission.READ_CONTACTS)
              .popupMessage("Add your contacts rationale here")
              .isMandatory(true)
              .popupThumbnailRemoteUrl("https://flagbeat.com/img/flagbeat_loader.png")
              .build()
);
```
2. Pass the permission list to the Gramit library
``` 
Gramit.builder()
      .context(this)
      .permissions(permissions)
      .showContextualMessage(true)
      .openSettingText("Go to App permissions and enable all the required permissions.")
      .popupOkButtonText("Got it")
      .build();
```
3. **(Optional)** In the activity, you will get the grant status callback in ```onActivityResult``` method. Received intent contains grant code, lists of granted permissions, denied permissions and declined permissions (Never ask again permissions).
**Grant Code:**
    1. Gramit.FULL_PERMISSION_GRANT
    2. Gramit.PARTIAL_PERMISSION_GRANT
    3. Gramit.NO_PERMISSION_GRANT

```java
    @Override
    public void onActivityResult(int requestCode, int resultCode,
                                 Intent data)
    {
        if (RESULT_OK == resultCode)
        {
            switch (requestCode)
            {
                case Gramit.GRAMIT_REQUEST_CODE:
                    handleGramitResponse(data);
                    break;
            }
        }
    }
    
    private void handleGramitResponse(Intent data)
    {
        Log.i("Grant Code ",
              String.valueOf(data.getIntExtra(Keys.GRANT_CODE, 0)));
        Log.i("GRANTED_PERMISSIONS ",
              Arrays.toString(data.getStringArrayExtra(Keys.GRANTED_PERMISSIONS)));
        Log.i("DENIED_PERMISSIONS ",
              Arrays.toString(data.getStringArrayExtra(Keys.DENIED_PERMISSIONS)));
        Log.i("DECLINED_PERMISSIONS ",
              Arrays.toString(data.getStringArrayExtra(Keys.DECLINED_PERMISSIONS)));
    }
```
That's it!
## Compatibility
minSdkVersion 16

## Progaurd
- Add OkHttp's rules: https://github.com/square/okhttp/#r8--proguard

## Debugging
If you are not getting a permission specific card in the permission dialogue carousel, then check below:
- Check if you have declared the permission in the Manifest file.
- Check if you have added the permission in the permission list before submitting it to Gramit library.
- Raise a bug [here](https://github.com/flagbeat/gramit/issues) otherwise as per this [template](https://github.com/flagbeat/Gramit/tree/master/.github/ISSUE_TEMPLATE).

## Who's using it
- [Flagbeat](https://www.flagbeat.com/)

## Developed by
- **Sumit Saurabh** (sumitsaurabh2293@gmail.com)
</br>[Find me on LinkedIn](https://www.linkedin.com/in/-sumitsaurabh/)

## Used libraries
- [Picasso](https://square.github.io/picasso/): It has been used to load remote url of thumbnails in permission dialogue.
- [Lombok](https://projectlombok.org/setup/android): It has been used for getter, setter and builder pattern but don't worry it is a compile-time only library, won't make your app 'heavier'.

## License
```
Copyright 2019 Sumit Saurabh

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

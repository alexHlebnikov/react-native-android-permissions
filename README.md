# react-native-android-permissions
React Native Android Permissions *experimental module* - Android M (6.0) permissions like to your React Native application.

If you need to work with Android M (6.0+) permissions model this experimental module that can be helpful to you. This works with only two methods: *checkPermission* and *requestPermission*.

First of all you need to set in your *android/app/build.gradle* file the *targetSdkVersion* to **23**:

```gradle
...
  defaultConfig {
      applicationId "com.awesomepermissions"
      minSdkVersion 16
      targetSdkVersion 23 // <--- this change
      versionCode 1
      versionName "1.0"
      ndk {
          abiFilters "armeabi-v7a", "x86"
      }
  }
```

After that, when you're testing your app in development mode, you have to allow the "Draw over other apps" permission:

![Setting the draw over permission](http://i.imgur.com/rdUzj0w.gif)

The recommended way of deal with permissions in Android is to preview and list all the permissions in your *AndroidManifest.xml* file:

```xml
...
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
  <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
...
```

Before that you can ask some of then in run time, when you need.

![Demo](http://i.imgur.com/bdMGD3d.gif)

### Installation

```bash
npm install react-native-android-permissions --save
```

### Add it to your android project

* In `android/setting.gradle`

```gradle
...
include ':RNPermissionsModule', ':app'
project(':RNPermissionsModule').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-android-permissions/android')
```

* In `android/app/build.gradle`

```gradle
...
dependencies {
    ...
    compile project(':RNPermissionsModule')
}
```

```java
import com.burnweb.rnpermissions.RNPermissionsPackage;  // <--- import

public class MainActivity extends ReactActivity {
  ......

  @Override
  protected List<ReactPackage> getPackages() {
    return Arrays.<ReactPackage>asList(
            new MainReactPackage(),
            new RNPermissionsPackage()); // <------ add this line to your MainActivity class
  }

  @Override
  public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
      RNPermissionsPackage.onRequestPermissionsResult(requestCode, permissions, grantResults); // very important event callback
      super.onRequestPermissionsResult(requestCode, permissions, grantResults);
  }

  ......

}
```

## Example of checkPermission method:

```javascript
import React, {
  AppRegistry,
  Component,
  StyleSheet,
  Text,
  View
} from 'react-native';

import {checkPermission} from 'react-native-android-permissions';

class AwesomePermissions extends Component {
  componentDidMount() {
    checkPermission("android.permission.ACCESS_FINE_LOCATION").then((result) => {
      console.log("Already Granted!");
      console.log(result);
    }, (result) => {
      console.log("Not Granted!");
      console.log(result);
    });
  }
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>
          Welcome to React Native!
        </Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
});

AppRegistry.registerComponent('AwesomePermissions', () => AwesomePermissions);
```

## Example of requestPermission method:

```javascript
import React, {
  AppRegistry,
  Component,
  StyleSheet,
  Text,
  View
} from 'react-native';

import {requestPermission} from 'react-native-android-permissions';

class AwesomePermissions extends Component {
  componentDidMount() {
    requestPermission("android.permission.ACCESS_FINE_LOCATION").then((result) => {
      console.log("Granted!", result);
      // now you can set the listenner to watch the user geo location
    }, (result) => {
      console.log("Not Granted!");
      console.log(result);
    });
  }
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>
          Welcome to React Native!
        </Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
});

AppRegistry.registerComponent('AwesomePermissions', () => AwesomePermissions);
```

## License
MIT

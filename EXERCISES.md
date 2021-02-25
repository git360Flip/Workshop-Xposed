# Workshop-Xposed
Workshop created by git360Flip for the Hub of Epitech

Documentation Xposed Framework: https://api.xposed.info/reference/packages.html

Android Code Search: https://cs.android.com/

# Exercise 1
Let's start by creating a simple hook without an interface.

1) Create a project using Android Studio

    - Select "No Activity"
    - Select either Java or Kotlin as language (tutorial is written in Java)

2) Edit your build.gradle file located in your app folder and add theses lines in your dependencies:

    ```javascript
    compileOnly 'de.robv.android.xposed:api:82'
    compileOnly 'de.robv.android.xposed:api:82:sources'
    ```
    Remember to always Sync your project with gradle when you modify your dependencies

3) Edit your AndroidManifest.xml (manifests folder) and add inside 'application':

    ```javascript
    <meta-data
        android:name="xposedmodule"
        android:value="true" />
    <meta-data
        android:name="xposeddescription"
        android:value="An example which changes the color of clock text in status bar" />
    <meta-data
        android:name="xposedminversion"
        android:value="53" />
    ```

4) Create a class and remember its name and the name of your package. This class will contains the code to modify the methods of the Android API, let's name it for the tutorial: XposedClass

5) Create xposed_init file

    - Create new repository called "assets" in the same location as your "res" folder
    - Create a file (with no extension) named "xposed_init"
    - Write into it the name of your XposedClass (example: com.myapp.test.XposedClass)

6) Modify your XposedClass

    - Add theses imports:
    ```javascript
    import de.robv.android.xposed.IXposedHookLoadPackage;
    import de.robv.android.xposed.callbacks.XC_LoadPackage;
    import de.robv.android.xposed.XposedBridge;
    ```

    - Implement IXposedHookLoadPackage for your XposedClass
    (search for keyword "implements" if you are new to Java)

    - Override the public method handleLoadPackage which takes a "LoadPackageParam"
    as parameter

    - Add this line inside your overriden method:
    ```javascript
    XposedBridge.log("Loaded app: " + lpparam.packageName);
    ```

7) Run the code

    - Press the start button with your emulator selected

    if you get "Error running 'app'":
        - Click on Run > Edit Configurations...
        - Change Launch 'Default Activity' to 'Nothing'
        - Press start again
    
    - Now go to the Xposed Installer application on the emulator
    - Select 'Modules' from the application's menu
    - Click on your application to activate it
    - Reboot your emulator to activate your Xposed module

If you now check your logs in Xposed Installer accessible from the menu,
You can see 'Loading class' followed by your package name.
All the 'Loaded app:' corresponds to the loading of each package of your system.
Each package initialisation is modified to log his name in addition of his initial initialisation.

# Exercise 2
Let's change the color of the current time on your emulator.

1) Find the package which contains Clock.java with an updateClock() method
(see Android Source Code - API 26)

2) Inside your XposedClass, when you handle the loading of each package, find and hook the updateClock() method of the package which contains Clock.java
(see Xposed documentation)

3) Override the Xposed's method called after the invocation of the updateClock() method from Clock.java
(see Xposed documentation)

4) Add theses lines in your hooked method of Xposed for updateClock():
    ```javascript
    TextView tv = (TextView) param.thisObject;
    tv.setTextColor(Color.RED);
    ```
5) Run the code on your emulator and reboot it

You will see every one minute, the text of the current time of your emulator displayed in red.

# Exercise 3
Let's modify the text of the current time on your emulator.

1) In your hooked method of Xposed for updateClock(), find a way to get the text of the current time

2) Create a string inside your hooked method of Xposed, which will be added to the text displayed for the current time of your emulator (choose a very short text)

3) Find a way to display the current time advanced by seven hours

Remember to reboot your emulator when you debug your Xposed module for reloading the Xposed configuration.
With Xposed you can also get and modify the parameters given to a hooked method.

# Exercise 4
Let's create an interface for your application.
Because you will save some data you have to do a quick setup before.

1) Add to your dependencies (build.gradle of your application)

    ```javascript
    implementation 'com.crossbowffs.remotepreferences:remotepreferences:0.7'
    ```
    Sync your project

2) Create a class with the same location as your XposedClass

    - Define your class to extends RemotePreferenceProvider

    - Add a constructor to your class and write this line into it replacing
    'com.myapp.example' by your package name
    ```javascript
    super("com.myapp.example", new String[] {"my_prefs"});
    ```
    "my_prefs" corresponds to your application's preferences (the saved data of your
    application)

3) Add to your AndroidManifest.xml (manifests folder) inside 'application'

    Replace:
        - 'com.myapp.example' by your package name
        - 'MyPreferenceProvider' by the name of your class
            which extends RemotePreferenceProvider (don't forget the '.' before)

    ```javascript
    <provider
        android:authorities="com.myapp.example.preferences"
        android:name=".MyPreferenceProvider"
        android:exported="true" />
    ```

You have done the setup for saving data of your application.
You use a ContentProvider that authorize access to your application's data with
all Android packages.

4) Add an empty activity to your package:
    - Click on File -> New -> Activity -> Empty Activity

5) Add a button to your layout

    - To find your activity's layout, click on res folder (not generated one),
    search your activity's name and click on it.

    - You can now design your application and add a button to it,
    don't forget to add layouts (setting position of your button), text and id

    - In your activity's class, find a way to assign a variable to your button

6) Set a ClickListener on your button

7) Repeat steps 5 and 6 creating another button

## Manage saved data inside your application

To get your data (preferences):
```javascript
SharedPreferences prefs = getSharedPreferences("pref_name", MODE_PRIVATE);
```
"pref_name" is the name of the file which save your data, you have to use the
same file name inside and outside your application to access to your saved data.

To get a String from your data (preferences):
```javascript
String test = prefs.getString("test_key", "default_value");
```
- "test_key" corresponds to the name of a variable inside your preferences file.
- "default_value" corresponds to the value if the variable isn't set.

To set a variable (String in this case) in your data (preferences):
```javascript
String value = "test";
prefs.edit().putString("var_name", value).apply();
```
"var_name" corresponds to the name of a variable inside your preferences file.

8) In the ClickListener of each button, set a different color value by saving this value in a variable in your preferences file

You can now change the data inside the application, but the others packages in your
emulator can't access to it, it's why we have setup a ContentProvider and you have to
edit your XposedClass.

## Retrieve saved data outside of your application

To get your data (preferences):
```javascript
SharedPreferences prefs = new RemotePreferences(AndroidAppHelper.currentApplication(), "com.myapp.example.preferences", "pref_name");
```
Replace "com.myapp.example" by your package name, "pref_name" by the name of your preference file

Getting and saving a variable in your preference file is done the same way as inside your application.

9) In your Xposed's hooked method in your XposedClass, set the text's color with the
value saved in your preferences file

# Exercise 5
Let's make our application prettier.

1) Add a TextView with the following parameters to your layout (.xml file)

    - The displayed text must be: "Clock Text Color"
    - The text must have a size of 20sp
    - The text must be displayed in bold
    - The text color must be set to black

2) Change the color of each button to match the color applied to clock text
    (Example: if your button changes the clock text to red, you need to set the button color to red)

You can see an image of a correct result named exercise5.png in the results folder.

# Exercise 6
Let's edit a text displayed by another app with a text that can be edited inside
your application.

1) Install the displayString.apk on your emulator (drag and drop it)

2) In your app, add a TextEdit with a Button in your Activity

    - When you press the button, the text inside TextEdit should be saved in
    your preferences (data)

3) Add a TextView with the following parameters to your layout

    - The displayed text must be: "Package Text"
    - The text must have a size of 20sp
    - The text must be displayed in bold
    - The text color must be set to black

4) Display a short message (called Toast) when your click on each button to 
say if the action was successfully completed

5) Hook the method that changes the text displayed in displayString.apk app

    displayString.apk information:
        packageName: "com.WsXposed.DisplayString",
        class: "MainActivity",
        method: "public void changeString(String txt)"
    
    You must use the text from your TextEdit to replace the text displayed in the displayString.apk app.

6) Compile your application and restart your emulator

If you are successful, when you start the displayString.apk application, you will get the text of your application preferences.

You can see an image of a correct result named exercise6.png in the results folder.

# Exercise 7
When you received a call on your phone, a method is called in the Android API to display the number of the incoming call.

Find a way to change the number of an incoming call.

The replaced number must be a variable in your application preferences, and can be changed in your application with a TextEdit coupled with a Button.

Because we never forget to display the title of a new feature,
Add a TextView with the following parameters to your layout

    - The displayed text must be: "Replacement of the Number"
    - The text must have a size of 20sp
    - The text must be displayed in bold
    - The text color must be set to black

You can see an image of a correct result named exercise7.png in the results folder.

# Bonus
If you are comfortable with Java and not with Kotlin, try to redo the exercises with Kotlin (also valid for the opposite case) ;)

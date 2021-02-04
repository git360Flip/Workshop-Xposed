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

    compileOnly 'de.robv.android.xposed:api:82'
    compileOnly 'de.robv.android.xposed:api:82:sources'

3) Edit your AndroidManifest.xml and add in your application:

    <meta-data
        android:name="xposedmodule"
        android:value="true" />
    <meta-data
        android:name="xposeddescription"
        android:value="An example which changes the color of clock text in status bar" />
    <meta-data
        android:name="xposedminversion"
        android:value="53" />

4) Create a class and remember its name and the name of your package. This class will contains the code to modify the methods of the Android API, let's name it for the tutorial: XposedClass

5) Create xposed_init file

    - Create new repository called "assets" in the same location as your "res" folder
    - Create a file (with no extension) named "xposed_init"
    - Write into it the name of your XposedClass (example: com.myapp.test.XposedClass)

6) Modify your XposedClass

    - Add theses imports:

    import de.robv.android.xposed.IXposedHookLoadPackage;
    import de.robv.android.xposed.callbacks.XC_LoadPackage;
    import de.robv.android.xposed.XposedBridge;

    - Implement IXposedHookLoadPackage for your XposedClass
    (search for keyword "implements" if you are new to Java)

    - Override the public method handleLoadPackage which takes a "LoadPackageParam"
    as parameter

    - Add this line inside your overriden method:

    XposedBridge.log("Loaded app: " + lpparam.packageName);

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
All the 'Loaded app:' correspond to the loading of each package of your system.
Each package initialisation is modified to log his name in addition of his initial initialisation.

# Exercise 2

1) Find the package which contains Clock.java with an updateClock() method

2) Inside your XposedClass, when you handle the loading of each package



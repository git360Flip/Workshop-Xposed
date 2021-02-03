# Workshop-Xposed
Workshop created by git360Flip for the Hub of Epitech

# Setup
- Download Android Studio: https://developer.android.com/studio#downloads

- Navigate to ./setup/ folder into a terminal:
	- chmod a+x genymotion-3.2.0-linux_x64.bin
	- ./genymotion-3.2.0-linux_x64.bin

This will install Genymotion into the setup directory, you can move it wherever you want
To launch the software navigate to the new folder "genymotion" and type into a terminal:
./genymotion

In genymotion software:
	- Create an account and login to Genymotion
	- Choose personal use
	- Accept the end user license agreement
	- Click the "+" button in right up corner
	- Click on "Android API" and tick the API 26 box
	- Click on "Custom Phone", set your preferences but keep NAT for the network mode
	- When the installation is done, double click on your new device for start it

/!\ If you have a GTX card in your computer, please run the software with it, otherwise
	your PC can freeze

- Navigate to ./setup/ folder into a terminal:
	- Drag and drop XposedInstaller_3.1.5.apk into your device
	- The application should start, click on the cloud under the Install/Update section
	- Click on Install (not Install via recovery)
	- Allow Xposed Installer to have Superuser access: select "Remember choice forever"
	- Click on Allow
	- Close your virtual device and start it again

The installation is done, congratulations :)

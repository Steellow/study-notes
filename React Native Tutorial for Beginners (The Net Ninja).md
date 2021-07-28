<-- [[README]]
🌍 https://www.youtube.com/playlist?list=PL4cUxeGkcC9ixPU-QkScoRBVxtPPzVjrQ
# React Native Tutorial for Beginners

## 1 - Introduction
- Nothing interesting

## 2 - Creating a React Native App
- You can use `Expo CLI` or `React Native CLI`
	- [Expo CLI](https://docs.expo.io/workflow/expo-cli/)
		- Easier to use
		- Provides utilities and such
	- React Native CLI
		- Requires more configuration
		- Apps made with this are considered to be pure React Native apps
			- Cuz they contain no unneeded utilities etc.
			- Only the very essentials
	- Will use Expo on this tutorial
		- You can change to RN CLI middle of project?
- Creating first project
	1. `npm install expo-cli --global`
	2. `expo init hello-world`
		- Doesn't work with Git Bash for some reason
		-  `blank` workflow 
	3. `cd hello-world`
	4. `npm start`
		- Actually uses `expo start` under the hood, both will work
- Expo Devtools will be opened in browser
	- Debug tools from Expo
	- There's a QR code which you can scan to access the app on your phone
		- You need [Expo](https://play.google.com/store/apps/details?id=host.exp.exponent&referrer=www) app on your phone
	- You can also use Android emulator
		- You need Android Studio
		- AVDM
			- You can access AVDM from 'Welcome to Android Studio' popup!
				- 'Configure' from bottom right corner
			- Create device if you don't have already
			- Press play button to launch the emulator
			- Click 'Run on Android device/emulator' from expo tools
				- Emulator needs to be open

## 3 - Views, Text & Styles
- Some built in RN components
	- `View`
		- Like a `div` in html
		- Used to wrap other elements
	- `Text`
	- `style` prop
		- Can be used on most components
		- Similiar to classes in html
		- Doesn't use css, but emulates is
		-  Created with `StyleSheet.create({ sheet })`
			- ![[Pasted image 20210727143756.png]]
- 
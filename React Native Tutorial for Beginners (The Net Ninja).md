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
	- `<View>`
		- Like a `div` in html
		- Used to wrap other elements
	- `<Text>`
	- `style` prop
		- Can be used on most components
		- Similiar to classes in html
		- Doesn't use css, but emulates is
		-  Created with `StyleSheet.create({ sheet })`
			- ![[Pasted image 20210727143756.png]]
	- Styles are not automatically inherited, like in CSS
		- One exception: nested `Text` widgets


## 4 - Using state
- Hooks can be used normally, just like in React
- You can insert variables inside `<Text>`, with curly braces, no need for backticks or dollars
	- `<Text>My name is {name}</Text>`
- `<Button>`
	- Is self-closing tag, unlike in CSS
		- `title` prop for text
	- Can't be given style prop!
		- You can wrap it with `<View>` with a style (like `styles.buttonContainer`)


## 5 - Text Inputs
- `<TextInput>`
	- Self-closing
	- No styles on default, unlike with Flutter
- Pretty basic stuff in this video


## 6 - Lists & ScrollView
- Can use `map` the same way as in React
	- Doesn't make it scrollable
	- Just wrap it with `<ScrollView>`!


## 7 - Flat List Component
- Can also make scrollable lists with `<FlatList>`
	- Just give the data in `data` prop
	- Render individual items with `renderItem` prop
		- Function has `item` and `index` as params
			- ![[Pasted image 20210730073449.png]]
	- Video says you don't need keys with `<FlatList>`, but that's false?
		- I guess he meant that if your data objects have key property, it can pick it automatically
			- If you don't have it, you can use `keyExtractor` prop! 🤩
	- Can also specify columns with `numColumns`
- Difference between this and `map`?
	- `<FlatList>` only renders those items which are visible
	- 
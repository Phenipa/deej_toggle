#Deej_toggle

##Introduction
This project is a straight fork of the original [deej](https://github.com/omriharel/deej/tree/b0e10f51048a11ee28937e4910bb6d44187c9a2d#hardware), with the added functionality of a button to swap between audo devices. To do this, I had to use external software called [SoundSwitch](https://github.com/Belphemur/SoundSwitch). This repository will likely NOT be maintained regularly, and as such, use at your own discretion. 

##Background
My wish when first finding out about this project was for there to be a way to toggle between headphones and speakers on the push of a button. Unfortunately, this was not in the works at the time, so I decided to make this myself. 

After checking in on the [deej discord](https://discord.gg/nf88NJu) and getting some feedback, I decided to use a software called SoundSwitch (thanks Ginjah_), which handles keyboard-shortcuts and swaps default sound device. All I had to do was make the arduino send a signal to the deej-software and have deej send the proper keyboard-shortcut. 

##Usage
1. Download [SoundSwitch](https://github.com/Belphemur/SoundSwitch) and select your prefered sound-devices, and set your desired hotkey. The default in deej is CTRL+ALT+F12.
2. Download or build my modified deej-binary, you will find an .exe under releases.
3. Upload the code under [arduino/deej-5-sliders-vanilla](./arduino/deej-5-sliders-vanilla), making sure to modify to suit your needs, just like with the original deej.
4. Set up your switch like this: ![](https://www.arduino.cc/en/uploads/Tutorial/button_sch.png) 
Make sure you use a 10K ohm resistor between the switch and ground, this is to make sure your switch doesn't "float", giving false inputs and making the button toggle wildly. What digital pin you choose to use is irrelevant, just make sure to edit the arduino-code to reflect your choice of pin, just like you would with the volume-sliders.
5. Enjoy! If I haven't forgotten anything, you should now be able to push that button to your hearts content!

##How it works
First I had to make some changes to the data-structure the arduino sends over the serial connection. The standard data-format is volume-data in the range of 0-1023 separated by the pipe-symbol (|), like the following: "XXXX|YYYY|ZZZZ". In order to make a seperation between volume-info and other data, I decided to append a semicolon, and behind this I could send other signals. The data sent over serial when the toggle is pressed, is like such: "XXXX|YYYY|ZZZZ;toggle". The word toggle is only sent once upon button-press (therefore the arduino usually sends data like this: "XXXX|YYYY|ZZZZ;"), making it so the deej software is always able to trust the signal, and can switch accordingly. When deej recieves the word toggle, it uses a go-library called "[keybd_event](https://github.com/micmonay/keybd_event)" to send the keyboard-shortcut "CTRL+ALT+F12". This can be changed, however as for now, you will have to edit the source code and build from scratch, check the original deej repo for instructions. To change the shortcut, check the serial.go-file. Once the shortcut is sent, the program "[SoundSwitch](https://github.com/Belphemur/SoundSwitch)" will interpret this as you wishing to cycle through audio-devices. Check out soundswitch's repo for instructions. And that's it! Now, when pressing a button attached to your arduino, the sound-device will (should) change.
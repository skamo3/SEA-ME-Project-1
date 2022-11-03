SEA:ME Pi Racer
-----------------------------------------------------------
# Table of Contents
- [Assemble Pi Racer](#assemble-pi-racer)  
- [Setup Raspberry Pi](#setup-raspberry-pi)  
  - [Write Image on Raspberry Pi](#write-image-on-raspberry-pi)
  - [Enable SSH](#enable-ssh)
  - [Find and Connect Raspberry Pi](#find--connect-raspberry-pi)
  - [Install and Set up Environment](#install--set-up-environment)
- [Create car application](#create-car-application)
- [Control car using by Web Controller](#control-using-by-web-controller)
- [Control car using by Gamepad](#control-using-by-gamepad)
- [Display OLED panel](#display-oled-pannel)
- [Tips](#tips)
- [Errors](#errors)

----------------------------------------

# Assemble Pi Racer
- Assemble the Pi-Racer following to the pdf
    - [Pi-Racer Wiki](https://www.waveshare.com/wiki/PiRacer_Pro_AI_Kit)
    - [Pi-Racer Pro AI Kit Tutorial pdf](https://www.waveshare.com/w/upload/a/a2/Piracer_pro_ai_kit-en2.pdf)  
> **Important Point** 
>> 1. When connect motor & servo, attention to the connection  
>> 2. Connect right direction the Camera

----------------
# [Set up Raspberry Pi](https://docs.donkeycar.com/guide/robot_sbc/setup_raspberry_pi/)

## Write Image on Raspberry Pi  

### Install Raspberry Pi imager for set up Raspberry Pi
- [Download Imager in link](https://www.raspberrypi.com/software/)
    - Follow OS specific guide
### [Download Image file](https://docs.donkeycar.com/guide/robot_sbc/setup_raspberry_pi/#step-1-flash-operating-system)
- Click this link  
    <img width="289" alt="image" src="https://user-images.githubusercontent.com/54701846/191256868-c60b590c-b9d1-4edd-84e4-cca7b37ef106.png">
###  Write downloaded Image on SD card
<img width="500" alt="image" src="https://user-images.githubusercontent.com/54701846/189227237-70456b2e-9adb-448b-a854-5b353e1934ef.png">  
  
- Operating System -> Downloaded image
- SD Card -> Your own SD card for Raspberry Pi

### Enable SSH & Setting WIFI
<img width="500" alt="image" src="https://user-images.githubusercontent.com/54701846/189228343-e1d0165b-3849-45f0-8a4c-28fc16b1b7ac.png">  

- Check **Enable SSH**
- Check **Set username and password**
    - Username is server's name
    - Password is server's password
    - **Remember this. It needs when connect Raspberry Pi server & PC**

![image](https://user-images.githubusercontent.com/54701846/189229057-2b93fa51-6bc7-49d2-bea5-63d155635273.png)  

- Check Configure wireless LAN
    - Enter the Wi-Fi or LAN information you are using on the PC you are connecting to

- Setting done. **Write the Image**

## Enable SSH
- Write finish and Move to **/boot** directory
- Enter the command ```touch ssh``` or ```touch /Volume/boot/ssh```
    - Second one only work in Linux or Mac

## Find & Connect Raspberry Pi

### 1st. Use monitor
1. Connect monitor with Raspberry Pi
2. Command ```ifconfig``` & check IP_ADDRESS
3. Command ```ssh -Y {SERVER_NAME}@{IP_ADDRESS}```

### 2nd. Use nmap
1. ```nmap -sn {IP_ADDRESS}.0/24 ```
    - ex) nmap -sn 192.168.2.0/24 -> Change last number to 0 or *
2. Find Raspberry Pi's IP_ADDRESS
3. Command ```ssh -Y {SERVER_NAME}@{IP_ADDRESS}```
> If there are many devices connected to the router, check the Raspberry Pie by turning it off and on

### 3rd. Make a wild guess
- Raspberry Pie IP is caught similar to a PC
> But I don't recommend it. It's not like a programmer.

## Install & Set up Environment
- [Follow Step.6 to Step.12](https://docs.donkeycar.com/guide/robot_sbc/setup_raspberry_pi/#step-6-update-and-upgrade)

--------------------

# [Create Car Application](https://docs.donkeycar.com/guide/create_application/)
## Create donkeycar from template
- Create a set of files to control your Donkey with this command
    - ```donkey createcar --path ~/mycar```
    - You can also change your path something else instead of **"mycar"**

## Configure I2C PCA9685
- It's only for Raspberry Pi
- ```sudo apt-get install -y i2c-tools``` -> install i2c-tools
- ```sudo i2cdetect -y 1``` -> check your car

    <img width="393" alt="image" src="https://user-images.githubusercontent.com/54701846/189234718-233d72a5-9cf3-419c-b821-ac962a1fbb91.png">  

    - If you can't see 40  
        1. On Pi, ensure I2C is enable in menu of ```sudo raspi-config```
            - It suggest reboot
        2. Check your hardwear
            - Maybe your cable or Something wrong

# Control with Web Controller & Gamepad

## [Control using by Web Controller](https://www.waveshare.com/wiki/DonkeyCar_for_Pi-WEB_Control)
- In terminal, follow commands
    ```
    pi@raspberrypi:~$ source ~/env/bin/activate
    (env) pi@raspberrypi:~$ cd mycar/
    (env) pi@raspberrypi:~/mycar$ python manage.py drive
    ```
- Open Chrome in host pc. Go to http://{RASPBERRY_PI_IP_ADDRESS}:8887
- [Reference Link](https://www.waveshare.com/wiki/DonkeyCar_for_Pi-WEB_Control)

## [Control using by Gamepad](https://www.waveshare.com/wiki/DonkeyCar_for_Pi-Teleoperation)
- Connect the USB adapter of Gamepad to Raspberry Pi
- In terminal, follow commands
    ```
    pi@raspberrypi:~$ source ~/env/bin/activate
    (env) pi@raspberrypi:~$ cd mycar/
    (env) pi@raspberrypi:~/mycar$ python manage.py drive --js
    ```
### Way to use Gamepad
1. Use ```--js``` option
    - Run donkeycar with this command  
    ```python manage.py drive --js```  
2. Modify myconfig.py file
    - Find ```USE_JOYSTICK_AS_DEFAULT```
    - Modify False to True  
     ```USE_JOYSTICK_AS_DEFAULT = True```

# Display OLED pannel
## Enable display in ```myconfig.py```
- Find line ```USE_SSD1306_128_32``` removing ```#```
- Find line ```SSD1306_RESOLUTION``` removing ```#```
- example  
    <img width="541" alt="image" src="https://user-images.githubusercontent.com/54701846/189624796-23e749a9-c88f-42a4-bb8a-4c20d9deee7a.png">

# Tips
## When you need to re-install Raspberry Pi
- It should be occur this message
![Image](https://user-images.githubusercontent.com/54701846/189970190-033abc35-dc00-44fb-99fc-f37fde48b53d.png)
- follow this code to reset ssh-key 
    - ```ssh-keygen -R {IP_ADDRESS}```
    - IP_ADDRESS => Raspberry Pi's IP address

# Errors
- Gamepad work different way

<img width="620" alt="image" src="https://user-images.githubusercontent.com/54701846/189236256-e21e3799-4dd4-4b26-a228-09f43155f815.png">  

- 7 & 8 is changed
- Left and Right -> Left is forward and Right is backward
- Front and Rear -> Front is right and Read is Left
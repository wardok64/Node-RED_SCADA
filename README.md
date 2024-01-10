# Node-RED SCADA (with Dashboard)

This project was developed to a start-up that designs and builds equipment for extracting natural 
ingredients from plants. The equipment consists of a variety of modules like: refrigerator, heater, 
pumps, solenoid valves.

The start-up was building a small system for a university and needed help with the way operators will 
interact with it. The unit is small and the budget was very limited, hence they was looking for 
open-source options.

## First requirements (user).

- Upon powering on system should load and open SCADA page.
- Basic authorization for a single user (e.g. password on a screen). This is just to
prevent unwanted access by random people.
o If easy we can add multiple users, or user creation.
- Main SCADA page to summarise all inputs and outputs and control the process
- Option to save a configuration for further use (e.g. dump all user inputs in a csv
file and load them up when needed.
- Log all sensors and save into a csv for an operator to take home and perform
some analysis.
- Number of read variables is around 50
- Number of write variables is around 50

![Screenshot from 2024-01-03 16-24-59](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/86bf880c-f021-436d-af1d-2a59379792ef)

### HMI functionality & Dashboard mockup.
- User login.
- Single operating panel.
- Additional service screen with ALL signals.
- Ability to log data.
- Ability to save input parameters.

![Screenshot from 2024-01-03 16-27-46](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/29a8fe64-fe31-4516-8776-3b1d2f232119)

## Requirement analysis:

The first thing to do was to understand the requirements and analyse them logically on paper and pencil, 
understanding what the client was referring to at each point.

The next thing was to describe the requirements, and create sub-requirements adding points that perhaps 
the client had not contemplated in the description of the requirements, with the necessary functions to 
fulfil the requirement, also with paper and pencil. Subsequently all the sub-requirements were taken as 
functions or modules to start developing them in pseudo-code and then in node-network.

![IMG_9183](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/b5769337-6e5c-4bf9-80b3-341edad63137)

NOTE: For the development of these modules I chose to simulate the input data (sensors) and controls, 
without modbus, as this could take time away from the hardware, and in this way I could move forward with
the modules and only at the end integrate the modbus layer.

### Test.

Hardware:

- Raspberry Pi 4B.
- Touch panel 7” screen.
- DC power 5V/3A.

Software:

- Node-RED service.
- Dashboard package.
- Digital Keyboard “onboard”.

### Basic diagram.

![diagram](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/868b2bb5-9190-4da8-9a08-f7ff4b8e717d)


### Some bugs during hardware testing.

1.- Pop-up window when entering USB memory in Raspberry Pi "Remobable medium is inserted".
Solution: disable the window from the system preferences, this allows that when a USB device is
connected, this window does not pop up.

Source: https://forums.raspberrypi.com/viewtopic.php?t=159739

2.- Top right box with message "Drive was removed without ejecting please use menu to eject before
removal" when removing USB after dismounting via dashboard.
Solution: I found this link, where a similar problem was presented, since it was a similar message 
but that corresponds to the power supply of the raspberry, one of the solutions to prevent this 
message is displayed was to remove the plugin that corresponds to the battery, in our case we had 
to find the plugin corresponding to the devices.

Source: https://forums.raspberrypi.com/viewtopic.php?t=286821
(Disable the "Battery monitor plugin for lxpanel")

So in another thread I found out how we can list the plugins we had installed on the raspberry with
a command:
```
dpkg -l | grep lxpanel
```
Source: https://raspberrypi.stackexchange.com/questions/133138/lxpanel-applets-cannot-be-added-to-the-status-bar

So I ran it on the raspberry, along with information from another thread, I found that the name of 
the plugin could be "lxplug-ejecter" so I started looking for a way to remove or disable this plugin
by some command or configuration file.

The command that I found was:
```
sudo apt remove lxplug-ejecter
```
The next thing was to test it and it worked correctly, the message is no longer displayed on the raspberry
screen.

## Node-RED Flows.

![Screenshot from 2024-01-03 16-56-09](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/49af5b67-7aee-4743-bc69-0a44770753ee)
![Screenshot from 2024-01-03 16-56-21](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/ca02ec1b-732d-4637-8001-dc5a8c35574c)
![Screenshot from 2024-01-03 16-56-31](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/c41908fb-6d84-47c6-9fd9-3cc2f14841b2)
![Screenshot from 2024-01-03 16-56-42](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/fa06ec2b-a86d-4ef5-9048-75f3fa0b8359)
![Screenshot from 2024-01-03 16-56-55](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/628a7ec0-6520-480a-8ab2-0c1c6c2419e1)
![Screenshot from 2024-01-03 16-57-08](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/98e4f3b1-354d-4fc0-a14b-0d5bba4fb7a6)
![Screenshot from 2024-01-03 16-57-21](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/9d4e688b-c87f-4578-ad1f-377dc2c88507)
![Screenshot from 2024-01-03 16-57-33](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/0933ef08-671e-4bb5-be8b-6322cc6a0128)
![Screenshot from 2024-01-03 16-57-48](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/3b73ac00-1745-4966-a2ec-487f93fa03f7)
![Screenshot from 2024-01-03 16-58-09](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/e3b22ef2-2f7d-4b5f-8a2e-8cb8d62d3063)

## Dashboard (SCADA).

### Login screen:
![1_](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/0ff7ea5a-ee94-4e22-ac0f-cde1b531db05)

### Scada (Admin):
![2_](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/426de352-83dd-460f-a581-a8b6c13a919c)

### Scada (Change color):
![4_](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/2b83170c-c2cb-43cf-99c5-5ff070dd5735)

### Scada (menu):
![3_](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/1adeefa2-ff20-4dfa-b984-34941f6e13d7)

### Scada (Read only / lock):
![5_](https://github.com/wardok64/Node-RED_SCADA/assets/104173190/bf64fb62-449b-4a5b-a42d-d3eb80851c9e)


## Video DEMO (Runing on FlowFuse).

### Authentication.

https://github.com/wardok64/Node-RED_SCADA/assets/104173190/ea19211b-b17e-47e7-b810-468f816da0e4

### Controls and PID.

https://github.com/wardok64/Node-RED_SCADA/assets/104173190/792e173d-00c1-4b1c-a0f0-385163afc4ff

### Log and lock.

https://github.com/wardok64/Node-RED_SCADA/assets/104173190/f710e28a-08d0-432a-84a6-e588be0887d8

### Unlock and color.

https://github.com/wardok64/Node-RED_SCADA/assets/104173190/9f1f107b-b017-4e89-ab20-791d82ffc0cc

### Change user/password.

https://github.com/wardok64/Node-RED_SCADA/assets/104173190/fa184d0a-0414-406c-9139-baaf6f8284a6














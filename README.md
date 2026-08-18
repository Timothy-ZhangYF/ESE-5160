# ESE 5160 IOT Final Project

    * Team Number: 9
    * Team Name: Shorts & Sparks
    * Team Members: James Steeman and Timothy Zhang

## 1. Video Presentation

[Video in Google Drive](https://drive.google.com/file/d/10uaTPHjv0_yxICQpvfujqLQYtL-Gkd34/view?usp=sharing)

## 2. Project Summary

### Preface:  

Our semester long project was quite literally scrapped and redone due to unreliable PCB manufacturer (entire class suffered this, but ours was arguably the most severe). Everything you are about to read is quite literally put together in a bit over 3 days, complete from the ideation phase. While it is unfortunate we could not get a fully functioning and polished product, we still wanted to share our journey as it was a valuable learning opportunity (and honestly, we are quite proud of our adaptability and accomplishments over 70 hours). So buckle up as things might be a little chaotic, for this was written with our last bit of willpower! 

### Device Description:  

Brief Project Description.

- Our final device is a boat active stabilization device (Seakeeper) to combat the rocking (roll) motion of a boat at sea due to waves, improving rider comfort. The system uses an IMU to detect boat tilting and uses the gyroscopic precession effect of a spinning flywheel to counteract the motion.

- For most of the semester, our project was a 3 axis closed loop CNC machine for milling prototype PCBs. Due to board manufacturing issues outside of our control, we worked with the Teaching Team to do a very late project change to salvage our PCB in a demo-able project. Read here for more information on board issues: [Board Issues Google Document](https://docs.google.com/document/d/17aVozhZWAjP6q5agUZiOJsjJb8daSV915WeSpyRnOJc/edit?usp=sharing)

- The inspiration for our project was less of an "inspiration", more of a "maximize what we had". PCB manufacturing issues made our original project impossible due to some critical component failures. We took everything that we found functional on our PCB, and asked ourselves what is the coolest project we could make. Specifically, the most functional of our three boards had a working  SAMW25, MOSFET + gate drive IC for a high-power DC motor, stepper driver, all of the power regulation (24V, 5V, 3.3V) and a QWIIC connector for an external I2C. From here, we came up with the boat stabilizer idea (control moment gyro/CMG) as it perfectly maximizes our functional hardware. Our original CNC spindle motor was repurposed to spin a flywheel, the stepper motor could be used as the gimbal actuator (later decided to use a simple PWM servo), and the QWIIC connector allows us to connect an external IMU to measure a the tilt so that the CMG could actively compensate and counteract the rocking motion.


### Device Functionality

The entire system revolves(get it? haha) around the flywheel. Using the principle of gyroscopic precession, this allows us to tilt the flywheel back and forth (pitch), and produce a rolling tourque to counteract waves rocking a boat. Our large DC motor (original CNC spindle), will spin up a laser cut disk with slots for 16 large bolts along the circomference (DIY flywheel with max rotational inertia). This inertia combined with a frankly terrifying RPM (~5000 with 25% full power) gives us a large amount of stored angular momentum to use and counteract the boat rocking side to side. A servo motor is used to act as the gimbal actuator to tilt the flywheel assembly back and forth as it was mechanically the simplest. An IMU is used which allows us to measure current tilt and correct for it inside of a PID loop (originally also planned for a kalman filter with sensor fusion but once again time was extremely limited). All of the processing is handled by the SAMW25 which runs Free RTOS to simultaneously perform active stability compensation and other tasks such as CLI, and WIFI communication with Node-Red Dashboard.

- Sensor: IMU
- Actuator: DC motor, Servo
- Processing: SAMW25

![alt text](images/5160_Block_diagram.drawio.png)

### Challenges

Where did you face difficulties? This could be in firmware, hardware, software, integration, etc.

- Our major challenge was hardware (and as a result a lack of time), due to unforseen factors from the manufactures that caused several unfixable PCB issues (possibly due to wrong reflow temperature that killed the ICs). This quite literally killed our original project, leaving us only 3 days to try to implement and demo a new project from scratch. Again, this document provides specific information on board issues: [Board Issues Google Document](https://docs.google.com/document/d/17aVozhZWAjP6q5agUZiOJsjJb8daSV915WeSpyRnOJc/edit?usp=sharing)

- The new project suffered from lack of development/debugging time, with potential further hardware issues that we did not have time to pinpoint (unable to move our gimbal actuator as we could not get a second PWM instance on a different tcc unit to output from other pins. Tried 8 total pins, only the one used for the flywheel motor worked). This meant that we were unable to demonstrate the full integrated active roll compensation (which was the main selling point).

How did you overcome these challenges?

- These fundamental challenges were in a way "impossible" to overcome, but we were able to make the most out of what was possible. Over the course of 3 days into the demo and a few hours of refinement post-demo day (Node-RED otafu and additional servo troubleshooting), we were able to: 
   1. Successfully completed a new mechanical CAD and finished the physical construction which had all the necessary features of a fully functioning CMG.
   2. Rebuilt our Node-RED rebuilt dashboard interface, integrated flywheel power setting, logging IMU data (boat tilt), manual/automatic activation of the system, OTAFU, boat capsize detection that will alert the dashboard and text a phone number.
   3. Wrote drivers for our new I2C device (IMU), and successfully read out the data of interest and send over to dashboard for data visualization + logging.
   4. Wrote a motor dc control driver that will ramp up/dpwn the speed to any user set speed from the dashboard.

- Due to potentially even more hardware issue, we were unable to generate a second PWM for the gimbal actuator (servo), which unfortunately also limited our ability to do a full integration of everything. Some of the attempted pins did generate pwm using the dev board, but given the end of semester and final exams, we did not have time to completely isolate and troubleshoot this issue to a resolution.

### Prototype Learnings

What lessons did you learn by building and testing this prototype?

- We learned a lot about the manufacturing and developing process and considerations to make in the planning and execution stages of prototyping.
    - Design your intial PCB prototype to be easily reworkable to make up for both design and manufacturing mistakes
    - Understand your manufacturer capabilities by developing subsection or component test boards (this can also help you get a sense of layout and routing regarding said component). This can help make sure the manufacturing process, such as reflow temp, is successful with the IC before integrating into the larger system
- How to isolate/assess the where issues come from between design, firmware, and chip / manufacturing issues
- Gain famililiarity on dev board to get code / mcu development familiarity before doing pin assigment on PCBA
- Many more!

### Next Steps & Takeaways

What steps are needed to finish or improve this project?

- For the Gyro, next steps would to be find and resolve the TCC PWM issue on all other pins and use a working pin for the servo motor. Then develop a driver for the servo to ramp speed to provide torque to boat
    - Implement kalman filter for sensor fusion
    - Implement PID for tilt compensation (moving servo)


### Project Links

[Node Red UI Instance](http://172.190.141.169:1880/ui/#!/0?socketid=IxHLWtM_3o6IbezjAABr)

[Altium 365 Link](https://upenn-eselabs.365.altium.com/designs/DDC6BC9F-ABAE-498F-8839-63F1F02EF066)

## 3. Hardware & Software Requirements

Note: Since we had a late project change, we have created new HRS and SRS for the new project (post-facto) based on our goals when this project was created. We will attempt to evaluate our final output relative to these goals that reflect the ideal new project expectations. We recognize that some of these requirements are very basic and could be developed much further to frame a project, but since they were made post facto and again very limited time (we focused more on actual implementation for as long as possible) we hope this is sufficient to have some picutre of the intentions. The review is also more brief, in accordance with the nature of the requirements.

### HRS

| Req ID | Requirement | Review |
| ------ | ----------- | ------ |
| HRS-01 | We shall repurpose the PCBA from the CNC project using as much hardware as possible | Success: we used the PCBA. Used everything besides SAMD21 and stepper motor driver (timeline constraint) |
| HRS-02 | We shall reporpose as much of mechanical hardware as possible | Success: Used the Spindle motor as flywheel motor, bearing blocks, linear rods, aluminum extrusion, ... See pictures/video to see structure |
| HRS-03 | The DC flywheel shall be driven with a mosfet driver to allow for PWM speed control | Success: See video demos for control |
| HRS-04 | The system shall have external non-voilatile memory (microSD) of no less than 512MB for storing G-code and current progress (in any pause scenario) | N/A |
| HRS-05 | An IMU shall sample acceleartion and gyro in necessary axes (gz, ax, ay) at at least 200 Hz (for Kalman filter input) | Success: The refresh rate of IMU is 417 Hz, and we sample in an RTOS task running at 200 Hz ?Testing video? |
| HRS-06 | The system shall use a stepper motor to change the angle of the flywheel and provide a torque to the boat | Fail: We switched to a servo motor using PWM from SAMW (unsuccessful also), as simpler than SAMD21 interfacing to use the Stepper driver (connected to SAMD21 on pcba) |

### SRS

| Req ID | Requirement | Review |
| ------ | ----------- | ------ |
| SRS-01 | System shall use IMU data as input to Kalman filter of optimally 200Hz and at least 100Hz | Fail: timeline and implementation issues prevented this from being persued and implemented |
| SRS-02 | A pid control loop shall execute at optimally 100Hz and at least 50Hz | Fail: servo was not functional, so PID was not necessary or implemented |
| SRS-03 | The MCU in the SAMW25 module shall run an RTOS | Success: See codebase, all videos run applicaiton code in FreeRTOS |
| SRS-04 | The user shall be able to remotely set flywheel speed | Success: See demo video |
| SRS-05 | The system shall be able to send IMU data to the web portal for remote user access | Success: See demo video |
| SRS-06 | The SAMW25 and SAMD21 mcus on the board should communicate over UART to transimit PWM or data for distributed processing as possible or best feasable as found in development | Fail: UART developed on both indpendantly successfully (on pins already connected on the PCBA). Logic analyzer captures were used to assess this on SAMD21 and moving the CLI uart to different pins on SAMW25. Creating a separate simultaneous (with cli) uart implementation on the samw led to integration issues, and this was dropped in favor of a simpler, time sensitive approach |
| SRS-07 | The system shall generate 10kHz PWM at various duty cycles in a ramping manner for safe DC motor control (flywheel) | Success: see codebase DcMotor.c and Demo video for example |
| SRS-08 | User shall be able to remotely activate and disable tilt compensation | Partial Success: Node red/wifi setting successful, but servo/feedback actuation unsucessful |
| SRS-09 | OTAFU shall be able to be initiated remotely through web portal (node-red) | Success: See demo video |
| SRS-10 | An OTAFU shall be implemented | Success: See demo video |


## 4. Project Photos & Screenshots

### Final Project Assembly

![alt text](images/IMG_5749.JPG)

### Standalone PCBA, top

![alt text](images/pcba_top.png)

### Standalone PCBA, bottom

![alt text](images/pcba_bottom.png)

### Thermal Image of PCBA under laod

![alt text](images/pcba_load_thermal.png)

### Altium Board design 2D view

![alt text](images/2d_altium.png)

### Altium Board design 3D view

![alt text](images/3d_altium.png)

### Node-RED dashboard

![alt text](images/node_red_dashboard.png)

### Node-RED backend

![alt text](images/node_red_backend.png)

### Block diagram of your system

![alt text](images/5160_Block_diagram.drawio.png)

#### A00G initial proposal system diagram for original CNC mill project

![alt text](images/BackgroundV2_Detailed.drawio.png)

This is the early diagram that remained mostly ture for throughout the PCB implementation and until the weekend before demo day, when enough critial issues with the PCBA manufacturing was found and isolate to issues outside our control and reworking capability that wee moved to the boat stabilization project.

## Codebase

### Firmware

[Bootloader Folder in course repo](https://github.com/ese5160/final-project-t09-shorts-sparks/tree/main/Bootloader)

[Application Folder in course repo](https://github.com/ese5160/final-project-t09-shorts-sparks/tree/main/Application)


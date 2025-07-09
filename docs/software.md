# Software

## Robot Control
Teams have to write their own software framework that is capable of receiving data from ssl-vision and ssl-game-controller and to communicate with their robots. The communication to the robots is not yet standardized. Most SSL teams use either low-level radio modules that work in the 2.4 GHz range or use 5 GHz Wifi.

More information on our software can be found [here](https://nrwiki.imbit.intra.uni-freiburg.de/research/robots/RoboCup_Small_Size_League_Robots/Robot_Software).

## Official SSL Software 
`SSL vision`
: This is the official, league-maintained software for robot and ball detection. It is responsible for transforming images from all cameras over the field to coordinates that are broadcast to the teams via multicast. During a competition, it is set up by the technical committee. Teams do not have to take care of it. However, the detections are not reliable and require further processing. Also, for division A there are two cameras. Detections can be on both cameras and need to be merged accordingly. 

- [Github Repo](https://github.com/RoboCup-SSL/ssl-vision)
- [Paper](https://link.springer.com/chapter/10.1007/978-3-642-11876-0_37)

`Game Controller`
: A match is coordinated by the SSL-Game-Controller. It is running on a computer next to each field and has a UI for the game controller operator. The game controller broadcasts referee messages via multicast, just like SSL-Vision. Additionally, there are synchronous network interfaces for autoRefs and teams to communicate bidirectionally.

- [Github Repo](https://github.com/RoboCup-SSL/ssl-game-controller)

`Autoref`
: There are automatic referees that supervise a match and support the SSL-Game-Controller, mainly with detecting fouls. In order to increase robustness and fairness of the foul detections, multiple AutoRefs can (and will) run simultaneously and decisions can be based on a majority vote.
There are two implementations that are currently under active development by teams and that are used since RoboCup 2018: 

- [ER-Force](https://github.com/robotics-erlangen/autoref)
- [TIGERs Mannheim](https://github.com/TIGERs-Mannheim/AutoReferee)

`Simulators`
: The [Simulation Protocol](https://github.com/RoboCup-SSL/ssl-simulation-protocol/) defines a common way to communicate with an SSL simulator. It has been initially introduced for the virtual tournament at RoboCup 2021. The common protocol should encourage teams to publish their simulator or to use other open sourced simulators. By having a common protocol, switching between different implementations gets easier.
The protocol allows simulators to add some custom definitions, like additional realism configurations.
There are currently two open-sourced simulators available: 

- grSim ([Github Repo](https://github.com/RoboCup-SSL/grSim), [Paper](https://link.springer.com/chapter/10.1007/978-3-642-32060-6_38))
- [ER-Force simulator](https://github.com/robotics-erlangen/framework#simulator-cli)


`SSL Status Board`
: The [SSL-Status-Board](https://github.com/RoboCup-SSL/ssl-status-board-client) can be run on a large screen to present the current state of the game to the audience and the human referee.

The information on this page and more can be found [here](https://ssl.robocup.org/technical-overview-of-the-small-size-league/).

## SSL Vision
---
### Opening the SSL Vision Software, Graphical Client and Terminal Client  
First locate and change to the ssl-vision directory with ```cd ssl-vision```.

**SSL vision software**: This is used for calibration, changing settings, etc. Type ```./bin/vision -s``` to start the software with image capturing, ```./bin/vision``` to start the software without image capturing.

**Graphical client**: This shows you the detected robots and balls in a graphical interface. Type ```./bin/graphicalClient```.

Image

**Terminal client**: This displays the returned json output for every frame which includes information about the current frame, detected balls, detected robots, etc. Type ```./bin/client```. 

### Camera Calibration
**0. Check visualization options**

In the visualisation options ("Thread 0/ Image Capture/ Visualization") check the "camera calibration" and "camera calibration result" options.

Image

**1. Reset**

Select the camera calibration tab on the right hand side and click the "Reset" button. 

**2. Update control points**

It is important that the number of line segments and the number of arcs are correct before updating the calibration points. Click the "Update calibration points" button. The four calibration points change their displayed coordinates accordingly. Move the calibration points so they are placed according to their displayed coordinates. To move the calibration points simply drag and drop them in the camera image. For our setup with five line segments and no arc the four control points are the four corners of the field. The control point with the coordinate (-half of the field length, - half of the field width) goes in the left upper corner of the field. Not that the field dimensions are measured from the outside of the field lines. Meaning that the control points should be on the outer corners of the field lines. Zoom in and adjust the control points in such a way that the corner of the field line is in the center of the control point square. Correct placement is cruial when you want the SSL vision output coordinates for robot and ball detection to match the actual coordinates on the field!

**3. Initial calibration**

Once the calibration points are placed click the "Do initial calibration" button. The red field lines should align with the field. 

**4. Improve initial calibration**

Next drag the camera height (in our case this is 2690mm) and the distortion slider until the calibration results (red lines) approximate the actual field lines. Click on "Do initial calibration" to update the calibration. Repeat until the calibration looks good. 
If the distortion slider distorts the image in the wrong way change the principal point of the camera ("Thread 0/Camera Calibrator/Camera Parameters") the principal point x and principal point y should 
be the pixel coordinates of the camera. So probably in the middle of field. However, this should in theory not be necessary as the calibration sets the principal points automatically. 

**5. Additional calibration points**

In addition to the four corner points SSL vision also uses points on the field lines for calibration. It detect the field lines automatically using basic edge detection. In the list of visualisation 
options check the "Detected edges" option. Now use the "Line search corridor width" slider so the the field lines are within the shown search corridor. Then click on "Detect additional points". 

**6. Full calibration**

There should now be a bunch of points marked along the field lines. Now click on the "Do full calibration" button. 

**7. Refine**

If necessary the last two steps can be repeated with a smaller corridor the improve the calibration results. 

#### Colour Segmentation
The robot and ball detection is done by the SSL vision software. It does so by colour thresholding. For that we need to populate a LUT (look up table). Go to the "Auto Color Calibration" tab on the right hand side. 

Image

Select a colour from the list on the right hand side (for example green) and click on sample points in the camera view where this colour can be seen. The field green does not need to be selected, also cyan is not used. Do this for all colours. It may be helpful the more the robots to different positions on the field for the colour sampling as there might be different lighting conditions.

You need to click "Update LUT" to update the thresholded image. The thresholded image is shown, when you turn on the "Thresholding" option from the visualisation options list. That can help to refine the LUT. When the thresholding is not good yet select more sample points. 

Furthermore, there is the "blob" option in the visualisation list. This shows you the detected blobs (e.g. a marker, the ball, etc.) as bounding boxed around the blobs.  

#### Image Capture Settings
Camera settings can only be changes when the camera is not currently capturing! So either click "stop" or open the SSL vision GUI with ```./bin/vision``` without the "-s". The camera settings can be found at "Thread 0/Image Capture/ Video 4 Linux/ Capture Settings".

#### SSL Vision Output JSON
Example output from the SSL vision client for one frame:
```json 
{"frame_number":707,"t_capture":10.783568999999993,"t_sent":10.783568999999993,"camera_id":3,"balls":[{"confidence":0.95492375,"x":0,"y":0,"pixel_x":0,"pixel_y":0}],"robots_blue":[{"confidence":1,"robot_id":0,"x":-1499.9639,"y":1120,"orientation":0,"pixel_x":-1499.9639,"pixel_y":1120},{"confidence":1,"robot_id":1,"x":-1499.9639,"y":1.9834129e-11,"orientation":-0,"pixel_x":-1499.9639,"pixel_y":1.9834129e-11},{"confidence":1,"robot_id":3,"x":-549.9639,"y":4.5318516e-11,"orientation":-0,"pixel_x":-549.9639,"pixel_y":4.5318516e-11},{"confidence":1,"robot_id":4,"x":-2499.9639,"y":8.739209e-11,"orientation":-0,"pixel_x":-2499.9639,"pixel_y":8.739209e-11},{"confidence":1,"robot_id":5,"x":-3599.9639,"y":4.5971272e-11,"orientation":-0,"pixel_x":-3599.9639,"pixel_y":4.5971272e-11}]}
{"frame_number":708,"t_capture":10.800235999999993,"t_sent":10.800235999999993,"camera_id":0,"balls":[{"confidence":0.952645,"x":0,"y":0,"pixel_x":0,"pixel_y":0}],"robots_yellow":[{"confidence":1,"robot_id":0,"x":1497.5712,"y":1120,"orientation":3.1415927,"pixel_x":1497.5712,"pixel_y":1120},{"confidence":1,"robot_id":1,"x":1497.5712,"y":-1.2529437e-12,"orientation":3.1415927,"pixel_x":1497.5712,"pixel_y":-1.2529437e-12},{"confidence":1,"robot_id":3,"x":547.5712,"y":9.41912e-12,"orientation":-3.1415927,"pixel_x":547.5712,"pixel_y":9.41912e-12},{"confidence":1,"robot_id":4,"x":2497.5713,"y":6.193469e-12,"orientation":-3.1415927,"pixel_x":2497.5713,"pixel_y":6.193469e-12},{"confidence":1,"robot_id":5,"x":359
```

`"balls"`: Provides information about the detected balls. For each detected ball the confidence, the x and y coordinates in the field coordinate system and the pixel position are given.

`"robots_blue"`: Provides information about the detected robots playing in the blue team. For each detected robot the confidence, the robot id (based on pattern), the x and y coordinates in the field coordinate system, the orientation and the pixel position are given. 

`"robots_yellow"`: Provides information about the detected robots playing in the yellow team.


### Game Controller
---


### Autorefs
---



### Simulators
---

### SSL Status Board
---


### Standard Network Parameters
---
| Protocol                                | Protobuf                 | Type            | Address         | Port  |
|-----------------------------------------|--------------------------|----------------|----------------|-------|
| SSL-Game-Controller (GC)                | Referee                  | UDP Multicast  | 224.5.23.1     | 10003 |
| SSL-Vision Detections                   | SSL_WrapperPacket        | UDP Multicast  | 224.5.23.2     | 10006 |
| SSL-Vision Detections Legacy            | SSL_WrapperPacket        | UDP Multicast  | 224.5.23.2     | 10005 |
| AutoRef -> GC                           | AutoRef                  | TCP            | GC             | 10007 |
| AutoRef -> GC                           | AutoRef                  | TCP + SSL      | GC             | 10107 |
| Team -> GC                              | Team                     | TCP            | GC             | 10008 |
| Team -> GC                              | Team                     | TCP + SSL      | GC             | 10108 |
| Remote Control -> GC                    | Remote Control           | TCP            | GC             | 10011 |
| Remote Control -> GC                    | Remote Control           | TCP + SSL      | GC             | 10111 |
| Continuous Integration -> GC            | Continuous Integration   | TCP            | GC             | 10009 |
| Continuous Integration -> AutoRef       | AutoRefCi                | TCP            | AutoRef        | 10013 |
| SSL-Vision-Tracker                      | TrackerWrapperPacket     | UDP Multicast  | 224.5.23.2     | 10010 |
| SSL-Vision-Client Visualization         | Visualization            | UDP Multicast  | 224.5.23.2     | 10012 |
| Simulation Control                      | SimulationControl        | UDP            | Simulator      | 10300 |
| Robot Control Blue                      | RobotControl             | UDP            | Simulator      | 10301 |
| Robot Control Yellow                    | RobotControl             | UDP            | Simulator      | 10302 |
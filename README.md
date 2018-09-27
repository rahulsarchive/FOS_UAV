# FOS_UAV

The aim of this project is to develop an open source aerial mapping platform using standard hardware and to develop a workflow from data acquisition to data processing. 

![open uav](Images/ICFOSS.png)

FOS UAV is part of the Drone research program by [ICFOSS](https://icfoss.in/) (International Center for Free and Open Source Software)

Most UAV's today are multirotors, they have the advantage of being able to hover and carry more payload like sensors and other auxilaries. But with the current battery technology, multirotors have a flight time in the range of 15- 20 mins, without compromising on the payload. They make good inspection drones in tight spots and hard to reach places. But because of the way they fly, they brun through a lot of  power, hence limiting the flight time and maximum range.

Fixed winged aircrafts fly on the principle of aerodynamic lift and are far more efficient at covering ground than multirotors. Even though they are considerably harder to design and build, they make up for with efficiency.

Our goal is to build a suitable fixed wing platform that can carry the necessary payload and provide usable data for arieal mapping. The craft needs to carry a good GPS system, a downward facing camera for taking images, and be able to fly missions autonomysly.

Our constrains being, it must be simple to make, and easy to use. All the hardware and software are low cost and open-source, without sacrificing saftey and usability.

## Related projects
 [**4-Axis Hotwire Foam Cutter**](https://github.com/rahulsarchive/4AxisFoamCutter)  
 [**CNC Drag knife**](https://github.com/rahulsarchive/cncdragknife)  

# Mapping Process
![open uav](Images/Software/ortho.jpg)

The mapping is done by a process called **Photogrametry.** The UAV carries a camera which is angled down to take photos of the ground when the crafts flies overhead. The UAV then flies along a defined GPS way-point mission which covers the area to be apped with sufficient overlap. A software then reads all the images and then stiches them into an 3 dimensional orthomosaic.

Multiple data can be infered from the orthomosaic like, ground elevation, water level, cubic volume of materials. With suffient overlap and good camera altitudes, centimeter level accuracy can be obtained. But inorder for the process to work, the UAV must provide reasonably stable data over the entire course of the mission.

# Software

## Ardupilot
![open uav](Images/Software/ardu.jpg)

Ardupilot is one of the most advanced open source autopilot solution, it has been there the longest and has an active community developing it. The firmware support a wide range of vechiles from airborne to terrestrial and even underwater. It is paired with a suitable ground control software to bring out is full functionalities.

In the latest release of Ardupilot they moved onto the Chibios platform and have now extended support for some of the common STM32 F4 flight controllers like the Omnibus F4 and the Matek F405 wing. We will be using this version and testing the features, the new firmware is still in beta and being actively developed.

## INAV
![open uav](Images/Software/inav.png)

Inav is a flight control software, which was forked from Clean Flight. The software supports both multi-rotors and fixed wings and has good GPS modes like Return to launch (RTH) etc. The software has a mission planner, which allows for setting full autopilot GPS way-point navigation. It supports a variety of Flight controller boards and is actively being developed by the community.


## Open drone maps

![open uav](Images/Software/odm.png)

OpenDroneMap is an open source toolkit for processing aerial drone imagery. Open drone maps turns the images captures by the drone into three dimensional geographic data that can be used in combination with other geographic datasets.

it can process a collection of images into point clouds, Digital surface models, Digital Elevation Models etc.

## Mission planner

![open uav](Images/Software/mp.jpg)

Mission Planner is a ground control station for Plane, Copter and Rover. It can be used as a configuration utility or as a dynamic control supplement for your autonomous vehicle. It can be used to configure and tune your vehicle, plan and load autonomus gps way-point missions. With proper telemetry hardware, it can provide the live status of the vehicle, record telemetry data and operate the vehicle in FPV (first person view)

# Hardware

## Design consideration

The flight characteristics need to be kept in mind when choosing an aircraft. We wanted to keep thing simple, and minimze the number of moving components. Hence no tail. The aircraft will be a flying wing,simple and monolithic in construction, good performance and agility.

![open uav](Images/Software/wing.jpg)

### Wing Aerodynamics 101

The two most important parameters when desiging an aircraft are the **Center of Gravity (COG)** and the **Center of Pressure (COP)**. The COG is the point where the weight of the aircraft acts and the COP is the point where the aerodynamic lift forces act. For the plane to be  stable, the COG must be in front of the COP. But this means the airplane has a tendency to nose down when flying. Thats where the tail comes in, the tail on a conventional airplane is set at a slightly downward angle compared to the wing, a term known as Decolage.

So the wing is lifting up and the tail is pushing down to keep the plane stable. If the COG is too far forward the wing has to push down even more and the wing has to make more lift to counter this effect. The inverse is also true, if the COG is too far back then the tail has to push up to keep the plane level, this is not a stable condition and its very difficult to fly a tail-heavy plane.

![open uav](Images/Software/ReflexWing.gif)

A wing emulates a stable flying conditon by having a forward COG and using its Elevons (Elevator+ Aeilerons) to pitch the wing up, A term known as **Reflex** in which the elevons are kept at a slight up to push the tail down and pitch up the wing. This is not without consequences, the more forward the COG the more the elevons have to pitch up, there by creating more induced drag. The next parameter which influences the COG and hence the stability is the **Sweep Angle**.

![open uav](Images/Software/sweep.gif)

 Sweep is importart because it provides leverage for the COG to act. The COG needs to be at about 25-30% of the chord, in a rectangular wing, this is very close to the leading edge and will not provide enough leverage to balance the COG, so we need a long fuselage in front of the wing to carry the weight.

In a wing, the CG needs to be at about 20-30% of the **Mean aerodynamic chord**, whcih takes into account the root and tip chord and the sweep angle. In simple terms, the more the sweep, the farther back the COG can be and the more leverage you can get. But the more the sweep the less the lift is produces per wingspan. Sweep also has another effect, it acts like a **Dihedral**

A **Dihedaral** is a slight angle between the wings to provide stability. When one wing dips, it produces more lift than the other wing and the aircraft comes back to level. A side effect being, it will cause the wing to wobble, as the side facing the wind will have more lift than the trailing wing.


Flying wings are a lot harder to control as they are inherently unstable aircrafts. They are very sensitive to the changes in Center of Gravity (CG) and flying them manually is a skill.

Since our objective is to get stable data, flying them manually is not an option. This is where a flight controller comes in, it will keep the craft stable and provide additional features.

### Power plant selection
	
	motor specification+ prop+ battery 

### Flight controller

A Flight controller is basically an inertial measurement unit (IMU) plus a couple of sensors and a processor to read the sensors and act on the motors. It si constantly monitoring the orientation of the craft and trying to keep it level against outside disturbances. Most flight controller can run software which provides a host of abilities like GPS navigation, Auto-land/take-off etc.

There are a many open-source flight controllers designed for fixed wing like the **APM** and **Pixhawk**. But we can use the mini race quad boom to get cheap and efficient flight controllers.

Most mini quads today use a flightcontroller which has an STM32 chip on it, depending on the performance required, they range from F1 to F7 series. we settled on an STM32F4 flight controller which is a happy medium between performance and the number of devices that can be connected to it.

The flight controller needs software to run. There are couple of opensource softwares to choose from like Betaflight, Cleanflight, INAV etc. Most softwares are geared towards multirotors but there are a few which support fixed wing crafts. We chose INAV as the flight control software as it met all the requirements and has some of the best GPS modes available.




## Hardware 

*Sheet  
list*


## Version 0.5 (Trainer)

Wingspan: 600mm (Polyhedral)  
Sweep: Nil  
AUW: 800g  
Power plant: 2212 1000kv motor (10x4.7 prop)  

![open uav](Images/V0.5/t1.jpg)

![open uav](Images/V0.5/t2.jpg)


We decided to make a trainer aircraft as our first plane. This will help us understand all concepts of aerodynamics and and give us a first hand experience in contruction techniques, control mechanisms, and learning to fly an aircraft.

We built a model based on the fish from flight test.

The model is a 3-channel aircraft made from 3mm coroplast sheets and joined together by hot-glue and wodden spars. It was not easy to work with coroplast, it has flutes running parallel to the lenght, cuttting it inot shapes is no easy task. We spend some time cutting the sheets by hand and joining them together with hot glue.

The model has an all up weight of 800 grams and flies with a 1000kv motor running a 10X4.7 prop, producing aproximately 950g of thrust. The plane takes off at 60-70% throttle and cruises at 60%.

It is a dream to fly. Very stable aircraft, once you got it dialed in. Easily hand launched The elevator and rudders are very responsive. The battery we used was a 3S 3800mah lipo, A bit heavy for this model but this puts the CG where we want it. Being a heavier battery, it needs to fly a little fast and if I let go of the throttle the plane would come down quickly.


## Version 1.0 (Phenoix, Cark Y foam wing)

Wingspan: 800mm  
Sweep: 30 deg  
AUW: 870g  
Power plant: 2212 1000kv motor (10x4.7 prop)  

This is our first attempt at making a flying wing. This wing will be made from styrofoam and since a hot wire cutter is the most efficient way to cut foam, we decided to built a [4-axis CNC hotwire cutter](https://github.com/rahulsarchive/4AxisFoamCutter) to help us make the wing cores. Its been a blast making it, see the repo for the documentation.
![open uav](Images/V1.0/dw1.png)

After a bit of reasearch on fying wings, we settled on the parameters and started working on the design.The wing will have a wingspan of 800mm, swept back at and angle of 30deg and have a blunt nose fuselange in between to carry all the payload. The hot wire cutter helped us tremendousely in cutting out the wing cores. 

![open uav](Images/V1.0/hw1.jpg)

![open uav](Images/V1.0/hw3.jpg)

![open uav](Images/V1.0/asm1.jpg)
For additional strenght we decided to do a composite on the wing cores with wood glue and paper towels. This gave us a hard and rigid outer shell which can take the impact of landings. This shell was coverd with a layer of packing tape to seal all the rough edges and then a layer of vinyl tape to provide some colour.

![open uav](Images/V1.0/asm2.jpg)

![open uav](Images/V1.0/asm3.jpg)
The wing is very light weight with the foam cores and has a tough shell to protect it in impacts. The motor mounts were laser cut on 3mm birch wood and joined to the wing with glue. Though originally desinged for a 2205 2600kv motor. In the end we had to go with a 2212 1000kv motor running a 10x4.7 prop.

We realized one problem when tried to balance the CG of the wing. There was not enough weight on the front to get the CG where we wanted, even with the bigger batttery the plane was still tail heavy. For the intial maiden we added bit of nose weight to bring the CG forward.

The maiden did not go successfully. It was very difficult to control and came down nose first. The wing was very heavy and barely had enough thrust to get it flying. Especially with all the drag from the large front bumpers. We realized that even though we designed for a 30deg sweep angle, after construction, the sweep was around 24deg. This pused the CG point forward and there was not enough weight in the front to balance it.

![open uav](Images/V1.0/asm4.jpg)

we tried putting a flight controller in it to help in stablizing the craft. We were able to fly with the help of flight controller. The model is still too tail heavy and the plane does not have enough thrust to carry the extra nose weight.
![open uav](Images/V1.0/ph1.jpg)

### Note 

1. The single biggest factor that controls the characteristics of a flying wing is the CG.  
2. Wings are very sensitive in the pitch axis and sluggish in the roll axis.  
3. Manually flying them are hard, unless you have a lot of experience or very low wing loading.  
4. Adding a flight controller helps with stability and leveling the aircraft.  

### Choosing an Airfoil 

In our version #2 build, we used the Clark y airfoil for the wing but it did not perform as well as we expected, after a bit of research on tip stalling and how to avoid it in a wing, we stumbled upon the **KFm airfoils**. On paper they seemed very good, so we decided to try them out.

![open uav](Images/Software/KFm3.jpg)

The KF stands for Kline and Foggleman who were the designers of the KF airfoils. Unlike a conventional smooth airfoil. The KFm airfoil uses a series of layers to create stepped airfoils.

There are a number of KF variations, That use a combination of layers and positioning to produce a wing that can be made to emulate what a normal wing airfoil profiles.

![open uav](Images/Software/kfm.jpg)

Drawn up by Dick Kline these give some basic guide numbers  for the various combinations that have been tried. The guide numbers however are open to changes, going thinner will always work, going a bit thicker will often work, go too thick and it wont work. 

The percentage thickness refers to each sections total thickness vs the total chord of the wing, and not the height of the step. The height of the step is only determined by the thickness of your foam sheet. Steps can be raised to give more height and the step effect increases proportionaly with both height and air speed.

The basic stepped airfoil idea is that there will be a vortex produced behind each step that fills in the gap and lets the air flow over over the wing as though  the gap was solid and profiled.

For more details, check the RCgroups form on KFm theory and science.

[Kline-Fogleman-(KFm) Airfoils Advanced Theory Science](https://www.rcgroups.com/forums/showthread.php?1296458-**-Kline-Fogleman-(KFm)-Airfoils-Advanced-Theory-Science-**)

#### Some positive characteristics of KFm airfoils

1. It can handle a wide range of speeds form very slow to fast.  
2. It has much greater range for its center of gravity, it could be moved as much as 40% back, since the entire rear section of the wing is producing lift.  
3. It has very good stall characterisitcs and it retards tip stalling in a flying wing.  

## Version 1.5

This is our first version of our wing using a KFm6 airfoil. The center fuselage is designed to carry a camera, two 2500mah lipo, the flight controller and the motor. It needs to be strong to survive impacts and protect the electronics.

![open uav](Images/V1.5/dw2.png)

![open uav](Images/V1.5/dw3.png)

We decided to keep this on hold until we test out the KFm airfoils on more simpler airframes. 


## Version 2.0 (Albatross, KFM6 wing)

Wingspan: 1000mmm   
Sweep: 35 deg  
root chord: 300mm
tip chord: 250mm
AUW: 1200g  
Power plant: 2826 1500kv motor (9X6 prop) 

Learning from our previous wing build, we decided to increase the sweep angle to 35 deg to push the COG backwards, so the wing would balace with less weight upfront. This wing will be an all foam construction and will accomodate a battery plate to house all the electronics.

![open uav](Images/V2.0/nw0.png)

The wing will have six layers in total, two base layers and two KFm steps on top and bottom. With all the extra foam, the construction will be heavy.

![open uav](Images/V2.0/nw01.png)

The motor mount is designed to sequrely hold on to the layers of foam. Its a minimal design and provides resonable sturdiness when attached to the foam.

![open uav](Images/V2.0/mm1.png)

The two locking clips on the top and bottom, provides assurance that the motor plate won't come loose from the assembly. The entire part is covered in a syntetic resin to provide strength and resist vibration.

The 3D model is converted into **DXF** format and made sutitable for CNC cutting. The design is cut on **Shopbot PRS alpha 3-axis CNC mill** using the [CNC Drag knife](https://github.com/rahulsarchive/cncdragknife) we designed. 


![open uav](Images/V2.0/cut1.png)

We did not have depron foam available with us, so we decided to use **Coroplast** for the construction. It's heavier than depron but is also much stronger, it can take impact well and binds well with hotglue and syntetic rubber adhesives.

![open uav](Images/V2.0/nw1.jpg)

The final assembly with all the layers stuck together.

![open uav](Images/V2.0/mw3.jpg)

![open uav](Images/V2.0/nw2.jpg)

The maiden went beautifully, the KFm performed well, the wing resisted tip stalling and glides like its on rails.Because of the sweep, the COG balances prefectly with a 3800mah 3S lipo. Takes off at about 70% throttle and cruises at 50%. Since we used coroplast, the wing is a bit heavy for a 1m wing span model, as such needs to fly a bit faster to maintain altitude. 

The wing loading on the plane is a bit higher with an area of 28dm^2 and AUW of 1200g. This needs an expert level of control to keep the craft in the air.

### Note

1. Sweep provides dihedral stability and COG leverage.
2. Coroplast is heavier, when desigining a model, built it at 1.5 times scale.
3. 1m wingspan and 28dm^2 wing area is not enough for a 1.2kg model. Wing cube loading puts it in the acrobatic range.
4. choose a motor with a thrust greater than the weight of the model.


## Version 2.1 (TomCat, KFM4 Wing)

Wingspan: 1200mmm   
Sweep: 35 deg  
Root chord: 320mm
Tip chord: 260mm
AUW: 1000g  
Power plant: 2826 1500kv motor (9X6 prop) 

We decided to built a bigger and lighter version of the wing, it will have lighter wing loading and can fly more slowly.

![open uav](Images/V2.1/v21b.png)

The wing is made from **5mm depron** sheets, depron is much more easier to work with than coroplast. It is lighter and can be bend when heated. The wing is made by stacking 4 layers of depron sheets to from the KFm4 airfoil.
![open uav](Images/V2.1/tm1.jpg)

The layers are stuck together with hotglue and syntetic resin. There is thin laminating layer on the depron which provides strength, if you remove the layer by sanding, it will weaken the structure.

![open uav](Images/V2.1/tm2.jpg)

We laminated the wing with black packing tape. The tape provides the strenght for the model, we used a thicker tape and the weight of the model increased by 100g. Light weight packing tape would be a better choice.

![open uav](Images/V2.1/tm4.jpg)

The stripes are added for effect. It does make the wing look cool.

![open uav](Images/V2.1/tm5.jpg)


The wing flies beautifully, it glides forever. Very good slow flying characteristics and controllability. When flying slow wind does push the craft around, buut once you get some speed it flies like an arrow. Very little relex on the elevons was need to keep it level.


### Note

1. Don't be afraid of building bigger wings, they have far more stable and have good slow flying characteristics.
2. Hot glue is heavy, use it sparingly.
3. Depron bonds well with polyurethane glue, for lighter build, can use ordinary glue stick.
4. Built elevons a bit bigger to increase responsiveness at slow speeds.


### Version 3.0 (OrangeFury)

Wingspan: 1400mmm   
Sweep: 30 deg  
Root chord: 380mm
Tip chord: 220mm
AUW: 1500g  
Power plant: 3520 1400kv motor (9X6 prop) 

This will be a slightly bigger wing than the previous one, we want it to carry a downward facing camera for arieal mapping purposes. 

![open uav](Images/V3.0/1.png)

![open uav](Images/V3.0/2.png)

It will be a heavier version witha slighly longer wingspan and reduced sweep angle to bring the CG forward as we will be putting a 6000mah (2X3S) battery and Gopro in the front.

![open uav](Images/V3.0/5.png)

The battery plate with the provisons for attaching two Gopro cameras, one facing downwards and other facing forwards. It will support 2X 2600 mah batteries, a flght controller and receiver.

![open uav](Images/V3.0/3.png)

The design made ready for cutting in the CNC machine using a Drag Knife. 

![open uav](Images/V3.0/4.png)

The parts are split apart and arranged to provide the most economical use of material and provide strength where needed and reduce weight were not needed.

![open uav](Images/V3.0/6.png)

The motor plate with the larger base and  stronger supports for the bigger motor.

![open uav](Images/V3.0/v31.jpg)

The wing is fitted with the MatekF405 flight controller, an Frsky S8r radio, SIK radio telemetry, Skywalker 60A ESC, DYS 3536 1100Kv motor

![open uav](Images/V3.0/v311.jpeg)

## Version 4.0

![open uav](Images/V4.0/01.jpeg)

![open uav](Images/V4.0/02.jpeg)

![open uav](Images/V4.0/1.jpeg)

![open uav](Images/V4.0/2.jpeg)

![open uav](Images/V4.0/5.jpeg)

![open uav](Images/V4.0/6.jpeg)


## Resources 







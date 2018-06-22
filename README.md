# Pt_UAV
A DIY, low cost, Fixed-wing aerial reconnaissance and mapping platform

The aim of this project is to create a working prototype of a fixed wing aircraft for areial reconnsasnce and mapping purpose. We aim to use all open hardware and software for this build, enabling anyone to replicate and build on our exisitng work.


Modules
Main readme (overview)
design documentation (readme)
hardware selection, link to google sheet
basic info on design parameters and areodynamic principles
build log and progress.


Most UAV's today are multirotors, they have the advantage of being able to hover and carry more payload like sensors and other auxilaries. But with the current battery technology, multirotors have a flight time in the range of 15- 20 mins, without compromising on the payload. They make good inspection drones in tight spots and hard to reach places. But because of the way they fly, they brun through a lot of battery power to maintain their flight, hence limiting the flight time and maximum range we can cover.

Fixed winged aircrafts fly on the principle of aerodynamic lift and are far more efficient at covering ground than multirotors. Even though they are considerably harder to design and build than multi-rotors, they make up for with efficiency and area covered in least time.

Our goal is to build a suitable fixed wing platform that can carry the necessary payload and provide uasable data for professional arieal mapping. The craft needs to carry a good GPS system, a downward facing camera for taking images, and be able to fly missions autonomysly.

Our constrains being, it must be simple to make, and easy to use. All the hardware and software are low cost and open-source, without sacrificing saftey and usability.

When choosing an aircraft there are many parameters you can use to select them based on your needs. We wanted to keep thing simple, and minimze the numver of moving components. Hence no tail. The aircraft will be a flying wing. Simpler in construction and have the capability to become an UAV.

Flying wings are a lot harder to control because they are inherently unstable aircrafts. They do not have a tail to stabilise the pitch of the aircraft and have no yaw control. They are very sensitive to the changes in Center of Gravity (CG) and flying them manually is a skill.

Since our objective is to get stable data, flying them manually is not an option. This is where a flight controller comes in. We can use the recent boom in mini racing quads to get cheap and efficent flight controllers.

Most mini quads today use a flightcontroller which has an STM32 chip on it, depending on the performance required, they range from F1 to F7. we settled on an STM32F4 flight controller which is ahappy medium between performance and the number of devices that can be connected.

The flight controller needs software to run, in order to do the function we want it to. There are couple of opensource softwares to choose from like Betaflight, Cleanflight, INAV etc. Most softwares are geared towards multirotors but there are a few which support fixed wing crafts. We chose INAV as the flight control software as it met all the requirements and has some of the best GPS modes available.

##Related projects
 **4-axis Hotwire cutter**
 **CNC Drag knife**
##INAV

##Version 0.5 (Trainer)

Wingspan: 600mm (Polyhedral)
Sweep: Nil
AUW: 800g
Power plant: 2212 1000kv motor (10x4.7 prop)

We decided to make a trainer aircraft as our first plane. This will help us understand all concepts of aerodynamics and and give us a first hand experience in contruction techniques, control mechanisms, and learning to fly an aircraft.

We built a model based on the fish from flight test.

The model is a 3-channel aircraft made from 3mm coroplast sheets and joined together by hot-glue and wodden spars. It was not easy to work with coroplast, it has flutes running parallel to the lenght, cuttting it inot shapes is no easy task. We spend some time cutting the sheets by hand and joining them together with hot glue.

 The model has an all up weight of 800 grams and flies with a 1000kv motor running a 10X4.7 prop, producing aproximately 950g of thrust. The plane takes off at 60-70% throttle and cruises at 60%.

It is a dream to fly. Very stable aircraft, once you got it dialed in. Easily hand launched The elevator and rudders are very sensitive. The battery we used was a 3S 3800mah lipo, A bit heavy for this model but this puts the CG where we want it. Being a heavier battery, it needs to fly a little fast and if I let go of the throttle the plane would come down quickly.


##Version 1.0 (phenoix)

Wingspan: 800mm
Sweep: 30 deg
AUW: 870g
Power plant: 2212 1000kv motor (10x4.7 prop)

This is our first attempt at making a flying wing. This wing will be made from styrofoam and since a hot wire cutter is the most efficient way to cut foam, we decided to built a 4-axis CNC hotwire cutter to help us make the wing cores. Its been a blast making it, see the repo for the documentation.

After a bit of reasearch on lfying wings, we settled on the parameters and started working on the design.The wing will have a wingspan of 800mm, swept back at and angle of 30deg and have a blunt nose fuselange in between to carry all the payload. The hot wire cutter helped us tremendousely in cutting out the wing cores. 

For additional strenght we decided to do a composite on the wing cores with wood glue and paper towels. This gave us a hard and rigid outer shell which can take the impact of landings. This shell was coverd with a layer of packing tape to seall all the rough edges and then a layer of vinyl tape to provide some colour.

The wing is very light weight with the foam cores and has a tough shell to protect it in impacts. The motor mounts were laser cut on 3mm birch wood and joined to the wing with glue. Though originally desinged for a 2205 2600kv motor. In the end we had to go with a 2212 1000kv motor running a 10x4.7 prop.

We realized one problem when tried to balance the CG of the wing. There was not enough weight on the front to get the CG where we wanted, even with the bigger batttery the plane was still tail heavy. For the intial maiden we added bit of nose weight to bring the CG forward.

The maiden did not go successfully. It was very difficult to control and came down nose first. The wing was very heavy and barely had enough thrust to get it flying. We realized that even though we designed for a 30deg sweep angle, after construction, the sweep was around 24deg. This pused the CG point forward and there was not enough weight in the front to balance it.

we tried putting a flight controller in it to help in stablizing the craft. We were able to fly with the help of flight controller. The model is still too tail heavy and the plane does not have enough thrust to carry the extra nose weight.

###Lessons learned

1. The single biggest factor that controls the characteristics of a flying wing is the CG.
2. Wings are very sensitive in the pitch axis and sluggish in the roll axis
3. Manually flying them are hard, unless you have a lot of experience.
4. Adding a flight controller helps with stability and leveling the aircraft.

##Version 2.0


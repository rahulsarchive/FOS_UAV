# Design documentation

Most UAV's today are multirotors, they have the advantage of being able to hover and carry more payload like sensors and auxilaries. But with the current battery technology, multirotors have a flight time in the range of 15- 20 mins, without compromising on the payload. They make goo inspection drones in tight spots and hard to reach places. But because of the way they fly, they brun through a lot of battery power to maintain their flight, hence limiting the flight time and maximum range we can cover.

Fixed winged aircrafts are fly on the principle of aerodynamic lift and are far more efficient at covering ground than multirotors. Even though they are considerably harder to design and build than multi-rotors, they make up for with efficiency and area covered in least time.

Our entire journey has been to build a suitable fixed wing platform that can carry the necessary payload and provide uasable data for professional areal mapping. The plane needs to carry a good GPS system, a downward facing camera for taking images, and be able to fly missions autonomysly.

Our constrains for this build are, it must be simple to make and easy to use, all the hardware and software are open-source, must be low cost without sacrificing saftey and usability.

When choosing an aircraft there are many parameters you can use to select them based on your needs. The most common type of FW UAV are the predator (US Army) They have straight wings, V-tails and can fly for days.

Since we wanted to keep thing simple, we limited the numver of moving components required. Hence no tail. The aircraft will be a flying wing. Simpler in construction and have the capability to become an UAV.

Aerodynamics 101: An airplne flies because its wings generate lift (woah!) but there is more to it than that. See while flying, the craft needs to be in an equilibrium state and how the aircraft acheives this equilibirum determine its flight charateristics.

For a conventional airplane, which has a wing and a tail, the wing creates lift in the upward direction, but the tail creates lift in the downward direction. This is to balance out the forward CG(Center of Gravity) of the aircraft. This is accomplised by keeping the tail at a slighly downward angle than the wing (Decolage). But this comes at a penalty, the tail is working against the wing in lift, so the wing has to compensate for it, which increased angle of attack and hence drag.

So the more forward the CG of an aircraft, the more stable it is, but the more downforce that the tail needs to produce and the more drag it creates( Stay with me).

So how do tail-less aircrafts fly? well, its because of a phenomenon called 'Reflex'. The elevons (aielerons+ evevator) of the wing point ever-so-slightly upwards, therby rasing the nose of the wing up and creating lift. Relex can be accomplished in many ways like, selecting a suitable airfoil, but most modellers give a few clicks of up-trim on their elevons to accomplish this.

## How do we design a flying wing?

Parameters 

1. Center of Gravity (COG)

It is the point where the entire weight of the aircraft balances. COG can change depending on the equipments used in the aircraft. 

2. Center of Pressure (COP)

The COP is the vector sum of all the lift force acting on the wing. Lift is generated all over the wing, but if you add all the vectors, they magically seem to focus at a point. This point is called the COP. Generally this is 25 to 30% of the wing chord from the leading edge. This is the point where all the lift forces add up.

3. Sweep Angle

It is the angle the wing makes with respect to the direction of air in forward flight. Sweeping the wings help in reducing the formation of shockwaves when the plane enters near super-sonic speeds. The sweep creates a pressure gradient in front of the wing, so the shock wave is concentrated on the front of the aircraft.


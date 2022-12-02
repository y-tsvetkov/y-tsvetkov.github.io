---
layout: page
title: BACKUPA cheaper, lighter approach to quadruped manipulation
description: Overview of my IROS 2022 paper about a quadruped robot capable of bimanual manipulation and self-loading with a single limb
img: assets/img/bot_overview.png
importance: 1
category: BACKUP
---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/bot_overview.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Something that's been bothering me about four-legged robots for a while now is how can we make them useful. One of the main things a legged bot can do better than a drone is environmental interaction - pushing buttons, pulling levers, carrying objects etc. For this task they are usually given arm payloads with grippers, which are relatively easy to control. However, these arms take up space and payload capacity, not to mention often costing as much as the robot itself.

Gut feeling and armchair engineering aside, the shortcomings of arms and a need for lighter and cheaper manipulation are starting to become apparent in practice. For example, in the [AIRA 2022](https://www.aira-challenge.com/) challenge, which featured navigation and locomotion tasks for legged robots, teams needed to swap between SLAM payloads and arms to complete all required tasks. Although that's fine for a challenge, in real-world deployment far away from an operator you can't change payloads. In his [ICRA 2022 keynote talk](https://www.youtube.com/watch?v=abdLIFOzdRo), Marco Hutter (founder of ANYbotics) also brought up manipulation if we want practical quadrupeds.

Another thing of note is the [recent interest and progress in the field of two-limbed manipulation](https://www.science.org/doi/10.1126/science.aat8414) (or 'bimanual' if you insist). A second arm is very useful when it comes to carrying big objects or providing support, like holding a bottle with one hand while screwing the cap on with the other, and people much smarter than me have been [devising ways to take advantage of 2 arms](https://arxiv.org/abs/2208.10552) (that's the IROS 2022 Best Paper winner BTW). The point is that it might not be too early to consider having 2 grippers on a quadruped to improve manipulation. Just sticking 2 arms just won't cut it - [Spot has a max payload of 14 kg](https://dev.bostondynamics.com/docs/payload/payload_configuration_requirements) and [2 Spot Arms would weigh 16 kg in total](https://www.bostondynamics.com/sites/default/files/inline-files/spot-arm.pdf), so you're out of luck unless your application involves carrying [at least 122](https://www.uu.edu/dept/physics/scienceguys/2000July.cfm) helium balloons.

## The idea
So, what can we do about it? My thinking went something like this: on a quadruped, we already have 4 legs already designed to be both dynamic and precise. Those legs have 3 motors each, capable of carrying a whole body. We are only 4 DoFs away from an arm + gripper, and these DoFs can have smaller and cheaper motors since the leg motors are the ones doing the heavy lifting (_ba-dum tss_).

In short, _what if we just slap some sort of grippers on the front legs for one- and two-limbed manipulation?_

To see how far could this idea go, I made a small-scale quadruped robot with 3-DoF manipulators on its front legs and normal hind legs. 
 
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/all_experiments_text.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
     ADD DUAL ARM MANIP TO TOP LEFT. LOWER FRAME RATE A BIT. REMOVE THE X4S AND X6S AND ADD THEM TO THE CAPTION MAYBE. IF SOMEBODY ASKS, MENTION THE PIPE PUSHED THE ROBOT AROUND FOR THE DUAL-ARM EXPERIMENT AND YOU RETURNED IT A BIT BACKWARDS. MAKE FOOTAGE MORE NATURAL. ADD SOME MORE TIME ON THE END FRAME SINCE IT'S STILL A BIT TOO SHORT. LINK TO PAPER RATHER THAN VID...? DO I FOLLOW THE WAY I BUILT IT, OR DO I PRESENT IT IN A WAY TO MAKE IT LESS COMPLEX TO UNDERSTAND... IT WENT SMTH LIKE OLD SIHT LEG -> NEW BETTER LEG -> CLAWLESS LEG -> EXPERIMENTS
</div>

Not too shabby... just about enough shabby to make it to IROS 2022, to be precise. 

## Design process:

### 

### Bad Grabby Leg

### Good Grabby Leg

To give this leg manipulation capabilities, I mimicked the anatomy of crab claw with a static tibia and an articulated pincer. Unlike crabs, the pincer here has 2 degrees of freedom so the tip is able to grab objects of varying orientation. 
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/grabby_leg.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Leg with manipulator.
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/cropped_dofs.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Here's only the tibia to show the grabby DoFs more clearly.
</div>

For articulation, the claw is cable-driven, with 3D-printed spiral torsion springs counteracting the cables. The springs were represented as logarithmic spirals with their parameters generated from the nonlinear constrained optimisation approach detailed in [this paper by Scarcia et al](https://asmedigitalcollection.asme.org/SMASIS/proceedings/SMASIS2016/50497/Stowe,%20Vermont,%20USA/289603). MATLAB's `nonlcon` worked well for this. The spring stiffness was chosen to be `servo_max_torque/(2*pi)` and they were pretensioned to 45 degrees for good holding power while not stressing out the servos too much.
 
The cables are routed in such a way that they do not experience contraction/extension as the knee joint rotates. They pass through a channel on the knee axis, followed by a conical channel in the tibia and finally reaching the finger. The routing is best seen in this GIF:
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/publication_preview/robot-leg.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Manipulator cable routing. The cables retain their length as the leg is actuated.
</div>

The cool thing about leg-mounted manipulators is that since all the extra motors will be only responsible for grasping and not counteracting the object's weight and moving it around, the added motors can be considerably smaller (and so would be the added weight). Here are a few interesting design details:

* It prints without slicer supports - there are only 2 tiny ones on the femur (4) that are baked in the model.
* For the knee joint, using 4mm width M3 belts instead of the more common 6mm M2 belts (as with the design I compared against) enables the servo pulley (5) to be mounted under the servo hub instead of over it. This reduces design size and moves the point at which force is applied closer to the base of the shaft, reducing bending stresses.
* The tibia servo motor is integrated as a stress-bearing member, improving leg resistance to side forces.
* The cavities on the inner side of the tibia halves (8-9) add perimeters of material away from the neutral axis, increasing the second moment of area and thus improving stiffness for about the same mass as a part with uniform infill.

### Non-grabby Leg
Before we give a leg manipulation capabilities, we need a leg first. Such a leg would need to be light, stiff and be able to bend its knee both ways to easily grab stuff from the ground with a manipulator (so, no four-bar designs like Stanford Pupper). 

For manufacturing, I don't have access to a CNC/metal laser cutter, so the design has to be 3D-printable. For actuation, I settled on the [Gobilda 2000 Torque](https://www.gobilda.com/2000-series-dual-mode-servo-25-2-torque/) since they have good torque and a range of 270°, allowing for the invertible knee. 2 motors in the knees should also be able to comfortably lift 2 kg from 15 cm away, which was the worst case for the mass of a body + 2 legs. Finally, I probably don't want the leg sections to be longer than 10 cm, just to not test the motor limits too much.

Considering these specs, I started with a belt-driven leg that can be printed without supports:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/normal_leg.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Manipulator-less leg.
</div>

It's about 45% lighter than [the best existing design at this scale I found with 3D-printed legs](https://github.com/OpenQuadruped/spot_mini_mini) and it took some heavy falls while testing without breaking, so I'm happy with it for a quick mod. 

Speaking of comparisons:

## Comparison between the 2 legs and OpenQuad
## Software and testing

I implemented a jerk-continuous motion profile detailed in this paper by Yi Fang et al for low-level control. The paper goes into more details about how exactly it works, but the cool thing is radially mirroring the profile past the midpoint saves some computational time. As for high-level control, I wanted to implement a Blender-based GUI that allows for more intuitive control, but ran out of time and resorted to manually driving the joints. Frankly, I still can't believe I didn't break anything with a poorly placed command, but it did result a somewhat shoddy video.

Even with a far-from-perfect control strategy, the robot managed to hold reliably screwdrivers and rubber ducks of various sizes with a single leg and use a pair of wire cutters on some rubber tubing with 2 legs. The footage of this can be seen near the end of my IROS 2022 presentation:
<div align="center">
  <a href="https://youtu.be/MOX-r0w0K7I">
  	{% include figure.html path="assets/img/iros_video_thumb.png" title="Click for a redirect to the video!" class="img-fluid rounded z-depth-1" %}
  </a>
</div>
## What's next
The results were promising but constrained by the small scale, so I'll be upscaling it to an ANYmal quadruped. It's possible that the moment of inertia increase becomes even smaller with a metal construction (metals have a better strength/mass ratio than PLA). Another thing that might help is that  motors tend to constitute a smaller % of the total mass as a robot gets bigger [add numbers relating motor torque, mass and bot weight to help your point]. Once the upscaled design is done, it will be uploaded here, so stay tuned.

[mention force hysteresis issue, the vid was shoddy because it was shot 1 day before the conference, maybe easier to evaluate as you can more or less predict cord deflection?]

There are many things I'd want to change, so this design is by no means final. For example, although we can control the grasping point, there is no way to change the orientation of the object on the axis connecting the points of contact since it's lacking a DoF to do this. Maybe whole-body motion can be used to circumvent this lack of a DoF, reducing the cost and complexity, but then this will make the gripper harder to use than an arm payload and thus less desirable... other viewpoints on this matter are very welcome here! Feel free to reach out at [Y.T.Tsvetkov@sms.ed.ac.uk](mailto:Y.T.Tsvetkov@sms.ed.ac.uk), I'd love to hear other people's opinions on this.

## Full paper
If you want to read about this work in more detail, you can find a preprint of the paper [here](https://arxiv.org/abs/2207.08765). I'll update it with a IEEE-hosted link once it gets uploaded there too.


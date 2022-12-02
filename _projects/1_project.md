---
layout: page
title: Putting grippers on legs and seeing what happens
description: Overview of my IROS 2022 paper about a quadruped robot capable of manipulation with its legs
img: assets/img/bot_overview.png
importance: 1
category: research
---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/bot_overview.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Something that's been bothering me about four-legged robots for a while now is how can we make them useful. One of the main things a legged bot can do better than a drone is environmental interaction - pushing buttons, pulling levers, carrying objects etc. For this task they are usually given arm payloads with grippers, which are relatively easy to control. However, these arms take up space and payload capacity, not to mention often costing as much as the robot itself.

Gut feeling and armchair engineering aside, the shortcomings of arms and a need for lighter and cheaper manipulation are starting to become apparent in practice. For example, in the [AIRA 2022](https://www.aira-challenge.com/) challenge, which featured navigation and locomotion tasks for legged robots, teams needed to swap between SLAM payloads and arms to complete all required tasks. Although that's fine for a challenge, in real-world deployment far away from an operator you can't change payloads. In his [ICRA 2022 keynote talk](https://www.youtube.com/watch?v=abdLIFOzdRo), Marco Hutter (founder of ANYbotics) also brought up manipulation if we want practical quadrupeds.

Another thing of note is the [recent interest and progress in the field of two-limbed manipulation](https://www.science.org/doi/10.1126/science.aat8414) (or 'bimanual' if you insist). A second arm is very useful when it comes to carrying big objects or providing support, like holding a bottle with one hand while screwing the cap on with the other, and people much smarter than me have been [devising ways to take advantage of 2 arms](https://arxiv.org/abs/2208.10552) (that's the IROS 2022 Best Paper winner BTW). It might not be too early to consider having 2 grippers on a quadruped to improve manipulation. Just sticking 2 arms just won't cut it - [Spot has a max payload of 14 kg](https://dev.bostondynamics.com/docs/payload/payload_configuration_requirements) and [2 Spot Arms would weigh 16 kg in total](https://www.bostondynamics.com/sites/default/files/inline-files/spot-arm.pdf), so a dual-armed Spot could only be useful if your application involves carrying [at least 122](https://www.uu.edu/dept/physics/scienceguys/2000July.cfm) helium balloons.

## The idea
So, what can we do about it? My thinking went something like this: on a quadruped, we already have 4 legs already designed to be both dynamic and precise. Those legs have 3 motors each, capable of carrying a whole body. We are only 4 DoFs away from an arm + gripper, and these DoFs can have smaller and cheaper motors since the leg motors are the ones doing the heavy lifting (_ba-dum tss_).

In short, _what if we just slap some grippers on the front legs for one- and two-limbed manipulation?_

To see how far could this idea go, I made a small-scale quadruped robot with 3-DoF manipulators on its front legs and normal hind legs. 
 
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/all_experiments_text.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    It worked fairly well.
</div>



Quick disclaimer before I start: documenting the bot here is not exactly in chronological order - I made the grabby leg first, followed by the conventional one, the writeup just flows a bit better the other way around. I'll likely do another writeup about the design/build process in a bit more detail.
## Conventional leg
To get to a grabby leg, we need a non-grabby leg first. Such a leg would need to be light, stiff and the knee should bend both ways to easily grab stuff from the ground once we make it grabby (so, no four-bar designs like Stanford Pupper). For manufacturing, I don't have access to a CNC/laser cutter, so 3D-printing it is. For actuation, I settled on the [Gobilda 2000 Torque](https://www.gobilda.com/2000-series-dual-mode-servo-25-2-torque/) since they have good torque and a range of 300°, allowing for the invertible knee. 2 motors in the knees should comfortably lift 2 kg from 15 cm away, which was the projected worst case for the mass of a body + 2 legs. Finally, I probably don't want the leg sections to be longer than 10 cm, just to not test the motor limits too much.

With the design specs out of the way, I started with a belt-driven leg that can be printed without supports:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/normal_leg.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Non-grabby leg.
</div>

It's about 45% or 158 g lighter than [OpenQuadruped, the best fully 3D-printed design I found at this scale](https://github.com/OpenQuadruped/spot_mini_mini) and it took some heavy falls while testing without breaking, so I'm happy with it. Here are a few interesting design details:

* It prints without slicer supports - there are only 2 tiny ones on the femur (4) that are baked in the model.
* For the knee joint, using 4mm width M3 belts instead of the more common 6mm M2 belts (as with the design I compared against) enables the servo pulley (5) to be mounted under the servo hub instead of over it. This reduces design size and moves the point at which force is applied closer to the base of the shaft, reducing bending stresses.
* The tibia servo motor is integrated as a stress-bearing member, improving leg resistance to side forces.
* The cavities on the inner side of the tibia halves (8-9) add perimeters of material away from the neutral axis, increasing the second moment of area and thus improving stiffness for about the same mass as a part with uniform infill.
* I resorted to directly threading the screws in plastic rather than heat set inserts/captive nuts since they'd add quite a bit of weight while [not being that much stronger.](https://youtu.be/2wRc1KbEAU8?t=522)

## Grabby leg

Now comes the fun part. My thinking for how to make the leg grabby was centered on adding a second end effector that can move in 3D relative to the EE of the leg. I was struggling to find a design that could be light while still allowing for 3D movement, until one faithful night of substituting sleep with YouTube I came across [this video of a crab eating noodles](https://www.youtube.com/watch?v=CcYRXgKgxnw). You can notice how when it grabs a noodle, only the upper claw moves, and the lower claw is just a static extension of the lower limb. If there is a 1-DoF wrist before the claw and we give the moving pincer another DoF, that's 3!

The more I think about it, the more I regret not citing this video in the paper, because that idea pretty much got me to the final grabby leg design:
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/grabby_leg.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Grabby leg. <a href="https://en.wikipedia.org/wiki/Carcinisation">Carcinization FTW!</a>
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/cropped_dofs.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Here's only the tibia to show the grabby DoFs more clearly.
</div>

The motors used for the pincer are [these Goteck micro servos](https://www.robotshop.com/products/25kg-goteck-metal-gear-micro-servo). They are really strong and really cheap for their size and their cables come out from the bottom rather than from the back, which allows for a tighter assembly. Lovely little guys.

The pincer is cable-driven, with 3D-printed spiral torsion springs counteracting the cables. The springs were represented as logarithmic spirals with their parameters generated from the nonlinear constrained optimisation approach detailed in [this paper by Scarcia et al.](https://asmedigitalcollection.asme.org/SMASIS/proceedings/SMASIS2016/50497/Stowe,%20Vermont,%20USA/289603). I plugged the equations in MATLAB's `nonlcon` and it worked just fine. The spring stiffness was chosen to be `[max servo torque]/(2*pi)` and they were pretensioned to 45 degrees for good holding power while not stressing out the servos too much. I also lowered the max acceptable stress % from 75% to 60, since PLA is more susceptible to [creep](https://en.wikipedia.org/wiki/Creep_(deformation)).
 
The cables driving the pincer are routed in such a way that they do not experience contraction/extension as the knee joint rotates. They pass through a channel on the knee axis, followed by a conical channel in the tibia and finally reaching the finger. Here's a GIF that can show it much better than I can put it in words:
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/publication_preview/robot-leg.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The cables retain their length as the leg is actuated.
</div>

Even with the additional mass, this leg is still 30% or 104 g lighter than OpenQuadruped's. In terms of moment of inertia (AKA MoI, how hard is it to rotate the leg, think of it as rotational mass), both legs here are still better, even when accounting for the slightly longer length of OpenQuadruped legs. Here's a graph to summarise it all:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/moi_mass_graph.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Apologies for the unnecessary Latin - 'dactylus' is 'pincer', 'sagittal Moi' means 'how hard is it to swing the leg forwards/backwards around the femur servo axis', and 'coronal MoI' is the same for side to side rotation around the coxa servo axis.
</div>

The MoI of the grabby leg is higher than the non-grabby one, which is expected. Looking at the MoIs on a part-by-part basis we can see why:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/moi_per_parts.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Again, 'dactylus' is 'pincer', 'sagittal Moi' means 'how hard is it to swing the leg forwards/backwards around the femur servo axis'. And again, sorry for the Latin.
</div>

The majority of the increase comes just from the wrist servo! MoI depends on the square of the distance from the axis around which you measure it, hence why the femur assembly for example (which constitutes the majority of the mass) is not that rotationally heavy. The wrist servo is about 15 g (a fair amount of weight at this scale) and it's fairly far away, so the increase checks out. Still, wrist servo aside, the pincer and its servos aren't too bad. I'd wager that at a bigger scale, there is a better solution to this that can make the assembly even lighter.

Another thing to note is the price. At $7.50 per micro servo, the electronics for a single pincer come out at $22.50, while an extra arm and gripper would need 3 Gobildas + 4 micro servos, or $126 at the time of writing. The added masses come out at 54 g and 290 g respectively. Overall, an arm would be about 5x more expensive and 5x heavier than a pincer. 

## Software and testing

I implemented a jerk-continuous motion profile detailed in [this paper by Yi Fang et al.](https://www.sciencedirect.com/science/article/pii/S0094114X20301786) for low-level control. It's just a long, piecewise equation you can integrate 3 times to get a displacement profile. The only cool thing about my implementation is that radially mirroring the profile past the midpoint saves some computational time. As for high-level control, I wanted to implement a Blender-based GUI that allows for more intuitive control, but ran out of time and resorted to manually driving the joints. I still can't believe nothing broke from a poorly placed command, but it did result a video with long wait times between movements which I had to manually edit out.

Even with a far-from-perfect control strategy, the robot managed to hold reliably objects of various sizes with a single leg and use a pair of wire cutters on some rubber tubing with 2 legs, neither of which I've seen another quadruped do. The footage of the experiments is in the thumbnail GIF and near the end of my IROS 2022 presentation:
<iframe width="775" height="436" src="https://www.youtube.com/embed/MOX-r0w0K7I" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
## What's next
The results were promising but constrained by the small scale, so I'll be upscaling it to the ANYmal quadruped in my local robotics lab. I expect the moment of inertia increase to become smaller if I used aluminum (for a better strength/mass ratio than PLA). Another thing that might help is that motors tend to constitute a smaller % of the total mass as a robot gets bigger (about 52% for this robot, 36% for ANYmal), meaning I might get away with a directly-driven wrist. And that's not mentioning the much more torque-dense motors I could have access to at a bigger scale. Once the upscaled design is done, it will receive a writeup here, so stay tuned.

There are many things I'd want to change, so this design is by no means final. The wrist will absolutely need to be redesigned because for a big bot, the shaft stresses would be off the charts. Furthermore, although we can control the grasping point, there is no way to change the orientation of the object on the axis connecting the points of contact since it's lacking a DoF for this. Maybe whole-body motion can be used to circumvent this lack of a DoF, reducing the cost and complexity, but then this will make the gripper harder to use than an arm payload and thus less desirable... ugh, this is complicated. Other viewpoints are very welcome here! Feel free to reach out at [Y.T.Tsvetkov@sms.ed.ac.uk](mailto:Y.T.Tsvetkov@sms.ed.ac.uk), I'd love to hear other people's opinions on this.

## Full paper
If you want to read about this work in more detail, you can find a preprint of the paper [here](https://arxiv.org/abs/2207.08765). If you have any feedback, don't hesitate to share it with me at [Y.T.Tsvetkov@sms.ed.ac.uk](mailto:Y.T.Tsvetkov@sms.ed.ac.uk)!


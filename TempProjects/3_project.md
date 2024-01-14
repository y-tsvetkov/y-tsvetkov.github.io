---
layout: page
title: Anemone
description: Soft robot for human-robot performance
img: assets/img/anemone_thumb.jpg
importance: 2
category: work
---

Anemone is a soft, interaction-safe robot built for Edinburgh Futures Institute's Between Two Waters - a thought-provoking dance performance about the relationship between humans and machines. It features an innovative continuum manipulator design that can be made many times faster and cheaper than existing ones. The robot performed a 13-minute dance with Scottish Ballet dancer Madelline Squire. I handled the design, manufacture and programming of the robot, successfully delivering it in a month in parallel with my studies.

## Backstory
The robot was conceptualised in a discussion with the design team, consisting of members from Konpanion. They wanted to portray the hope for humans and machines to coexist symbiotically by using the visual language of the sea anemone, and the way it has formed a beneficial relationship with the clownfish.

We settled on a platform with multiple tentacles, that would hang above the dancer. I had to build 10 tentacles of 1.5m each, on a limited budget (£100 per tentacle), limited manufacturing capabilities (1 3D printer available midnight-8am) and limited time (1 month).

The biggest challenge in this design would be the flexible manipulator body itself, as the part that would take the longest to manufacture. I began with looking at existing designs, and determining if any of them could be adapted for our uses, but considering my constraints, none of them were suitable:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/tentacle_problems.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Instead of printing the tentacle segments, I settled on shaping a pool noodle using 3D-printed tools, saving kilograms of filament and days in manufacturing time. I designed a parametric cutting tool to easily create horisontal, vertical and helical cuts while maintaining high rigidity for minimal material cost:

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/parametric_tool.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

This tool delivered clean cuts to the tentacle segments:
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/pool_noodle_idea.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


The end result is a tentacle body with extremely low cost (£2 per manipulator), low manufacturing time (1 hr per tentacle) and very high degree of customisability. One can very easily make a tentacle with a different radius and length, and even adjust the stiffness along the length of the tentacle using horisontal cuts to obtain a particular behaviour from it.

With this out of the way, I designed the actuation module to interact with the tentacle. My main concerns here were the functionality (actuating the manipulator), as short manufacturing time as possible and the ease of assembly and disassembly. WHAT IF SERVOS ARE QUIETER? To reduce space as much as possible, I took advantage of the servo motor's 180 degree ranges, and made the winches overlap, resulting in a smaller footprint.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/manipulator_base.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/manipulator_tip.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

In terms of electronics, I considered a wall adapter with a power supply, but I found it would not supply enough power to guarantee the servos can all move at the same time. I settled on using multiple Li-Po batteries, making sure their capacities could support movement for the whole 15 minutes.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/pipeline_diagam.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

As the art team was familiar with Blender, I prepared a tentacle rig, and a script that extracs the endpoints of each tentacle and plugs them into an IK solving script.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/pipeline_diagam.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Did it work?
It took a lot of concentrated hard work within this 1 month, but we managed to deliver the project. You can watch the performance [here](https://efi.ed.ac.uk/event/edinburgh-futures-conversations-between-two-waters-a-performance-2/).

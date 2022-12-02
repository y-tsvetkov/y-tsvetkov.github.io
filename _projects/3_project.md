---
layout: page
title: ОБЕЛИСК - a 3D-printed mech model kit
description: a project that redirects to another website
img: assets/img/obelisk_thumb.jpg
importance: 3
category: fun  
---

Every project has a beautiful feature showcase page.
It's easy to include images in a flexible 3-column grid format.
Make your photos 1/3, 2/3, or full width.

To give your project a background in the portfolio page, just add the img tag to the front matter like so:

    ---
    layout: page
    title: project
    description: a project with a background image
    img: /assets/img/12.jpg
    ---

I love model kits, especially ones of giant robots, but most of them don't quite scratch the mechanical-monstrosity itch for me. I had some free time over this summer in between reseach, so I cooked up this guy in Fusion 360 and renders in Keyshot (open images in a new tab for HD):
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/obelisk_front.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/obelisk_back.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/obelisk_irl.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

For references, I used the inner frame of the walking Gundam in Yokohama, the robots from Virus (1999) and a bunch of brutalist buildings from my home city.
It's nearly all M2x8 screws with the exception of the shoulder joint, which is M3 for just a bit more holding power. It's also parametric - you can adjust its scale, clearance (for where parts fit together) and screw holes, so you can make it as big/small as you want or adjust it to fit your 3D-printer's specs.

It was printed on an Ultimaker 3 in my local makerspace. Loose belts and underextrusion aside, it's quite fun and surprisingly durable (it survived, like, 4 falls in the last month, with the exception of the antenna). I'd love to print it in SLA to see it in its full glory and check if the joints would still hold up.

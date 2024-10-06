---
layout: page
title: CurtainNet
description: ACM Sensys'23
img: assets/img/imp1.jpg
importance: 1
category: work
related_publications: true
---

Recent trends in flexible antennas and printed circuit boards present an opportunity to leverage deformable substrates such as textiles to deploy large UHF, VHF and ISM band antenna arrays in smart homes. Low-frequency large antenna arrays are rarely deployed in indoor settings due to their large size which makes them bulky and difficult to deploy. By embedding these arrays on existing surfaces such as curtains, we can improve through-wall sensing, beamforming for IoT devices equipped with low-power radios and indoor localization of Bluetooth tags.

However, antenna arrays on curtains present new challenges since deformation shifts their phase centers and changes the 3D positions of antennas. We present <strong>CurtainNet</strong>, a flexible UHF-band antenna array on a large surface curtain that leverages a combination of optical and RF tracking to compensate for these changes while dealing with occlusions and phase changes. Results show that <strong>CurtainNet</strong> outperforms alternative methods by more than 155% in beamforming performance and increases indoor range by 20m.


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/systemflow.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
     CurtainNet Pipeline
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/imp1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/imp3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    
</div>
<div class="caption">
    (a) Our photo-sensing board with patch antenna. (b) Single board evaluation using Qualisys system.
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/curtainNet.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    A 2 x 4 array setup on store-bought curtains.
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/imp5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
     Array testing setup in a conference room.
</div>



You can also put regular text between your rows of images, even citations {% cite einstein1950meaning %}.
Say you wanted to write a bit about your project before you posted the rest of the images.
You describe how you toiled, sweated, _bled_ for your project, and then... you reveal its glory in the next row of images.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>

The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

{% raw %}

```html
<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
```

{% endraw %}

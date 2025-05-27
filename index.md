<style>
body {
  /* Ensures content has some space and doesn't overlap weirdly if body is too small */
  min-height: 100vh; 
}

#rocket-animation {
  position: fixed;
  /* Centering the animation */
  top: 80%; /* Lower down the page */
  left: 10%; /* More to the left */
  width: 80px; 
  height: 80px;
  transform: translate(-50%, -50%) scale(0.7); /* Scale it down a bit */
  z-index: -1; 
  opacity: 0.4; /* More subtle */
}

#rocket-engine {
  width: 50px; /* Smaller engine */
  height: 80px;
  background-color: #505050; 
  border-top-left-radius: 25px;
  border-top-right-radius: 25px;
  border-bottom-left-radius: 4px;
  border-bottom-right-radius: 4px;
  margin: auto;
  position: relative; 
}

#rocket-engine::after {
  content: '';
  position: absolute;
  bottom: -20px; 
  left: 50%;
  /* We will control transform and scale via JS for flame flicker */
  transform: translateX(-50%); 
  width: 20px;
  height: 0px; /* Start height, will be animated */
  background: linear-gradient(to top, orange, yellow);
  border-radius: 50% 50% 20% 20% / 80% 80% 30% 30%;
  opacity: 0; /* Start opacity, will be animated */
}
</style>

---
layout: default
title: Home
---

# Welcome to My Blog

This is the homepage. Below, you can the stuff I wrote.

## Recent Posts
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a> - {{ post.date | date: "%B %d, %Y" }}
      {% if post.categories.size > 0 %}
        <span class="post-tags">
          {% for category in post.categories %}
            <span class="tag tag-{{ category | slugify }}">{{ category }}</span>
          {% endfor %}
        </span>
      {% endif %}
    </li>
  {% endfor %}
</ul>


<div id="rocket-animation">
  <div id="rocket-engine"></div> <!-- Engine directly in HTML for simplicity -->
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/animejs/3.2.1/anime.min.js"></script>
<script>
document.addEventListener('DOMContentLoaded', function () {
    anime({
        targets: '#rocket-engine',
        rotate: '1turn', // Simpler way to say 360 degrees
        loop: true,
        easing: 'linear',
        duration: 20000 // Slower spin: 20 seconds
    });

    // Flame animation
    anime({
        targets: '#rocket-engine::after',
        height: [
            {value: '25px', duration: 200, delay: 100},
            {value: '35px', duration: 300},
            {value: '20px', duration: 250}
        ],
        opacity: [
            {value: 0.9, duration: 100},
            {value: 0.6, duration: 200},
            {value: 1, duration: 300}
        ],
        scaleY: [
            {value: 1.2, duration: 150, easing: 'easeInOutSine'},
            {value: 1, duration: 150, easing: 'easeInOutSine'}
        ],
        loop: true,
        direction: 'alternate',
        easing: 'easeInOutSine'
    });
});
</script>

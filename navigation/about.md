---
layout: post
title: About
permalink: /about/
comments: true
---

# As a conversation Starter

## Here are some of my interests .


- Music
- Painting
- Sewing
- Beadwork
- Fashion
- Taco Bell. I love taco bell.

<img src="https://upload.wikimedia.org/wikipedia/en/b/b3/Taco_Bell_2016.svg" alt="taco bell logo">

<comment>
places
<comment>

<style>
    /* Style looks pretty compact, 
       - grid-container and grid-item are referenced the code 
    */
    .grid-container {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); /* Dynamic columns */
        gap: 10px;
    }
    .grid-item {
        text-align: center;
    }
    .grid-item img {
        width: 100%;
        height: 100px; /* Fixed height for uniformity */
        object-fit: contain; /* Ensure the image fits within the fixed height */
    }
    .grid-item p {
        margin: 5px 0; /* Add some margin for spacing */
    }

    .image-gallery {
        display: flex;
        flex-wrap: nowrap;
        overflow-x: auto;
        gap: 10px;
        }

    .image-gallery img {
        max-height: 150px;
        object-fit: cover;
        border-radius: 5px;
    }
</style>

<!-- This grid_container class is used by CSS styling and the id is used by JavaScript connection -->
<div class="grid-container" id="grid_container">
    <!-- content will be added here by JavaScript -->
</div>

<script>
    // 1. Make a connection to the HTML container defined in the HTML div
    var container = document.getElementById("grid_container"); // This container connects to the HTML
    // 2. Define a JavaScript object for our http source and our data rows for the Living in the World grid
    var http_source = "https://upload.wikimedia.org/wikipedia/commons/";
    var living_in_the_world = [
        {"flag": "0/01/Flag_of_California.svg", "greeting": "Hey", "description": "California - forever"},
        {"flag": "/4/41/Flag_of_India.svg", "greeting": "Namaste or Namaskaram", "description": "Never lived here but my parents did till 2003"},
    ];

    // 3a. Consider how to update style count for size of container
    // The grid-template-columns has been defined as dynamic with auto-fill and minmax

    // 3b. Build grid items inside of our container for each row of data
    for (const location of living_in_the_world) {
        // Create a "div" with "class grid-item" for each row
        var gridItem = document.createElement("div");
        gridItem.className = "grid-item";  // This class name connects the gridItem to the CSS style elements
        // Add "img" HTML tag for the flag
        var img = document.createElement("img");
        img.src = http_source + location.flag; // concatenate the source and flag
        img.alt = location.flag + " Flag"; // add alt text for accessibility

        // Add "p" HTML tag for the description
        var description = document.createElement("p");
        description.textContent = location.description; // extract the description

        // Add "p" HTML tag for the greeting
        var greeting = document.createElement("p");
        greeting.textContent = location.greeting;  // extract the greeting

        // Append img and p HTML tags to the grid item DIV
        gridItem.appendChild(img);
        gridItem.appendChild(description);
        gridItem.appendChild(greeting);

        // Append the grid item DIV to the container DIV
        container.appendChild(gridItem);
    }
</script>


# Journey through Life

How I Got to Where I Am

- 🏫 Elementary School at Stone Ranch Elementary School
- 🏫 Middle and High School in Glendale (CA), Hoover High graduated '77
- 🎓 Glendale CA Community College, UCLA Extension, LA Wilshire Computer Tech School '77 to '79
- ⛪ England, London Missionary for Church of Jesus Christ of Latter-day Saints '79 to '81
- 💼 Culver City, Glendale CA founder at Ashton-Tate, original PC's dBase 2 and 3 '82 to '87
- 🎓 Eugene Oregon Undergraduate CompSci Degree at University of Oregon (Go Ducks!) '89 to '91
- 💼 Eugene Oregon, founder and owner @ Microniche `88, Point Control CAD CAM developer '91 to '96
- 🏢 San Diego CA Qualcomm, Satellite Comm and 1st Mobile OS (BREW) '96 to '19
- 👨‍🏫 San Diego CA Teacher of Computer Science @ Del Norte High School San Diego '19 to present

### Culture, Family, and Fun

The mango is an important memento to me realizing i typed baseur1 and not baseurl and finally getting images to work..

- My friends and music are really important to me
- One of my favorite bands is a local band called Citrus Jr. I go to a lot of their concerts and I love local shows generally, as well as all music
- There are photos of my friends from across the states, a lot of whom are from the east coast. I met them at a summer program I did at Georgia Tech and I miss them every day
- Finding time for the people I love always makes me grateful for the life I live and the people I've met

<comment>
Gallery of Pics, scroll to the right for more ...
</comment>
<div class="image-gallery">
<img src="{{site.baseurl}}/images/krishna_images/mango_test.jpg" alt="pleaseeeeeeee">
  <img src="{{site.baseurl}}/images/krishna_images/2025_grad.jpg" alt="2025 grad">
  <img src="{{site.baseurl}}/images/krishna_images/belmont_park.jpg" alt="belmont park">
  <img src="{{site.baseurl}}/images/krishna_images/georgia_tech_aquarium.jpg" alt="georgia aquarium">
  <img src="{{site.baseurl}}/images/krishna_images/hoco.jpg" alt="hoco">
  <img src="{{site.baseurl}}/images/krishna_images/mcr.jpg" alt="mcr">
  <img src="{{site.baseurl}}/images/krishna_images/me_and_tanishqa.jpg" alt="driving jk parked">
  <img src="{{site.baseurl}}/images/krishna_images/us_on_christmas.jpg" alt="christmas">
  <img src="{{site.baseurl}}/images/krishna_images/prom.jpg" alt="prom">
  <img src="{{site.baseurl}}/images/krishna_images/us_at_citrus.jpg" alt="citrus jr">
</div>

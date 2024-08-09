<script>
  // IMPORT STATEMENTS
  // Importing necessary D3 modules and Svelte components
  import { forceSimulation, forceX, forceY, forceCollide } from "d3-force";
  import { scaleLinear, scaleSqrt, scaleBand, scaleOrdinal } from "d3-scale";
  import { nest } from "d3-collection";
  import { group } from "d3-array";

  import * as d3 from 'd3'
  
  // Importing custom Svelte components
  import Scrolly from "./Scrolly.svelte";
  import Force from './Force.svelte'
  import ScatterPlot from './ScatterPlot.svelte'
  import Histogram from './Histogram.svelte'
  import AxisY from './AxisY.svelte'
  // import nodes  from "./Force.svelte"
  // console.log(nodes)

  // Importing data
  import data from "./data.js";
  console.log(data)

  // CONSTANTS & SCALES
  // Setting up constants and scales
  let width = 1000;
  let height = 600;
  let circleRadius = 15; 
  // const performanceCategories = ["Below Average", "Average", "Good", "Excellent"]

  var circles = d3.select("svg");
  console.log(circles)

  const colorRange = [
    "#C6458D",
    "#808080",
    "#044892",
    "#D0842E",
    "#00502F"
    ];

  // const colorScale = scaleOrdinal().domain("1", "2", "3", "4", "5", "6").range(colorRange);

  // Setting up dimensions
  let margin = { top: 50, left: 0, right: 10, bottom: 50 };
  $: chartWidth = width - margin.left - margin.right
  $: chartHeight = height - margin.top - margin.bottom

  // Setting up scales using D3
  $: xScale = scaleLinear().domain([0, 100]).range([0, chartWidth]);
  $: yScale = scaleLinear().domain([0, 60]).range([chartHeight, 0]);
  $: roleScale = scaleBand().domain(["1", "2", "3", "4", "5"]).range([chartWidth*0.2, chartWidth * 1]);
  $: monsterScale = scaleBand().domain(["1", "2"]).range([chartWidth*0.2, chartWidth * 1]);
  $: raceScale = scaleBand().domain(["1", "2"]).range([chartWidth*0.2, chartWidth * 1]);
  $: surviveScale = scaleBand().domain(["1", "2"]).range([chartWidth*0.2, chartWidth * 1]);
  $: themeScale = scaleBand().domain(["1", "2", "3", "4", "5"]).range([chartWidth*0.2, chartWidth * 1]);
  $: empowerScale = scaleBand().domain(["1", "2"]).range([chartWidth*0.2, chartWidth * 1]);
  $: directorScale = scaleBand().domain(["1", "2"]).range([chartWidth*0.2, chartWidth * 1]);

  // Setting up additional scales for the histogram
  // $: yHistScale = d3.scaleLinear().domain([0, 10]).range([chartHeight, 0])
  // $: xHistScale = d3.scaleBand().domain(performanceCategories).range([0, chartWidth])
  
  // DECLARATIONS
  // Setting up variables for scrolly telling
  let currentStep;
  let simulation = forceSimulation(data)
  

</script>	

<section>
    <div class="photo-container">
      <img src="./img/women-in-horror-banner.png" alt="Women in Horror Banner" class="photo">
        <figcaption class = "caption">Natalie Portman in Black Swan (2010)</figcaption>
    </div>
</section>

<section>		
  <!-- Section content with scrolly telling -->
  <div class='hero'>
    <!-- <h1>Women in Horror</h1> -->
    <h1>How do the most popular horror movies of all time depict their leading ladies?</h1>
    <h2>By Christina Li</h2>
  </div>
  <div class='spacer' />
  <div class="content">
     <p> I didn't always like horror movies. In fact, I tended to avoid the genre altogether. But some time in the last few years, I started to take notice in how entertaining yet insightful
      horror movies could be in their social commentary. My appreciation for the horror genre has only grown as I discovered more horror movies with complex female characters that captured
      all of the intense emotions women experience - their fear, rage, claustrophobia, quest for revenge or justice. Horror movies of today are unlike the ones in the
      1970s and prior, which were slasher flicks that targeted women as victims to be disposed of.
    <br>
    <br>
    With this in mind, I wanted to look at the portrayal of women in the top horror movies of all time. In order to carry out this movie analysis, I first referred to The Pudding's sci-fi analysis <a href="https://pudding.cool/2024/07/scifi/">Who Killed the World?</a>, along with
    Five Thirty Eight's <a href="https://projects.fivethirtyeight.com/next-bechdel/">The Next Bechdel Test</a>.
    <br>
    <br>
    To categorize and analyze common themes found in horror movies, I did research on existing literature on the representation of women in horror films.
    Ultimately, I used <a href="https://www.routledge.com/The-Monstrous-Feminine-Film-Feminism-Psychoanalysis/Creed/p/book/9780415052597">Monstrous-Feminine</a> by Barbara Creed to look at how women are depicted as monstrous figures in horror movies, as well as
    <a href="https://digitalcommons.pace.edu/cgi/viewcontent.cgi?article=1216&context=honorscollege_theses">Fears and the Female Circumstance: Women in 1970s Horror Films</a> by Lorian Gish as a guide for creating theme categories related to womanhood.
    By analyzing horror movies through a feminist lens, we can see how these films reinforce certain ideas about women, including deep-seated fears and anxieties about womanhood.
    <br>
    <br>
    The top 400 movies of all time were evaluated because they are the most watched and most discussed.
    These movies are the most culturally relevant, contributing to ongoing discussions on the real-life treatment of women.
    <br>
    <br>
    By comparing the top 400 movies in horror to other genres, it's clear that horror is a genre that is quite gendered. Similar to romance genre, nearly half of the top movies have a woman in the leading role.
    <br>
    <br>
    </p>
    </div>
    <div class="chart">
      <img src="./img/Bar-Chart.png" style="width: 1000px; height: auto; display: block; margin: 0 auto;" alt="Women in Horror Banner"> 
      </div>
    <br>
    <br>
    <div class="content">
    <p>WARNING: SPOILERS AHEAD!</p>
  </div>

  <div class= 'section-container'>
    <div class='steps-container'>
      <!-- Scrolly component to handle steps -->
      <Scrolly bind:value = {currentStep}>
        {#each ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p'] as text, index}
          <div class='step' class:active = {currentStep === index}>
            <div class='step-content'>
              <!-- Conditional content for each step -->
              {#if text === 'a'}
                <p>Out of the top 400 horror movies, 187 have a woman in the leading role.
                  <br>
                  <br>
                  Each circle represents a female-centric horror movie.
                  <br>
                  <br>
                  Hover to see each movie title.
                </p>
              {:else if text === 'b'}
                <p>What role do these women play? 
                  <br>
                  <br>
                Categories from left to right:</p>
                <p><a class='bg-tag pink-bg'>Mother or housewife</a></p>
                <p><a class='bg-tag yellow-bg'>Adolescent girl</a></p>
                <p><a class='bg-tag blue-bg'>Professional role associated with beauty</a>
                  <br>
                  (such as dancer or actress)</p>
                <p><a class='bg-tag green-bg'>Professional role associated with caretaking</a>
                  <br>
                (such as nun or nurse)</p>
                <p><a class='bg-tag gray-bg'>Other</a>
                  <br>
                  <br>
                  A lot of horror movies cast the leading lady in a "traditional" gender role associated with femininity and vulnerability.
                  In 105 of the films, the protagonist adheres to one of the traditionally female roles, while 82 do not. 
                  </p>
              {:else if text === 'c'}
                <p>Let's look at some examples of female protagonists who fall in the <a class='bg-tag gray-bg'>Other</a> category, playing a role that is not traditionally seen as feminine.
                <br>
                <br>
              For each movie, there is an explanation provided by ChatGPT 4.o as to how it categorized it.</p>
              {:else if text === 'd'}
              <p>
                Next, let's look at how many women are portrayed as monsters.
                <br>
                <br>
                <a href="https://www.routledge.com/The-Monstrous-Feminine-Film-Feminism-Psychoanalysis/Creed/p/book/9780415052597">The Monstrous-Feminine</a> by Barbara Creed talks about how women in horror movies are often shown as monstrous figures, the "monstrous" being the female reproductive body.
                These portrayals tap into cultural fears and taboos about women's bodies, such as having powers that threaten male characters or giving birth to something monstrous.
                <br>
                <br>
                Using her book as a framework for analyzing the portrayal of women in horror movies, I gave Chat GPT 4.o the book blurb which breaks down "the monstrous-feminine"
                into 7 categories, including archaic mother, monstrous womb, vampire, witch, possessed body, monstrous mother and castrator.
                <br>
                <br>
                How many horror movies depict its female protagonist as a <a class='bg-tag gray-bg'>monster</a> versus <a class='bg-tag pink-bg'>not a monster</a>?</p>
              {:else if text === 'e'}
              <p>Here are some examples of <a class='bg-tag gray-bg'>monsters</a>.</p>
              {:else if text === 'f'}
              <p>The horror genre can hold up a mirror to society, as a way to explore real fears and anxieties that people experience.
                <br>
                <br>
                The female body is often portrayed as grotesque or deformed in some way, making it a source of horror.
                <br>
                <br>
                What fears and anxieties related to womanhood, especially as it relates to the female body, do horror movies reflect?
                <br>
                <br>
                Categories from left to right:
                <br>
                <p><a class='bg-tag pink-bg'>Motherhood</a>
                <br>
                (including pregnancy or childbirth)</p>
                <p><a class='bg-tag yellow-bg'>Gender-based violence</a></p>
                <p><a class='bg-tag green-bg'>Female sexuality</a>
                  <br>
                  (including coming of age)</p>
                <p><a class='bg-tag blue-bg'>Beauty and perfection</a></p>
                <p><a class='bg-tag gray-bg'>Other</a>
                <br>
                <br>
                Perhaps unsurprisingly, gender-based violence is depicted the most in horror movies, followed by stories related to motherhood.
              </p>
              {:else if text === 'g'}
              <p>Some examples of movies about <a class='bg-tag pink-bg'>motherhood</a>.</p>
              {:else if text === 'h'}
              <p>Some examples of <a class='bg-tag yellow-bg'>gender-based violence</a>.</p>
              {:else if text === 'i'}
              <p>Some examples of <a class='bg-tag green-bg'>female sexuality</a>.</p>
              {:else if text === 'j'}
              <p>Some examples of <a class='bg-tag blue-bg'>beauty and perfection</a>.</p>
              {:else if text === 'k'}
              <p>Some examples of themes in <a class='bg-tag gray-bg'>Other</a> category.</p>
              {:else if text === 'l'}
                <p>The good news is that more female protagonists <a class='bg-tag pink-bg'>survive</a> than <a class='bg-tag gray-bg'>die</a> at the end.</p>
              {:else if text == 'm'}
                <p>But that doesn't always mean their story is empowering to watch.
                  <br>
                  <br>
                  Even when there is a woman leading the movie, more of their stories are ultimately about <a class='bg-tag pink-bg'>female victimization</a> than <a class='bg-tag gray-bg'>female empowerment</a>.
                <br>
                <br>
                In order to assess empowerment, I used one of the prompts from Five Thirty Eight's <a href="https://projects.fivethirtyeight.com/next-bechdel/">The Next Bechdel Test</a> as a starting point, which has the following conditions:
                  <br>
                  <span class="code-font">- The female lead has dimension and exists authentically with needs and desires that she pursues through dramatic action
                <br>
                - And the audience can empathize with or understand the female lead's desires and actions</span>
                <br>
                <br>
                I then added more definitions for empowerment versus victimization, so Chat GPT 4.o would have clearer knowledge of how to categorize each movie: 
                <br>
                <span class="code-font">- Additionally, empowerment is defined as the protagonist is able to overcome physical or psychological threats, and she is able to gain agency and autonomy during the movie's runtime. The movie is more about her taking control over her own story
                <br>
                  - Victimization, on the other hand, can be defined as the protagonist is subjected to physical or psychological threats for most of the movie's runtime, and she is not able to gain agency or autonomy. The movie is more about her immediate survival</span>
                </p>
              {:else if text == 'n'}
                <p>Some examples of stories centered around 
                  <a class='bg-tag pink-bg'>female victimization</a>.</p>
              {:else if text == 'o'}
                <p>Some examples of stories centered around
                  <a class='bg-tag gray-bg'>female empowerment</a>.</p>
              {:else if text == 'p'}
                <p>The stories that horror movies tell about women are complex and layered.
                  <br>
                  <br>
                  So, who gets to tell stories about womanhood?
                  <br>
                  <br>
                  Far more of the most popular horror movies of all time have been made by a <a class='bg-tag pink-bg'>male director</a> than a 
                  <a class='bg-tag gray-bg'>female director</a>.
                </p>
              {/if}
            </div>
          </div>
        {/each}
      </Scrolly>
    </div>
    
    <div class= 'sticky' bind:clientWidth = {width}>
      <!-- Dynamic content based on scrolly step -->
      {#if currentStep === 0}
        <Force {width} {height} {margin} {simulation} X ={width / 2} Y ={height/2} circleRadius={10} alpha={0.7} adecay={0.05}/>
      {:else if currentStep === 1}
        <Force {width} {height} {margin} {simulation} X ={d => roleScale(d.Role_Code)} Y ={height/2} circleRadius={10} alpha={0.7} adecay={0.05} fills={"Role_Code"}/>
          <!-- {#each nodes as node}
            <circle fill={colorScale(node.Role_Code)}/>
          {/each} -->
      {:else if currentStep === 2}
      <img src="./img/Photo-Boards-Roles.png" style="width: 1000px; height: auto; display: block; margin: 0 auto;" alt="Women in Horror Banner">
      {:else if currentStep === 3}
        <Force {width} {height} {margin} {simulation} X ={d => monsterScale(d.Monster_Code)} Y ={height/2} circleRadius={10} alpha={0.7} adecay={0.05} fills={"Monster_Code"}/>
        {:else if currentStep === 4}
        <img src="./img/Photo-Boards-Monsters.png" style="width: 1000px; height: auto; display: block; margin: 0 auto;" alt="Women in Horror Banner">
      {:else if currentStep === 5}
        <Force {width} {height} {margin} {simulation} X ={d => themeScale(d.Theme_Code)} Y ={height/2} circleRadius={10} alpha={0.7} adecay={0.05} fills={"Theme_Code"}/>
      {:else if currentStep === 6}
      <img src="./img/Photo-Boards-Motherhood.png" style="width: 1000px; height: auto; display: block; margin: 0 auto;" alt="Women in Horror Banner">
      {:else if currentStep === 7}
      <img src="./img/Photo-Boards-Violence.png" style="width: 1000px; height: auto; display: block; margin: 0 auto;" alt="Women in Horror Banner">
      {:else if currentStep === 8}
      <img src="./img/Photo-Boards-Sexuality.png" style="width: 1000px; height: auto; display: block; margin: 0 auto;" alt="Women in Horror Banner">
      {:else if currentStep === 9}
      <img src="./img/Photo-Boards-Beauty.png" style="width: 1000px; height: auto; display: block; margin: 0 auto;" alt="Women in Horror Banner">
      {:else if currentStep === 10}
      <img src="./img/Photo-Boards-Other.png" style="width: 1000px; height: auto; display: block; margin: 0 auto;" alt="Women in Horror Banner">
      {:else if currentStep === 11}
        <Force {width} {height} {margin} {simulation} X ={d => surviveScale(d.Die_Survive_Code)} Y ={height/2} circleRadius={10} alpha={0.7} adecay={0.05} fills={"Die_Survive_Code"}/>
      {:else if currentStep === 12}
        <Force {width} {height} {margin} {simulation} X ={d => empowerScale(d.Victim_Empower_Code)} Y ={height/2} circleRadius={10} alpha={0.7} adecay={0.05} fills={"Victim_Empower_Code"}/>
      {:else if currentStep === 13}    
      <img src="./img/Photo-Boards-Victim.png" style="width: 1000px; height: auto; display: block; margin: 0 auto;" alt="Women in Horror Banner">    
      {:else if currentStep === 14}    
      <img src="./img/Photo-Boards-Empower.png" style="width: 1000px; height: auto; display: block; margin: 0 auto;" alt="Women in Horror Banner">    
      {:else}
      <Force {width} {height} {margin} {simulation} X ={d => directorScale(d.Director_Code)} Y ={height/2} circleRadius={10} alpha={0.7} adecay={0.05} fills={"Director_Code"}/>
      {/if}
    </div>
  </div>

  
  <div class="content">
    <p>Methodology section to come soon.</p>
    <p>Svelte template can be found <a href="https://svelte.dev/repl/ea206af4d7bc454f969ee6e31fb545b0?version=4.1.1">here</a>.</p>
    <br>
    <br>
    <br>
    <br>
    <br>
  </div>
</section>


<link href="https://fonts.googleapis.com/css2?family=Source+Code+Pro&display=swap" rel="stylesheet">

<style>

    .code-font {
        font-family: 'Source Code Pro', monospace;
        font-size: 16px;
    }

  .photo-container {
    text-align: right;
    margin: 20px
  }

  .photo {
    max-width: 100%;
    height: auto;
  }

  .caption {
    font-style: italic; /* Italicize the caption */
    margin-top: 10px; /* Add some space between the image and the caption */
    font-size: 12px;
  }

  .chart {
    text-align: center;
    justify-content: center;
  }

  h1 {
    text-align: center;
    font-size: 40px;
    font-family: Georgia, 'Times New Roman', Times, serif;
    text-rendering: optimizeLegibility;
  }

  h2 {
    text-align: center;
    font-size: 25px;
    font-family: Georgia, 'Times New Roman', Times, serif;
    text-rendering: optimizeLegibility;
  }

  p {
    text-align: left;
    font-size: 20px;
    font-family: Georgia, 'Times New Roman', Times, serif;
    text-rendering: optimizeLegibility;
    line-height: 1.5em;
  }
  
  a {
    color:#883178;
    }
    
  .bg-tag {
    color: white;
    background-color: black;
    padding-right: 10px;
    padding-left: 10px;
    padding-top: 5px;
    padding-bottom: 5px;
    border-radius: 10px;
  }

  .pink-bg{
    background-color: #C6458D;
  }

  .yellow-bg{
    background-color: #D0842E;
  }

  .blue-bg{
    background-color: #044892;
  }

  .green-bg{
    background-color: #00502F;
  }

  .gray-bg{
    background-color: #808080;
  }


.content {
    max-width: 640px;
    margin: auto;
}

@import url('https://fonts.googleapis.com/css2?family=Barlow+Condensed:ital,wght@1,800&family=IBM+Plex+Serif&display=swap');
    :global(body) {
        overflow-x: hidden;
        width : 100%;
        margin: 0;
        background-color: #FEFAF1
        
    }

    .hero {
        display: flex;
        flex-direction: column;
        justify-content: left;
        text-align: left;
        display: flex;
    }
    
    .content{
        width : 75%;
        margin:auto;
    }
    
  .spacer {
    height: 10vh;
  }

  .section-container {
    margin-top: 1em;
    text-align: center;
    transition: background 100ms;
    display: flex;
  }

  .step {
    height: 100vh;
    display: flex;
    place-items: center;
    justify-content: center;
  }

  .step-content {
    font-size: 1rem;
        background-color: #FEFAF1;
    color: #ccc;
        border: 1px solid #FEFAF1;
    padding: .5rem 1rem;
    /* display: flex; */
    /* flex-direction: column; */
    /* justify-content: center; */
    transition: background, border 500ms ease;
    text-align: left;
        width: 75%;
        margin: auto;
        max-width: 500px;
  }

    .step.active .step-content {
        background-color: #FEFAF1;
        border: 1px solid black;
        color: black;
    }
    
  .steps-container,
  .sticky {
    height: 100%;
  }

  .steps-container {
    flex: 1 1 40%;
    z-index: 2;
  }

    .sticky {
    position: sticky;
    top: 10%;
        flex: 1 1 60%;
    width: 80%;
  }
/* Comment out the following line to always make it 'text-on-top' */
  @media screen and (max-width: 768px) {
    .section-container {
      flex-direction: column-reverse;
    }
    .sticky {
      width: 95%;
            margin: auto;
    }
  }
</style>


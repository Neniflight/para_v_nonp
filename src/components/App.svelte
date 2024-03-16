<script>
  import Graph from "./Graph.svelte"
  import Scroller from "@sveltejs/svelte-scroller";
  import MLE_Vis from "./MLE_Vis.svelte"
  let count, index, offset, progress;
  let width, height;
  let isFirstIndex = false; 
  let isThirdIndex = false;
  let isSeventhIndex = false;

  $: isFirstIndex = index === 0;
  $: isThirdIndex = index === 2;
  $: isSeventhIndex = index === 6;


</script>

<main>
  <Scroller
  top={0.0}
  bottom={1}
  threshold={0.5}
  bind:count
  bind:index
  bind:offset
  bind:progress
>
  <div class="background" slot="background" bind:clientWidth={width} bind:clientHeight={height}>
    <div class="first-index-background" class:visible={isFirstIndex}></div>
    <div class="third-index-background" class:visible={isThirdIndex} ></div>
    <div class="seventh-index-background" class:visible={isSeventhIndex}></div>
    <div class="progress-bars">
      <p>current section: <strong>{index + 1}/{count}</strong></p>
      <progress value={count ? (index + 1) / count : 0} />

      <p>total progress</p>
      <progress value={progress || 0} />
    </div>
  </div>
  <div class="foreground" slot="foreground">
    <!-- first section -->
    <section>
      <h1>Estimating Customer Spending with Parametric and Nonparametric Methods</h1>
    </section>
    <!-- second section -->
    <section>
      <h1>Why do we want to know a customer's distribution of spending?</h1>
      <div class="dropdown-container">
      <div class="dropdown">
        <button class="dropbtn">Recommendations</button>
        <div class="dropdown-content">
          <p>A customer's spending could help in giving other companies recommendations on items to sell. For instance, a low spending customer is not likely to buy items like TVs or computer parts, but is likely to buy cheaper items like fidget toys and school supplies</p>
        </div>
      </div>

      <div class="dropdown">
        <button class="dropbtn">Financial Advice</button>
        <div class="dropdown-content">
          <p>We can offer more insightful finance advice, if we know how our customer generally spends. For instance, if a customer is only distribution of spending mainly consists of smaller purchases on their credit card, we can give them advice to only use the credit card for big ticket items, in order to maximize credit score</p>
        </div>
      </div>

      <div class="dropdown">
        <button class="dropbtn">Fraud Detection</button>
        <div class="dropdown-content">
          <p>We could more easily detect fraudulent purchases. If a customer’s purchase history suggests that the probability of buying a high priced item is very low, it could potentially be a scammer using that account instead of the customer. </p>
        </div>
      </div>

      <div class="dropdown">
        <button class="dropbtn">Categorization</button>
        <div class="dropdown-content">
          <p>We can cluster customers into certain spending habits. From there, we can install different type of tax/bank programs for different types of spending habits in order to maximize profits</p>
        </div>
      </div>
    </div>
    </section>
    <!-- third section -->
    <section>
      <h1>People's Spending Habits</h1>
      <div class="text-container">
        <p>A person's spending habits are affect by factors by</p>
        <li>Social Standing</li>
        <li>Initial Wealth</li>
        <li>Number of Friends</li>
        <li>Family</li>
        <li>and many more Factors!</li>
      </div>
    </section>
    <!-- fourth section -->
    <section>
      <h1>Types of Customer Spending</h1>
      <p>Each person has a unique way or distribution for spending their money. For instance, a typical person could spend consistently $50-60 on a daily basis. This is represented by a normal distribution.</p>
      <h2>Normal Distribution</h2>
      <div class="img-container">
        <img class="img1" src="/images/normal_dist.png" alt="normal">
        <img class="img2" src="/images/normal_man.png" alt="normal">
      </div>      
    </section>
    <!-- fifth section -->
    <section>
      <p>Another person doesn't have much money to spend. Therefore, they make frequent, small purchases very often and never make big purchases. This is represented by an exponential distribution.</p>
      <h2>Exponential Distribution</h2>
      <div class="img-container">
        <img class="img1" src="/images/expon_dist.png" alt="expon">
        <img class="img2" src="/images/poor_person.png" alt="poor">
      </div>
    </section>
    <!-- sixth section -->
    <section>
      <p>This last person has a lot of money to spend. Sometimes they make big purchases and sometimes they make smaller purchases! This is modeled best by a bimodal distribution</p>
      <h2>Bimodal Distribution</h2>
      <div class="img-container">
        <img class="img1" src="/images/bimodal_dist.png" alt="bimodal">
        <img class="img2" src="/images/rich_man.png" alt="rich">
      </div>
    </section>
    <!-- seventh section -->
    <section>
      <h1 class="mystery">INTRODUCING A NEW MYSTERY PERSON</h1>
      <p class="mystery">We have no idea, what kind of spending distribution this mystery person who just transferred banks is.</p>
      <p class="mystery">We only have a sample of his purchases and in order to get more, we have to use time/money to scrape his history.</p>
      <p class="mystery">What is the most optimal way to get this person's spending distribution and habits?</p>
    </section>
    <!-- parametric -->
    <section>
      <MLE_Vis/>
    </section>
    <section>
      <p>Let's say we have Person A. Person A's spending fits a normal distribution.</p>
      <div class="img-container">
        <img class="img1" src="/images/normal_dist.png" alt="normal">
        <img class="img2" src="/images/questionmarkman.png" alt="normal">
      </div>   
    </section>
    
    <!-- visualization section -->
    <section>
      <div class = "caption_text">Here we will sample people's incomes from different income distributions. The blue line represents what their true income distribution looks like.
      The red line is if we assumed their income distribution to be normal. The histogram shows a nonparametric approach to guessing the distribution of this person's income.
      </div>
      <Graph/> 
    </section>
  </div>

  </Scroller>
</main>

<style>
  /* Write your CSS here */
  .background {
    width: 100%;
    height: 100vh;
    position: relative;
    background-color: #B7D3F5;
    outline: green solid 3px;
    overflow: hidden; /* Ensure background image doesn't overflow */

  }
  .caption_text{
    font-size: 20px; /* Adjust the font size as needed */
    color: #14407e; /* Change the color of the text */
    text-align: center; /* Align the text to the center */
    margin-top: 10px; /* Add some top margin for spacing */
}

  .first-index-background {
    width: 100%;
    height: 100vh; 
    position: absolute;
    opacity: 0;
    visibility: hidden;
    background-image: url('images/bank.png'); 
    background-size: cover;
    transition: opacity 0.5s, visibility 0.5s;
  }

  .third-index-background {
    width: 100%;
    height: 100vh; 
    position: absolute;
    opacity: 0;
    visibility: hidden;
    background-image: url('images/types.png'); 
    background-size: cover;
    transition: opacity 0.5s, visibility 0.5s;
  }

  .seventh-index-background {
    width: 100%;
    height: 100vh; 
    position: absolute;
    opacity: 0;
    visibility: hidden;
    background-image: url('/images/mystery_person.jpeg'); 
    background-size: cover;
    transition: opacity 0.5s, visibility 0.5s;
  }

  .first-index-background.visible {
    opacity: 1;
    visibility: visible;
  }

  .third-index-background.visible {
    opacity: 1;
    visibility: visible;
  }

  .seventh-index-background.visible {
    opacity: 1;
    visibility: visible;
  }

  .foreground {
    width: 50%;
    margin: 0 auto;
    height: auto;
    position: relative;
  }

  .progress-bars {
    position: absolute;
    background: rgba(170, 51, 120, 0.2) /*  40% opaque */;
    visibility: visible;
  }

  section {
    height: 80vh;
    /* color: white; */
    outline: rgb(130, 196, 234) solid 3px;
    text-align: center;
    max-width: 900px; /* adjust at will */
    color: black;
    padding: 1em;
    margin: 0 0 2em 0;
    display: flex;
    flex-direction: column;
    align-items: center; /* Center horizontally */
    text-align: center; /* Center text horizontally */
  }

  h1, h2 {
    color: #0E3562;
    font-weight:bold;
    font-family: 'Verdana'
  }

  .dropdown-container {
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;
  }
  .dropdown {
    position: relative;
    display: inline-block;
    margin: 1rem;
  }

  .dropbtn {
    font-family: 'Verdana';
    color: #424242;
    background-color:#BEDCFF;
    border-radius: 10px;
    border-color: #000000;
    border-width: 1px solid;
    font-size: 1.25em;
    padding:0.25em;
    font-weight: bold;
  }

  .dropdown-content {
    display: none;
    position: absolute;
    background-color: #f9f9f9;
    min-width: 20rem;;
    padding: 0.5rem;
    box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2);
    z-index: 1;
    font-family: 'Verdana';
    text-align: left;
  }

  .dropdown:hover .dropdown-content {
    display: block;
  }
  
  p, li {
    font-family: 'Verdana';
    text-align: left;
    font-size: large;
  }

  .text-container {
    display: flex;
    flex-direction: column;
  }

  .img-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .img1 {
    width: 60%;
  }
  .img2 {
    width: 20%;
  }

  .mystery {
    font-family: "Verdana";
    color: white!important;
  }

</style>


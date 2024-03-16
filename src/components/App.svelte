<script>
  import Graph from "./Graph.svelte"
  import Scroller from "@sveltejs/svelte-scroller";
  import MLE_Vis from "./MLE_Vis.svelte"
  import { base } from '$app/paths';
  let count, index, offset, progress;
  let width, height;
  let isFirstIndex = false; 
  let isThirdIndex = false;
  let isSeventhIndex = false;
  let isTenthIndex = false;

  $: isFirstIndex = index === 0;
  $: isThirdIndex = index === 2;
  $: isSeventhIndex = index === 6;
  $: isEighthIndex = index === 7;
  $: isTenthIndex = index === 17;

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
    <div class="eigth-index-background" class:visible={isEighthIndex}>
      <h1 class="slide-in" class:visible={isEighthIndex} class:active={isEighthIndex}>Parametric vs Nonparametric Estimators</h1>
    </div>
    <div class="tenth-index" class:visible={isTenthIndex}>
      <h1>Acknowledgements</h1>
      <div class="img-container">
        <div>
          <h2>Justin Eldridge</h2>
          <img class="potrait" src="{base}/images/justin.png" alt="justin">
        </div>
        <div>
          <h2>Sam Lau</h2>
          <img class="potrait" src="{base}/images/sam.png" alt="sam">
        </div>
      </div>
    </div>
    <!-- <div class="progress-bars">
      <p>current section: <strong>{index + 1}/{count}</strong></p>
      <progress value={count ? (index + 1) / count : 0} />

      <p>total progress</p>
      <progress value={progress || 0} />
    </div> -->
  </div>
  <div class="foreground" slot="foreground">
    <!-- first section -->
    <section>
      <h1>Estimating Customer Spending with Parametric and Nonparametric Methods</h1>
      <h2><a href="https://www.youtube.com/watch?v=DbPK3Ewc3Fo">Video for the Project!</a></h2>
    </section>
    <!-- second section -->
    <section>
      <h1>Why do we want to know a customer's distribution of spending?</h1>
      <p>Hover over the buttons after thinking about it!</p>
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
      <p>These factors produce a spending distribution! Each person has a unique way or distribution for spending their money. For instance, a typical person could spend consistently $50-60 on a daily basis. This is represented by a normal distribution.</p>
      <h2>Normal Distribution</h2>
      <div class="img-container">
        <img class="img1" src="{base}/images/normal_dist.png" alt="normal">
        <img class="img2" src="{base}/images/normal_man.png" alt="normal">
      </div>      
    </section>
    <!-- fifth section -->
    <section>
      <p>Another person doesn't have much money to spend. Therefore, they make frequent, small purchases very often and never make big purchases. This is represented by an exponential distribution.</p>
      <h2>Exponential Distribution</h2>
      <div class="img-container">
        <img class="img1" src="{base}/images/expon_dist.png" alt="expon">
        <img class="img2" src="{base}/images/poor_person.png" alt="poor">
      </div>
    </section>
    <!-- sixth section -->
    <section>
      <p>This last person has a lot of money to spend. Sometimes they make big purchases and sometimes they make smaller purchases! This is modeled best by a bimodal distribution</p>
      <h2>Bimodal Distribution</h2>
      <div class="img-container">
        <img class="img1" src="{base}/images/bimodal_dist.png" alt="bimodal">
        <img class="img2" src="{base}/images/rich_man.png" alt="rich">
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
    </section>
    <section>
      <p>Let's say we have Person A. Person A's spending fits a normal distribution.</p>
      <div class="img-container">
        <img class="img1" src="{base}/images/normal_dist.png" alt="normal">
        <img class="img2" src="{base}/images/questionmarkman.png" alt="normal">
      </div>   
      <p>But we do not know this. We only have a small sample of their spending. </p>
      <p>Let's say we only have 10 recorded spendings!</p>
      <p>The first thing we may do is plot it out using a histogram</p>
    </section>

    <section>
      <h1>Nonparametric Estimator(Histogram)</h1>
      <div>
        <p>Histograms are an example of a <b>nonparametric</b> estimator because they do not take a <b>parameter</b> to make. To make a histogram, you just make bins and sort your data into them.</p>
        <img class = "img1" src="{base}/images/Histogram10.png" alt="hist10">
        <p>This is our sample of 10 sorted into bins, as you can see it does not look like our distribution(notated by the red line). This means we should use something else.</p>
        <p>So instead of raw plotting our data, we will try preprocessing it then plot it</p>
      </div>
      
    </section>

    <section>
      <h1>Parametric Estimator(MLE)</h1>
      <div>
        <p>We will use a MLE(Maximum Likelihood Estimator) are an example of a <b>parametric</b> estimator. We will create an MLE of a normal curve, as it is the most common.</p>
        <p>First we find the mean and the standard deviation of our data, then fit it to the normal distribution equation</p>
        <img class =  "img1" src = "{base}images/MLEfunctions.png"> 
        <p>These functions above are the the most optimal way to find the best parameters given a sample of points</p>
        <img class = "img1" src="{base}/images/MLE10.png" alt="mle10">
        <p>We applied this method to our sample of 10 and <b>WOW!</b> it fits almost perfectly!</p>
        <p>So now we have the shape of Person A's distribution!</p>

      </div>      
    </section>

    <section>
      <p>In this visualization, try yourself to find the optimal parameters to fit the data below, <b>maximizing the likelihood</b> that the data can appear from our plot</p>
      <MLE_Vis/>
      <p>If you notice, moving the <b>mean</b> will slide our plot left and right, and moving the <b>variance</b> changes the width of our plot</p>
    </section>


    <section>
      <h1>So why use nonparametric?</h1>
      <div>
        <p>Parametric requires you know the previous shape of the distribution. We got lucky that we chose the most common distribution and it happened to fit our person's distribution.</p>
        <div class="img-container">
          <img class="img3" src="{base}/images/MLEexp.png" alt="normal">
          <img class="img3" src="{base}/images/MLEbi.png" alt="normal">
        </div>
        <p>If Person A was not a normal distribution, it would fit like these graphs above, for exponential and bimodal respectively. This is not a good estimate.</p>
        <p>Nonparametric gets better with sample size:</p>
        <div class="img-container">
          <img class="img3" src="{base}/images/Histogram500.png" alt="normal">
          <p>In this example we have a sample of 500, and it does fit our distribution quite well.</p>
        </div>  
      </div>      
    </section>

    <section>
      <h1>So why use nonparametric?</h1>
      <div>
        <p>In fact, with enough sample size, our <b>nonparametric approach can fit any distribution!</b></p>
        <div class="img-container">
          <img class="img3" src="{base}/images/HISTbi.png" alt="normal">
          <img class="img3" src="{base}/images/HISTexp.png" alt="normal">
        </div>
        <p>So it is up to you to decide whether you have enough sample size to use a nonparametric approach, or a generalized parametric approach for predicting these distributions.</p>
      </div>      
    </section>

    <!-- visualization section -->
    <section>
      <h1> YOU TRY! </h1>
      <div class = "caption_text">Here we will sample people's incomes from different income distributions.</div>
      <div class = "caption_text"> You can play with the sample size to see when the histogram approach fits better than our MLE approach</div>
      <Graph/> 
    </section>
    <section>

    </section>
    <!-- Takeaways -->
    <section>
      <h1>Takeaways</h1>
      <p>There are <b>tradeoffs</b> between parametric and nonparametric estimators</p>
      <p>If we have pre-existing information about the distribution. Parametric requires much less samples! However if we do not have any info, then we have a very high error rate. <b>↑ chance of error, ↓ cost.</b></p>
      <p>Nonparametric will always eventually capture the true distribution, but it requires many more points. <b>↑accuracy, ↑cost</b>.</p>
      <p>This visualization allows you to visually see these differences and directly compare between parametric and nonparametric for different types of customers. Typically, these concepts are introduced separately, so this visualization allows for better comparisons and tradeoffs. </p>
    </section>
    <section>

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
    /* outline: green solid 3px; */
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
    background-image: url('bank.png'); 
    background-size: cover;
    transition: opacity 0.5s, visibility 0.5s;
  }

  .third-index-background {
    width: 100%;
    height: 100vh; 
    position: absolute;
    opacity: 0;
    visibility: hidden;
    background-image: url('types.png'); 
    background-size: cover;
    transition: opacity 0.5s, visibility 0.5s;
  }

  .seventh-index-background {
    width: 100%;
    height: 100vh; 
    position: absolute;
    opacity: 0;
    visibility: hidden;
    background-image: url('mystery_person.jpeg'); 
    background-size: cover;
    transition: opacity 0.5s, visibility 0.5s;
  }

  .eigth-index-background {
    width: 100%;
    height: 100vh; 
    position: absolute;
    opacity: 0;
    visibility: hidden;
    background-image: url('/images/para_v_nonp.png'); 
    background-size: cover;
    transition: opacity 0.5s, visibility 0.5s;
  }

  .slide-in {
    opacity: 0; /* Start with 0 opacity */
    position: absolute;
    top: 10%;
    left: 50%;
    transform: translateX(100%); /* Start with element below its final position */
    transition: opacity 0.5s ease-in-out, transform 0.5s ease-in-out; /* Transition properties */
  }

  .slide-in.active {
  opacity: 1; 
  transform: translate(-50%, -50%);
}

  .tenth-index {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.5s, visibility 0.5s;
  }

  .tenth-index.visible {
    opacity: 1;
    visibility: visible;
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

  .eigth-index-background.visible {
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
    /* outline: rgb(130, 196, 234) solid 3px; */
    text-align: center;
    max-width: 100%; /* adjust at will */
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
  .img3 {
    width: 40%;
  }
  .img3 {
    width: 50%;
  }
  .mystery {
    font-family: "Verdana";
    color: white!important;
  }

  .potrait {
    margin-left: 2rem;
    margin-right: 2rem;
  }

</style>


Project Overview: Tackling the Hidden Landscapes of Black-Box Optimization

1. What We Were Up Against
   
When I first started this project, the whole setup felt a bit wild. We were handed eight completely hidden "black-box" systems and told to find the input settings that would give us the absolute highest performance scores. The catch? We had zero visibility. No formulas, no equations, and no gradients to tell us which way was up. We were essentially flying blind in continuous spaces that went from basic 2D maps all the way up to a massive, mind-bending 8-dimensional space.

To make it interesting, each function simulated a different messy, real-world engineering headache:

●Function 1 (2D): Trying to spot a needle in a haystack—hunting for tiny contamination points in a massive, mostly empty radiation field.

●Function 2 (2D): Navigating a noisy, bumpy model to find the best possible log-likelihood score.

●Function 3 (3D): Mixing and balancing raw variables for a drug discovery simulator.

●Function 4 (4D): Figuring out the most cost-effective arrangement for a physical warehouse layout.

●Function 5 (4D): Tuning factory settings to squeeze the highest chemical product yield out of a processing plant.

●Function 6 (5D): Sweeping through a five-ingredient parameter mix to optimize a cake recipe.

●Function 7 (6D) & Function 8 (8D): Digging through complex, multi-modal validation terrains to tune machine learning models with 6 and 8 hyperparameters.

2. Our Game Plan and Architecture
3. 
Because running these tests was incredibly slow and we had a strict budget of only one guess per function per week for 13 weeks, we couldn't just guess randomly or use a brute-force grid search. We would have run out of time instantly. Instead, I built a modular Bayesian Optimization pipeline to work smarter, not harder.

[ Data Ingestion ] ──► [ Probabilistic Model ] ──► [ Exploration Strategy ] ──► [ Next Best Guess ]
 (Initial Data +                  (Gaussian Process)           (Expected Improvement      (Actual Coordinates)
 Weekly Progress)                                                    via Random Restarts)

 The system relies on three core pieces:

 1.The Predictive Model: I used a Gaussian Process Regressor to create a live probability map of the hidden surfaces. It keeps track of two things: what it thinks is the best path forward (mean) and where it is completely unsure (standard deviation). I chose a Matérn 5/2 kernel because it is rugged enough to capture sudden, sharp peaks without smoothing them over like a standard RBF kernel does.
 
2.Target Balancing: The output scores were all over the map—Function 1 spit out microscopic fractions down to 10⁻¹²⁴, while Function 5 soared past 1200. To stop the model from crashing or ignoring the tiny numbers, I forced a strict normalisation layer (normalize_y=True) right into the training loop so everything scaled nicely.

3.The Guessing Engine: I hand-coded an Expected Improvement (EI) formula to mathematically weigh risk versus reward. To find the highest peak on this bumpy preview map, the code throws 50 random starting points across the space and uses the L-BFGS-B algorithm to climb to the absolute highest point.

3. The Mid-Project Trap and the Week 9 Pivot
   
My journey over the 13 weeks was a massive learning curve. It basically split into two completely different phases divided by a major breakthrough:

[Weeks 1–8: Cautious Over-Exploiting] ──► [Week 9: The Global Reset Pivot] ──► [Weeks 10–13: Precision Drilling]

●Weeks 1 to 8 (Getting Hopelessly Stuck): Early on, I was way too conservative. I wanted to protect my weekly score improvements, so I set the exploration parameter low (ξ=0.01) and kept the code searching in a tight, safe circle around my best points. This worked fine for the easy 2D tasks, but it was a disaster for the high-dimensional functions. Function 7 completely flatlined at a mediocre score of 0.591998 for four straight weeks. Because I kept sampling in the same small cluster, the model's length scales collapsed, making it think the rest of the universe was completely blank.

●The Week 9 Breakthrough: By Week 9, I realised that playing it safe was a losing strategy. I rewrote the pipeline to force a complete global reset. I made the code re-aggregate the entire baseline data matrix directly from the DATA_PATH folders alongside our active weekly logs. I cranked the multi-start budget up to 50 random global restarts and pushed the exploration jitter up to ξ=0.05. This forced the algorithm to completely break away from its safe zone and take a wild leap across the map. The payoff was massive: Function 7 violently exploded from 0.591998 to 1.364919, and Function 8 leapt to 9.598432.

●Weeks 10 to 13 (Locking It Down): Once the global pivot landed us squarely inside the true highest valleys, I shifted the code back into pure precision mode. I dialled the exploration jitter all the way down to 0.01 (and eventually 0.0001 by Week 12) to stop the model from wandering and let it pinpoint the exact tip of the global peaks.

4. Final Outcomes and Reflections

By the time Week 13 rolled around, the optimization loop had successfully converged on major peaks across all eight tasks. Here is where the journey ended:

●Function 1 (2D): Hit a peak of 1.088195 × 10⁻⁷ at [0.684321, 0.712345]

●Function 2 (2D): Hit a peak of 0.737285 at [0.710000, 0.930000]

●Function 3 (3D): Hit a peak of -0.023599 at [0.520000, 0.640000, 0.360000]

●Function 4 (4D): Hit a peak of -3.820595 at [0.580313, 0.430313, 0.430313, 0.280313]

●Function 5 (4D): Hit a peak of 1258.716961 at [0.230000, 0.860000, 0.890000, 0.890000]

●Function 6 (5D): Hit a peak of -0.636036 at [0.747422, 0.177422, 0.747422, 0.717422, 0.047422]

●Function 7 (6D): Hit a peak of 1.364970 at [0.057896, 0.491672, 0.247422, 0.218118, 0.420428, 0.730970]

●Function 8 (8D): Hit a peak of 9.598482 at [0.056447, 0.065956, 0.022929, 0.038786, 0.403935, 0.801055, 0.488307, 0.893085]


Ultimately, this capstone project hammered home a massive lesson: machine learning models are entirely a reflection of the data you give them. If you get lazy and sample the same comfortable spots, your model will develop massive blind spots. True optimisation success in tough, high-dimensional terrains requires having the patience to explore broadly and trust the data long before rushing to cash in on a local win.

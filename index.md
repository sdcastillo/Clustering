---
layout: default
title: Clustering
description: Explanations, simulations, and comparisons of k-means, k-medoids, and k-means++.
samwiki: true
---

<p class="sw-intro">These are student notes on clustering, written by Sam Castillo while studying mathematics at UMass Amherst. They were meant as the first in a series: an explanation, a discussion, simulations, and comparisons of clustering methods, drafted in R and first posted on RPubs.</p>

<p>The main note treats clustering as a mixture problem. A retailer can watch corporate accounts, leisure shoppers, and budget-constrained students behave differently, without a column that names those groups. The task is to cut a collection of observations into homogeneous subsets. K-means does that for continuous measurements. Choose the number of clusters <em>K</em>, place <em>K</em> centers, assign every point to the nearest center by Euclidean distance, and replace each center with the coordinate-wise mean of the points that landed on it. Those last two steps repeat. The write-up is clear that the loop can stop at a local minimum of the within-cluster deviance, so the usual remedy is to restart from many different centers.</p>

<p>The geometry is built on purpose. One sample is two normal clouds in the plane, and k-means with two centers recovers the split that was simulated. A second sample is less tidy, and the same fit is drawn twice, once with <em>K</em> = 2 and once with <em>K</em> = 3. The note then leaves the plane for Fisher’s iris measurements: sepal length, sepal width, petal length, and petal width for 50 flowers from each of three species. Distance-based assignment is sensitive to units, so each column is scaled by subtracting its mean and dividing by its standard deviation. An elbow plot of the total within-cluster sum of squares, for <em>K</em> from 1 to 10, flattens near <em>K</em> = 3. Density plots show the same four measurements before that split and after it.</p>

<p>The last section of the main note swaps the mean for a median. K-medoids, through <code>kmedoids</code> in the <code>clue</code> package, is harder for a wild point to drag: one iris sepal length is spiked, the columns are scaled either as standard scores or by min–max feature scaling, and the within-cluster sum of squares is added up by hand. A second notebook implements k-means++ from Arthur and Vassilvitskii. The first center is drawn at random. Each later center is sampled with probability proportional to its squared distance to the nearest center already chosen, which pushes the starts apart, and ordinary k-means runs from that initialization. The check is a smaller stand-in for the paper’s Gaussian simulations.</p>

<p>Both rendered notes are the original R Markdown pages, plots included. This landing page is only the guide in front of them.</p>

<h2>Key points</h2>
<ul>
  <li>K-means fixes <em>K</em>, assigns points by Euclidean distance, and updates each center to the mean of its cluster.</li>
  <li>Random starts matter, because the iteration can settle on a local minimum of within-cluster sum of squares.</li>
  <li>Columns need a common scale. The iris example uses standard scores; the outlier example also tries min–max feature scaling.</li>
  <li>On the scaled iris data, the elbow in total within-cluster sum of squares sits near three clusters.</li>
  <li>K-medoids uses a median, so in the planted-outlier example it is less pulled by a single extreme sepal length.</li>
  <li>K-means++ chooses later initial centers with probability proportional to squared distance from the nearest center already picked.</li>
  <li>The simulations and the k-means++ functions stay in the original HTML notes linked below. The <code>.Rmd</code> sources remain in the GitHub repository.</li>
</ul>

<h2>Notes</h2>
<div class="sw-grid">
  <article class="sw-card">
    <div class="sw-card-top">
      <h3 class="sw-name"><a href="{{ '/kmeans_clustering.html' | relative_url }}">K-means and k-medoids</a></h3>
      <span class="sw-lang">R Markdown</span>
    </div>
    <p class="sw-badge">Main note</p>
    <p class="sw-desc">Simulated clusters in the plane, the iris elbow plot, pre- and post-cluster densities, and a medoid comparison with a planted outlier.</p>
    <div class="sw-actions">
      <a class="sw-btn sw-btn-live" href="{{ '/kmeans_clustering.html' | relative_url }}">Open note</a>
      <a class="sw-btn sw-btn-source" href="http://rpubs.com/samdcastillo/kmeans_clustering">RPubs</a>
    </div>
  </article>
  <article class="sw-card">
    <div class="sw-card-top">
      <h3 class="sw-name"><a href="{{ '/kmeans_plus_plus.nb.html' | relative_url }}">K-means++</a></h3>
      <span class="sw-lang">R notebook</span>
    </div>
    <p class="sw-badge">Initialization</p>
    <p class="sw-desc">An R implementation of Arthur and Vassilvitskii’s seeding rule, then a smaller Gaussian comparison against ordinary k-means.</p>
    <div class="sw-actions">
      <a class="sw-btn sw-btn-live" href="{{ '/kmeans_plus_plus.nb.html' | relative_url }}">Open notebook</a>
    </div>
  </article>
</div>

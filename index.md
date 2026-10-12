---
layout: default
title: Clustering
description: Explanations, simulations, and comparisons of k-means, k-medoids, and k-means++.
samwiki: true
---

<p class="sw-intro">These notes explain clustering methods Sam Castillo learned in mathematics at UMass Amherst and wrote down while employed as an actuarial analyst, in a role that used these algorithmic measures. They were meant as the first in a series: an explanation, a discussion, simulations, and comparisons, drafted in R and first posted on RPubs.</p>

<p>The same family of methods sorts people, not only flowers and simulated clouds. Credit, insurance, hiring, housing, and similar decisions group observations by distance, by a chosen number of centers, and by whatever columns happen to be on the file. Sam has worked to keep that step from becoming a quiet way to deny U.S. citizens, and people more broadly, equal treatment. The aim is to protect human rights, and the civil rights that U.S. law already names, from algorithmic bias and discrimination. This page is that support for human rights. It is not a protest against politics.</p>

<p>Publishing the mechanics is part of that work. A reader can see that Euclidean assignment follows correlated stand-ins when a protected trait is left out of the table, that scaling changes who is “near” a center, that a mean is pulled by an extreme point, and that a local minimum can freeze a bad partition. The notes below keep those facts in the mathematics. The reading list after them is the legal and ethical record he uses alongside the formulas: how bias enters, how U.S. and other law tries to answer it, and why a fairness metric that only inspects the code can still leave the harm in place.</p>

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
  <li>Sam wrote the notes as an actuarial analyst, in a role that used these measures, and publishes them in support of human rights and the rights of U.S. citizens against algorithmic discrimination.</li>
</ul>

<h2>Reading on algorithmic bias and rights</h2>
<p>The search behind this section returned ten hits. Two are second copies of the same papers, so there are eight articles. They are summarized here. Sam is not an author of them. They are the research he points to when a clustering method is used on people.</p>

<h3><a href="https://arxiv.org/pdf/2508.14954">Bridging research and U.S. civil enforcement</a></h3>
<p>Colleen V. Chien, Anna Zink, and Irene Y. Chen study algorithms in credit scoring, housing, and employment, where civil enforcement has become a practical check on discrimination. Fairness research is often too abstract for a case. From 15 U.S. actions, covering agency enforcement, class actions, and individual suits, they name five gaps: finding an equally accurate and less discriminatory alternative, cascading bias from one system into the next, quantifying disparate impact, getting past information barriers, and handling missing data on protected groups.</p>

<h3><a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC10087838/">Directly discriminatory algorithms</a></h3>
<p>Jeremias Adams-Prassl, Reuben Binns, and Aislinn Kelly-Lyth challenge the habit of treating algorithmic bias only as indirect discrimination. Under UK and EU law, indirect discrimination can be lawful if a practice is a proportionate way to pursue a legitimate aim, so a biased system can be justified and the legal claim disappears. Direct discrimination is stricter: less favorable treatment because of a protected characteristic is generally unlawful. They argue proxy discrimination, from a tainted target, and sampling bias, from unrepresentative training data, often belong in that stricter category. A prima facie case can rest on two showings: a similarly situated person without the characteristic gets a better output, and the outputs are consistently skewed, as when otherwise identical applications score lower for women than for men. The PMC PDF in the source list is this same article.</p>

<h3><a href="https://www.mdpi.com/2075-471X/14/3/41">Algorithmic bias as a legal dilemma</a></h3>
<p>Gergely Ferenc Lendvai and Gergely Gosztonyi compare a fragmented U.S. approach with the EU’s GDPR, Digital Services Act, and AI Act. They describe U.S. law as rooted in civil rights and the Fourteenth Amendment, with equality, non-discrimination, and transparency carried by sector statutes and agencies such as the Fair Credit Reporting Act and the EEOC, plus newer state rules: New York City’s Local Law 144 on audits of hiring tools, and Colorado’s broader statute on algorithmic decisions. There is still no single U.S. statute aimed at algorithmic bias. On the EU side, the AI Act’s Recital 27 ties bias to diversity, fairness, and non-discrimination; Article 14 requires oversight that can question automation bias; Article 15 tells high-risk systems to limit biased feedback loops. They also use the Obermeyer health-care result, where predicted need tracked past spending and so favored groups that already received more care, and they argue that regional rules will keep reproducing inequality under a claim of objectivity unless governments, developers, and civil society share a rights standard that can reach transnational systems.</p>

<h3><a href="https://brie.berkeley.edu/sites/default/files/brie_wp_2018-3.pdf">Algorithms, platforms, and ethnic bias</a></h3>
<p>Selena Silva and Martin Kenney argue that tools carry the beliefs and goals of the people who build them, including ones nobody stated. Dropping race or gender from the column list does not remove them when other variables are highly correlated, or when the database already encodes the old pattern. Facial recognition trained on too narrow a set of faces performs worse on the groups left out. Studies of sentencing, probation, and police stops have repeatedly found biased outcomes. Machine learning makes this harder, because a black-box score cannot be explained, and an unexplained decision is difficult to contest.</p>

<h3><a href="https://scholarship.law.unc.edu/cgi/viewcontent.cgi?article=1493&amp;context=ncjolt">Fair admissions, fair decisions, and fair outcomes</a></h3>
<p>McLean Waters looks at education, employment, healthcare, and housing. Bias enters without ill intent: through the priorities built into the system, through historical data, and through outputs that still fall harder on some groups. Disparate impact can remain even when the model never receives a protected characteristic and was not trained on an obviously biased label. The article treats that pattern as unacceptable discrimination and looks to anti-discrimination law, rules on how models are built, and audits that continue after deployment.</p>

<h3><a href="https://arxiv.org/pdf/2107.04642v2">From formal to substantive fairness</a></h3>
<p>Ben Green argues that the usual fairness toolkit is formal: it judges only the explicit function of the algorithm. That is a weak instrument for equality in criminal sentencing, hiring, and social services. Algorithms adopted to reduce human bias have themselves favored men over women and white defendants over Black defendants. On COMPAS, ProPublica found 58.8 percent of Black defendants labeled high risk, against 34.8 percent of white defendants. Green’s alternative is substantive. It asks whether the system pushes back on a social hierarchy or automates it. The unfairness of a risk score is not settled by showing that the formula was applied the same way to everyone.</p>

<h3><a href="https://pmc.ncbi.nlm.nih.gov/articles/PMC7892355/">Algorithmic injustice and relational ethics</a></h3>
<p>Abeba Birhane writes that machine learning automates historical patterns in medical care, crime prediction, welfare eligibility, and hiring, and that the people already at the margin absorb the harm. Most proposed fixes stay technical and do not start from those communities. Relational ethics moves the question from a lone decision rule to the relationships the system is in, and it asks that any remedy begin with the people who can recognize the harm because they bear it.</p>

<h3><a href="https://www.frontiersin.org/journals/artificial-intelligence/articles/10.3389/frai.2024.1320277/pdf">Types of algorithmic discrimination in U.S. practice</a></h3>
<p>Xukang Wang, Ying Cheng Wu, Xueliang Ji, and Hongpeng Fu review 85 articles and separate five types: bias in the algorithmic agent, discrimination through feature selection, proxy discrimination, disparate impact, and targeted advertising. U.S. practice, as they map it, mixes civil-rights principles, controls before deployment, liability after the fact, self-regulation, and outside regulation. Title VII of the Civil Rights Act can hold an employer responsible for disparate impact from a practice that looks neutral, including an algorithm. From the privacy side, they describe the FTC treating this harm as a violation of rights in personal data, using after-the-fact examination and “privacy by design.” The mitigations in the review are practical and partial: clean or reweight training data, add fairness constraints, and audit. The Frontiers PDF and the PMC copy in the source list are this same paper.</p>

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

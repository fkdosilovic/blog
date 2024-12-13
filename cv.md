---
layout: page
title: CV
permalink: /cv/
---

<style>
.h2 {
  font-weight: bolder;
  font-size: larger;
  border-bottom: 2px solid black;
}

.h3 {
  font-weight: bolder;
  font-size: large;
  color: gray;
  margin-top: 25px;
  border-bottom: 2px solid gray;
}

.experience-entry {
  display: inline-flex;
  margin-bottom: 3ch;
}

.experience-entry .entry-date {
  font-weight: bold;
  width: 12ch;
}

.experience-entry .entry-description {
  width: 60ch;
  margin-left: 5ch;
}

ul {
  padding-left: 1em
}

</style>

<p class="h2">Work Experience</p>

<div class="experience-entry">
<span class="entry-date">Oct 2023. - Present</span>
<span class="entry-description"><strong>NLP Engineer</strong> at <strong>Newfire Global Partners</strong>, Zagreb, Croatia
<br />
<ul>
    <li>Interviewed and evaluated ML/NLP engineer candidates</li>
    <li>Trained, evaluated, and integrated Transformer-based multi-task relation linking model that reduced the false-positive extractions by 90%</li>
    <li>Trained, evaluated, and integrated Transformer-based multi-task relation linking model that 1) proved the usefulness of a multi-task approach for our service, 2) set the training and evaluation standards and guidelines for future multi-task models, and 3) provided foundation for currently 3 multi-task models in the service</li>
    <li>Trained, evluated and integrated a general, Transformer-based word sense disambiguation (WSD) language model that: 1) achieved 80% zero-shot accuracy, 2) replaced five specialized WSD models, and 3) improved the overall service</li>
    <li>Created CLIs for automatic data cleaning (removing duplicates, near-duplicates, and outliers) and consistency verification</li>
</ul>
</span>
</div>

<div class="experience-entry">
<span class="entry-date">Oct 2021. - Sep 2023.</span>
<span class="entry-description"><strong>Research Associate</strong> at <strong>Text Analysis and Knowledge Engineering Lab, University of Zagreb, Faculty of Electrical Engineering and Computing</strong>, Croatia
<br />
<ul>
    <li>Developed and deployed REST API for event extraction that processed over a million news articles over a 6 month period</li>
    <li>Trained, evaluated, and adapted Transformer-based language models for efficient and robust text classification and extractive question answering</li>
    <li>Improved the event extraction pipeline’s throughput by over 300% while maintaining extraction performance and robustness</li>
    <li>Lead and supervised two annotation projects, a text classification and named entity linking, with a team of 6 annotators in each project. Additionally, co-lead an annotation project (sequence labeling) with a team of 8 annotators</li>
    <li>Co-mentored 3 student’s BSc thesis and 2 student’s MSc thesis</li>
    <li>Teaching assistant for Artificial Intelligence course and Machine Learning course</li>
</ul>
</span>
</div>

<div class="experience-entry">
<span class="entry-date">Feb 2018. - Jun 2019.</span>
<span class="entry-description"><strong>Junior Robotics Software Engineer</strong> at <strong>Gideon</strong> (Brothers), Zagreb, Croatia
<br />
<ul style="vertical-align: middle;">
<li>Developed a centralized system for coordinating a heterogeneous fleet of autonomous robots</li>
<li>Completed a comprehensive review of optical flow algorithms and models, and
evaluated their potential for use on autonomous mobile robot</li>
</ul>
</span>

</div>

<div class="experience-entry">
<span class="entry-date">Summer 2017.</span>
<span class="entry-description"><strong>Game AI Developer Intern</strong> at <strong>Lion game Lion</strong>, Zagreb, Croatia
<br />
<ul>
<li>Developed an AI, based on Monte Carlo Tree Search, for a turn-based
strategy game that surpassed human performance.</li>
</ul>
</span>
</div>

<p class="h2">Publications</p>

<ul>
    <li style="font-size:11pt"> F. K. Došilović, M. Brčić and N. Hlupić, "Explainable artificial intelligence: A survey," <em>2018 41st International Convention on Information and Communication Technology, Electronics and Microelectronics (MIPRO)</em>, 2018, pp. 0210-0215, doi: 10.23919/MIPRO.2018.8400040.
    </li>
</ul>

<br />
<p class="h2">Education</p>

<span><strong>University of Zagreb, Faculty of Electrical Engineering and Computing, Croatia</strong></span>

<div class="experience-entry">
<span class="entry-date">Sep 2019. – Feb 2022.</span>
<span class="entry-description">
    <strong>Master's in Computer Science</strong><br />
    Thesis: <em>Extraction of Articles from News Portals Using Machine Learning</em>
</span>
</div>

<div class="experience-entry">
<span class="entry-date">Oct 2015. – Sep 2019.</span>
<span class="entry-description">
    <strong>Bachelor's in Computing</strong><br />
    Thesis: <em>Object Tracking Using Camera Mounted on Mobile Robot</em>
</span>
</div>

<br />
<p class="h2">Selected Projects</p>

<p class="h3">Personal Projects</p>

<div class="experience-entry">
<span class="entry-date">Mar 2023.</span>
<span class="entry-description">
    <a href="https://github.com/fkdosilovic/gpustatus"><strong>gpustatus - a simple CLI for collecting info about GPU</strong></a>
    <br /> A simple CLI, written in Go, for collecting info on GPUs from servers listed in .ssh/config.
</span>
</div>

<div class="experience-entry">
<span class="entry-date">Feb 2023.</span>
<span class="entry-description">
    <a href="https://github.com/fkdosilovic/jsonfmt"><strong>jsonfmt - a simple CLI formatting large JSON files</strong></a>
    <br /> A simple CLI, written in Nim, for formatting large JSON files.
</span>
</div>

<div class="experience-entry">
<span class="entry-date">Feb 2017. – May 2017.</span>
<span class="entry-description">
    <a href="https://github.com/fkdosilovic/molecule"><strong>molecule - a simple chess engine written in modern C++</strong></a>
    <br /> A simple chess engine written in modern C++. Engine features
    standard components, such as: a) move generation module, based on magic
    bitboards, b) alpha–beta pruning and quiescence search, c) Zobrist
    hashing for detecting identical states during search.
</span>
</div>

<p class="h3">University Projects</p>

<div class="experience-entry">
<span class="entry-date">Fall 2020.</span>
<span class="entry-description">
    <a href="https://finding-intuition.com/merging-statistically-similar-regions"><strong>Merging statistically similar regions</strong></a><br />
    Contemporary approaches for semantic segmentation use deep neural networks
    and achieve amazing results. Instead of using DNNs for segmenenting images,
    in this seminar I explore and expand on an algorithm for merging
    statistically similar regions, usually applied on grayscale images, applied
    on RGB images.
</span>
</div>
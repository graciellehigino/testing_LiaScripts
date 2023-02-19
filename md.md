<!--

author:   Gracielle Higino

email:    higino[at]zoology.ubc.ca

version:  0.0.1

language: en

narrator: US English Female

comment:  Intro to Data Science for biologists

script:   https://github.com/graciellehigino/testing_LiaScripts/blob/main/md.md
-->

# Syllabus

**Instructor:** Professor Gracielle Higino
**Office:** BRC ####
**Office hours:** 1:30-3:00pm, Tu/Th
**Email:** higino@zoology.ubc.ca
**Lecture:** 9:30-11:00am, Tu/Th



## 📜 Course Description

This course provides students an introduction to approaches for synthesizing complex datasets in biology research, including data collation, integration, analysis, and visualization. Additionally, students will learn best practices for data management in research, covering the entire data lifecycle from collection and storage to documentation and archiving. The course will develop students' skills in R programming, collaborative research, reproducible workflows, and data analysis, and will equip them with the tools to manage and rescue previously collected data. Through individual data management plans and group projects, students will apply these techniques to their own research.

The course will begin with an introduction to data science and its applications in biology, covering the basics of statistics and machine learning. Students will learn how to perform exploratory data analysis, use visualization tools to communicate data insights, and conduct hypothesis testing using inferential statistics. Throughout the course, students will be encouraged to engage in critical thinking and collaborative problem-solving as they learn how to apply data science techniques to real-world environmental and social science problems. In addition, the course will emphasize the importance of ethics in data science, including data privacy and data bias, and introduce students to open source tools such as R, Python, and Jupyter Notebook that promote collaboration and reproducibility in research.

## 🗝 Enrollment

**Prerequisite(s):** course(s) that must be taken prior to this course
**Co-Requisite(s):** course(s) that must be taken prior to or simultaneously
**Concurrent Enrollment:** course(s) that must be taken simultaneously
**Recommended Preparation**: course work or background that is advisable, not mandatory

## 📚 Readings

<aside>

</aside>

[Required Texts](https://www.notion.so/5ab1e8185af146f0bb125f6ceff4751c)

## Course Schedule

<aside>

</aside>


## 🏆 **Grading**

### Breakdown

Participation: **15%**
Response Papers: **15%**
Essay #1: **15%**
Midterm: **20%**
Essay #2: **15%**
Final Exam: **20%**

### **Scale**

**A** 90%-100%
**B** 80%-89%
**C** 70%-79%
**D** 60%-69%
**F** < 60%

### **Assignment Submission**

All essays and papers are due in lecture (due dates are listed on the schedule). No electronic copies will be accepted!

### **Late Assignments**

Late work will be deducted 5% per twenty-four hour period that elapses after the due date. If foreseen or unforeseen circumstances prevent you from completing an assignment on time, you may request an extension. Extensions must be requested in advance of the due date. If the situation warrants an extension, we will determine a new due date for the essay based on your individual circumstances.

## 😢 Plagiarism

Presenting someone else’s ideas as your own, either verbatim or recast in your own words – is a serious academic offense with serious consequences. Please familiarize yourself with the discussion of plagiarism in our campus policies.

## 🧠 Final Examination

The final examination will consist of an essay written during the exam period. You will receive the question at least one week before the test and may use a single page of notes during it.


# Getting to know the data
## Data types 

# Bias

## Overview

### Previous knowledge

- Data types
- [Basic data management with OpenRefine](https://datacarpentry.org/OpenRefine-ecology-lesson/)

### Previous reading

- Saini, A. (2017). *Inferior: How science got women wrong—and the new research that's rewriting the story.* Beacon Press. pg.
    
    [Inferior](https://books.google.ca/books?id=U5DEDgAAQBAJ&pg=PA74&source=gbs_toc_r&cad=4#v=onepage&q&f=false)
    

### Learning outcomes

- New concepts:
    - Sample
    - Population
    - Bias
- How sampling shapes distribution, that shapes hypotheses
- How bias has shaped the scientific agenda
    - Inferior
    - AI tech

## In class agenda

### Sampling and data distribution

- What is a population?
- What is a sample?
- Types of distribution
    - Compare the distributions of the population and the sample

### Bias

- Run video
- Why is missing data a problem?
    - confounding variables
- GBIF example
    - The Biodiversity Observation Networks
    - Taxonomic and social biases on biodiversity data
        - The open data value to diminish data biases in biodiversity
    - Consequences of biases to conservation, clinical studies, society
        - Miss the focus on the right species
        - Lead to erroneous medical advice
        - Wrong public policies
            - Any examples?
- Inferior discussions
    - Add Miro board for collaborative notes

## Homework

- Ask chatGPT <something>
- Submit a screenshot of your question and the response you’ve got
    - Be mindful of the environmental and social costs of using this tool, so think carefully about your prompt before using it.
- Submit a critical analysis of the chatGPT’s response


# Sampling and bias

Let's investigate what sampling and bias analysis look like! The code presented here is written in Julia. Click here for code in R, and here for code in Python.

First, let's create a fictional **population**. As you remember from class, a population is a *complete set of samples in a study group*. It's like you can assess every single tree in your neighbourhood's park and count how many individuals of each species are there. The total of trees individuals in your park is your population.

Notice that the word "population" here does not necessarily mean the same thing in Ecology. In Ecology, sometimes a population is not equal to the total of individuals in an area.

To start our calculations, let's set a "seed". A "seed" in code is the initial state of the (pseudo)random generator algorithm that allows us to get always the same results in random simulations (which guarantees the reproducibility of our calculations). Read more about this [here.](https://<https://r-coder.com/set-seed-r/)

```julia id=04a7ca8d-9a5e-4029-965f-3477515a8096
# Run this code chunk to make sure the functions and packages used here are of the same version.
using Pkg;
Pkg.activate();
Pkg.instantiate();
```

```julia id=08a6fb9b-007a-4976-971c-13bc6907bb29
using Random           # calls a package to manage the random number generator
Random.seed!(1234);    # sets the seed for our random number generator
```

Nice! Now we can create our population and start digging into what "bias" means. For the purpose of this lesson, let's create a random set of numbers that will describe our population. Think of them as, for example, the height measure for each tree in your park, or the length of the beak of each individual bird species you are studying. 

```julia id=3f412926-b089-43e1-917b-ffeba3c0072d
our_population = randn(10^3);            # generate a normal distribution of 10ˆ3 random numbers

# Plotting our population
using Plots; using Statistics;           # Load packages
bins = collect(0:0.1:1)                  # Create categories to help us visualize variation
h1 = histogram(vec(our_population),      # Create a histogram for our_population...            
     color=:pink, linecolor=:match,      # ... make it pink!...
     label="Population")                 # ... and add a label to our plot.

vline!([mean(our_population)],           # Add a line where the mean of our population is...
  linewidth=5,                           # ... tweak the vertical line to be thicker...
  linecolor="#0ABAB5",                   # ... make it tiffany!...
  label = "mean")                        # ... and add the label "mean".
```

![result][nextjournal#output#3f412926-b089-43e1-917b-ffeba3c0072d#result]

Ok, so our population can be described in terms of its mean (the tiffany line). Other metrics are:

```julia id=82701aae-1a48-4889-8dd6-c65cd943b0b3
using StatsBase
summarystats(our_population)
```

Now let's suppose we are conducting a study on this population and we need to measure a sample of it. But we didn't really pay attention that some of **these individuals are closer together for a very specific reason**, and **we could only sample 10% of our population**. So we ended up with samples that looked like this:

```julia id=b52ca03b-197b-4f70-861b-cd2f7ac67b22
our_sample = [x for x in our_population if x > mean(our_population)][1:100];
summarystats(our_sample)
```

```julia id=2c69e0bf-3e37-4d52-a6d4-65e7df983ff9
# Plotting our sample

histogram(vec(our_sample),               # Create a histogram for our_population...                 
  color="#0ABAB5",linecolor=:match,      # ... make it tiffany!...
  label="Sample",                        # ... add a label to our plot.
  xlims=xlims(h1),                       # Determine the limits of the X axis...
  ylims=ylims(h1))                       # ... and the limits of the Y axis.

vline!([mean(our_sample)],               # Add a line where the mean of our population is...
  linewidth=5,                           # ...tweak the vertical line to be thicker...
  linecolor=:pink,                       # ... make it pink!...
  label = "mean")                        # ... and add the label "mean".
```

![result][nextjournal#output#2c69e0bf-3e37-4d52-a6d4-65e7df983ff9#result]

Do you think our small sample represents our population?

[nextjournal#output#3f412926-b089-43e1-917b-ffeba3c0072d#result]:
<https://nextjournal.com/data/QmbMZHDRHG3gmTHczbd6DJFktgsetX2HKg6Ty8sjb6syeN?content-type=image/svg%2Bxml&node-id=3f412926-b089-43e1-917b-ffeba3c0072d&node-kind=output>

[nextjournal#output#2c69e0bf-3e37-4d52-a6d4-65e7df983ff9#result]:
<https://nextjournal.com/data/QmX2pC56wQW3fd8vyceFGqjU7XKMwsgvjiYjL9CEk38c8q?content-type=image/svg%2Bxml&node-id=2c69e0bf-3e37-4d52-a6d4-65e7df983ff9&node-kind=output>

<details id="com.nextjournal.article">
<summary>This notebook was exported from <a href="https://nextjournal.com/a/RGCZ2SRh5homKpXHxg1d2?change-id=DLown5SixpEj9JqH3L1KQu">https://nextjournal.com/a/RGCZ2SRh5homKpXHxg1d2?change-id=DLown5SixpEj9JqH3L1KQu</a></summary>

```edn nextjournal-metadata
{:article
 {:nodes
  {"04a7ca8d-9a5e-4029-965f-3477515a8096"
   {:compute-ref #uuid "4034c8b5-c705-4b5a-b809-5b8863a28e68",
    :exec-duration 292,
    :id "04a7ca8d-9a5e-4029-965f-3477515a8096",
    :kind "code",
    :output-log-lines {:stdout 2},
    :runtime [:runtime "84c4a017-c813-4321-a6c6-54fe6fd9bcce"]},
   "08a6fb9b-007a-4976-971c-13bc6907bb29"
   {:compute-ref #uuid "9b42d25a-cf32-4204-9f26-8bbfe1567300",
    :exec-duration 128,
    :id "08a6fb9b-007a-4976-971c-13bc6907bb29",
    :kind "code",
    :output-log-lines {},
    :runtime [:runtime "84c4a017-c813-4321-a6c6-54fe6fd9bcce"]},
   "2c69e0bf-3e37-4d52-a6d4-65e7df983ff9"
   {:compute-ref #uuid "63e0489c-45a1-4237-8ad4-ca202358431f",
    :exec-duration 575,
    :id "2c69e0bf-3e37-4d52-a6d4-65e7df983ff9",
    :kind "code",
    :output-log-lines {},
    :runtime [:runtime "84c4a017-c813-4321-a6c6-54fe6fd9bcce"]},
   "3f412926-b089-43e1-917b-ffeba3c0072d"
   {:compute-ref #uuid "7d934cc2-0cf8-4121-84f6-b6f221e825fd",
    :exec-duration 535,
    :id "3f412926-b089-43e1-917b-ffeba3c0072d",
    :kind "code",
    :output-log-lines {},
    :runtime [:runtime "84c4a017-c813-4321-a6c6-54fe6fd9bcce"]},
   "82701aae-1a48-4889-8dd6-c65cd943b0b3"
   {:compute-ref #uuid "9bf6f954-8338-4c6d-9128-0c1b932c224d",
    :exec-duration 535,
    :id "82701aae-1a48-4889-8dd6-c65cd943b0b3",
    :kind "code",
    :output-log-lines {},
    :runtime [:runtime "84c4a017-c813-4321-a6c6-54fe6fd9bcce"]},
   "84c4a017-c813-4321-a6c6-54fe6fd9bcce"
   {:environment
    [:environment
     {:article/nextjournal.id
      #uuid "5b460d39-8c57-43a6-8b13-e217642b0146",
      :change/nextjournal.id
      #uuid "5fa43e2c-530b-4260-aab2-857bac98e540",
      :node/id "39e3f06d-60bf-4003-ae1a-62e835085aef"}],
    :id "84c4a017-c813-4321-a6c6-54fe6fd9bcce",
    :kind "runtime",
    :language "julia",
    :resources {:machine-type "n1-standard-2"},
    :type :nextjournal},
   "b52ca03b-197b-4f70-861b-cd2f7ac67b22"
   {:compute-ref #uuid "f81ad790-cc6b-4eef-9937-9e7939ac4d7d",
    :exec-duration 249,
    :id "b52ca03b-197b-4f70-861b-cd2f7ac67b22",
    :kind "code",
    :output-log-lines {},
    :runtime [:runtime "84c4a017-c813-4321-a6c6-54fe6fd9bcce"]}},
  :nextjournal/id #uuid "036337a1-4324-4995-9b2e-0389aca537b1",
  :article/change
  {:nextjournal/id #uuid "63f1b9e0-badf-4252-b945-5940d85b1262"}}}

```
</details>

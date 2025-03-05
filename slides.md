---
# You can also start simply with 'default'
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Using Stryker mutator
info: |
  ## Are 100% test coverage good enough?
  How to make your unit or component tests bulletproof!

# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
lineNumbers: true
---

# Using Stryker Mutator

## Make your tests bullet proof!

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
layout: default
transition: slide-up
---

# Sten Johnsen

A tech geek spending his work and spare time figuring out stuff involving electronics and software


- **Bouvet** - Since 2008, currently team-lead and (full-stack) developer
![Sten](/images/Sten_Johnsen_6879.jpeg) {width=200px margin=30px align=right}

- **Experience** - Graduated 1991 - B.Eng Microelectronic computer systems. Programming since my first real job - never looked back.
  
- **Roles** - Programmer, project manager, program manager, department head, entrepreneur, agile coach and relationship counsellor

- **Trainer** - DevOps certification courses, Agile, Scrum

- **Busy with** - Quality of software and creating high performing teams

<!--
15+ years in Bouvet, 
Full stack developer, 
teamlead and 
DevOps trainer
-->

---
layout: default
transition: slide-up
---


# Erlend Lunde

A geek who loves spending his spare time in front of a computer or immersed in a good fantasy book.

- **Bouvet** - Joined in 2023 as a full-stack developer for Team Epilogue
![Erlend](/images/Erlend_Lunde.jpg) {width=200px margin=30px align=right}

- **Experience** - Graduated in winter 2022 with a Bachelor's degree in Digital Culture. Started at Bouvet fresh out of the classroom.
  
- **Roles** - Programmer, bug squasher

- **Busy with** - Writing testable code, and writing tests




---
layout: image-right
image: /images/DORA-action.png
backgroundSize: contain
---

# devops-metrics-action

<v-click>

A gihub action that calculates DORA key metrics based
on Issues, PR's and releaseses in one or more github
repositories.

</v-click>

<v-clicks>

- implemented in typescript<br/>
- 100% test coverage<br/>

</v-clicks>

<v-after>

![test coverage](/images/test-coverage.png)

</v-after>

<!--
Consider this GitHub action - a plugin for calculating DORA metrics based on the issues, releases and commits to one or more repositories

[click] Is entirely coded in Typescript
100% test coverage. Not only on 

- lines, but also [click] 
- statements, [click] 
- functons and [click] 
- branches [click] 

-->

---
layout: image-right
image: /images/LeadTimeTest.png
---

# LeadTime

Lets consider the LeadTime module,
having a automated component tests
in `LeadTime.test.ts` test suite.

<v-clicks>

- more than 740 lines of code<br/>
- (probably too big)<br/>
- 19 tests<br/>
- Provides 100% test coverage<br/>
![LeadTime test log](/images/LeadTime-test-log.png)

</v-clicks>

<v-click>

How do we know these tests are good?<br/>
(And by good we mean: able to detect bugs we introduce)

</v-click>

<!--
The leadTime module calculates the Lead time for changes by the team. Time is an average for changes over the last two months, calculated as the time between the first commit on the issue until the change is in production.

The LeadTime module has automated tests implemented in LeadTime.test.ts

As like the overall program, this test suite also provide 100% test coverage.

But are we sure the tests are reliable and will let us know if we later on introduce bugs in the code?
-->

---

# LeadTime.ts

Lets test our tests to see how waterthight our testing is by considering a few lines of code in the LeadTime module:<br/>
> How many bugs can we introduce and still have all tests passing?

<v-click>

````md magic-move {lines: true}
```ts {*|6-10}{lines:true,startLine:2}
// code
  getLog(): string[] {
    return this.log
  }
  async getLeadTime(filtered = false): Promise<number> {
    if (this.pulls.length === 0 || this.releases.length === 0) {
      return 0
    }

    if (filtered) {
      this.log.push(`\nLog is filtered - only feat and fix.`)
    }
```

<Arrow x1="10" y1="20" x2="100" y2="200" />

```ts {6}
// bug 1
  getLog(): string[] {
    return this.log
  }
  async getLeadTime(filtered = false): Promise<number> {
    if (false) {
      return 0
    }

    if (filtered) {
      this.log.push(`\nLog is filtered - only feat and fix.`)
    }
```

```ts {6}
// bug 2
  getLog(): string[] {
    return this.log
  }
  async getLeadTime(filtered = false): Promise<number> {
    if (false || this.releases.length === 0) {
      return 0
    }

    if (filtered) {
      this.log.push(`\nLog is filtered - only feat and fix.`)
    }
```

```ts {6}
// bug 3
  getLog(): string[] {
    return this.log
  }
  async getLeadTime(filtered = false): Promise<number> {
    if (this.pulls.length === 0 && this.releases.length === 0) {
      return 0
    }

    if (filtered) {
      this.log.push(`\nLog is filtered - only feat and fix.`)
    }
```

```ts {6}
// bug 4
  getLog(): string[] {
    return this.log
  }
  async getLeadTime(filtered = false): Promise<number> {
    if (this.pulls.length === 0 || false) {
      return 0
    }

    if (filtered) {
      this.log.push(`\nLog is filtered - only feat and fix.`)
    }
```

```ts {6}
// bug 5
  getLog(): string[] {
    return this.log
  }
  async getLeadTime(filtered = false): Promise<number> {
    if (this.pulls.length === 0 || this.releases.length === 0) {}

    if (filtered) {
      this.log.push(`\nLog is filtered - only feat and fix.`)
    }
```

```ts {10}
// bug 6
  getLog(): string[] {
    return this.log
  }
  async getLeadTime(filtered = false): Promise<number> {
    if (this.pulls.length === 0 || this.releases.length === 0) {
      return 0
    }

    if (true) {
      this.log.push(`\nLog is filtered - only feat and fix.`)
    }
```

```ts {10}
// bug 7
  getLog(): string[] {
    return this.log
  }
  async getLeadTime(filtered = false): Promise<number> {
    if (this.pulls.length === 0 || this.releases.length === 0) {
      return 0
    }

    if (false) {
      this.log.push(`\nLog is filtered - only feat and fix.`)
    }
```

```ts {10}
// bug 8
  getLog(): string[] {
    return this.log
  }
  async getLeadTime(filtered = false): Promise<number> {
    if (this.pulls.length === 0 || this.releases.length === 0) {
      return 0
    }

    if (filtered) {}
```

```ts {*}
// bug 8
  getLog(): string[] {
    return this.log
  }
  async getLeadTime(filtered = false): Promise<number> {
    if (this.pulls.length === 0 || this.releases.length === 0) {
      return 0
    }

    if (filtered) {
      this.log.push(`\nLog is filtered - only feat and fix.`)
    }
```

````
</v-click>

<v-click>

- 8 possible bugs in 6 lines of code

</v-click>

<v-click>

> ## Full test coverage does **not** ensure high code quality

</v-click>

<!--
Let's analyze a small part of the code of the module production code to see if we can introduce bugs that our tests does not detect.

This is a small fraction of the production code,

Lets consider only 5-6 lines of the code and start introduce changes.

- The whole if condition might be inadvertently evaluate to false always
- The first half of the condition might be false
- Behaviour might change to AND from OR
- The last half might be false
- The whole block might be removed
- The second if condition might be set to true
- or to false
- or the whole block might be removed.

All of these changes are not caught by any of the tests covering the code.
-->

---

# Test quality

In the previous example, there were also 4 bugs that caused a test to fail:<br/>

<br/>

$$
\begin{aligned}
\frac{4 \cdot detected}{4 \cdot detected + 8 \cdot undetected} &= \frac{4}{12} &= 0,33 &= 33\%
\end{aligned}
$$

<br/>

<v-click>
A few takeaways:
</v-click>

<v-clicks>

- Test coverage alone is not good enough, although

- Tests are not useless!

- Test quality can be better, but also:

- No test coverage = no tests at all
</v-clicks>

<!--
Turns out that there are 4 changes that was detected by our tests - that we did not cover now.
-->

---
layout: image-right
image: images/test-code.png
---

# But Why?

<v-click>
Doing this exercise enables us to:
</v-click>

<v-clicks>

- Find functionality we have not tested
- Detect useless code
- Get  hints to write better tests
- Make our code more robust

</v-clicks>

<br/>

<v-click>

> In this particular case I can add tests to verify edge cases the code is meant to catch, or I could delete some of the code if it turns out it's useless.<br/>
</v-click>

<v-click>
BTW: The overall score for the LeadTime module tests is 67%. <br/><br/>
How do I know?
</v-click>

<!--
So, why should we do this exercise?

We can detect functionality we have not yet good test for. 
(typically a symptom of writing tests when the production code is already done)

There might be parts of the code we really don't need if everything works in spite of inserted changes.

Point to areas where we might improve the tests

Increase the robustness of the code by improving tests so that we feel safer when changing it later
-->

---
layout: image-left
image: images/stryker-man.png
---

# Stryker-mutator

Stryker is an automated way of testing your tests through altering your code by inserting bugs (mutants) and then running your tests to verify that the tests are catching the bugs or not.

<v-clicks>

- open source
- no licenses
- does not leak your code externally
- available for:
  JS/TS, C# and Scala
- produces kill-score for your tests
- all in a handy report

</v-clicks>

<v-after>

![Stryker results](/images/stryker-scores.png)

</v-after>

---

# How Stryker Works

<v-clicks>

- Filters the files to be mutated
  
- Determines what mutations to insert
- Performs a dry-run of all tests to verify all is passing
- Makes a copy of your code, applies one mutation and runs all tests
- Counts the status of the tests for each mutation
- Creates a report based on all mutations

</v-clicks>

---
layout: two-cols-header
class: gap-x-12

---

# Practical use in other projects

Apart from running Stryker from command line, there might also be a benefit from running stryker in a github action. <br/>

::left::

## Nightly

Implemented nightly Stryker test of all code-base,<br/>
storing the report available to the team.

<v-clicks>

- Track total score over time
- Use changes in total score as input to<br/>
  retrospectives

<div class="flex gap-12">
  <div class="w-9/10">

```yaml
name: Stryker mutation tests frontend

on:
  workflow_dispatch:
  schedule:
    - cron: "30 0 * * 1,2,3,4,5"
      
jobs:
...
```
  </div>
</div>

</v-clicks>

::right::

## Pull Requests

Run Stryker on changed code as part of the validation<br/>
checks in pull-requests.

<v-clicks>

- Make it quick for immediate feedback to developers
- Discuss within the team if there should be a minimum score

```yaml
name: Stryker frontend PR diff

on:
  workflow_dispatch:
  push:
    branches:
      -'*'
      -'!main'  
  pull_request:
    paths:
      - 'frontend/src/**/*.tsx'
      - 'frontend/src/**/*.ts'
      
jobs:
...
```
</v-clicks>

---

# Caveats

Stryker, in its nature, runs your test suites for every mutation it decides is relevant. This means that for large codebases a full report takes a long time to complete.

Stryker has ways to reduce time spent:
<v-clicks>

- Incremental mode: Only run mutations changed
- Pre-check code for compilation or build errors
- Option to ignore static mutations
- Ignores mutations in uncovered code
- Run stryker on part of the code only (lines changed)

</v-clicks>
---
layout: two-cols
---

# Our Experience
### Report on pull request
![GitReport](/images/Report_from_git.png) {width=430px margin=30px align=left style="filter: brightness(150%);"}

#### Get feedback on test quality where you are working 
::right::

<br>

<v-clicks>

## What do we do with this information?

<v-click>

- **Drop everything to improve score?** No
</v-click>
<v-click>

- **Act on feedback from new tests**: How good are the new tests?
</v-click>
<v-click>

- **Get information**: How good are the test testing things touched by your new fancy experimental feature?
</v-click>
<v-click>

- **Challenges Faced**: As with any theology added to a project it must be maintained
</v-click>
<v-click>

- **What we get**: A well of information about our tests from little effort (once stryker is set up)
</v-click>

</v-clicks>


---
layout: default
---

<iframe src="mutation.html" style="zoom: 0.5; width: 125%; height: 125%; border: none;"></iframe>

---

# How to get high stryker score?

<v-clicks>

- Adopt a culture of Quality First
- Arrange for fail-fast and fast feedback
- Focus on Stryker score in peer-reviews
- Do TDD!

</v-clicks>

---
layout: image-right
image: images/tdd-img.png
backgroundSize: 20em

---

# TDD

Some other benefits from TDD:
<v-clicks>

- Trustworthy code
- Better architecture
- Tests are the documentation
- Easy when debugging<br/>

and as an extra benefit:
- Guarantees test coverage and high stryker score

</v-clicks>

<v-after>

```
|------------------------|---------|----------|---------|---------|-------------------|
|File                    | % Stmts | % Branch | % Funcs | % Lines | Uncovered Line #s |
|------------------------|---------|----------|---------|---------|-------------------|
|All files               |     100 |      100 |     100 |     100 |                   |
| ChangeFailureRate.ts   |     100 |      100 |     100 |     100 |                   |
| CommitsAdapter.ts      |     100 |      100 |     100 |     100 |                   |
| DeployFrequency.ts     |     100 |      100 |     100 |     100 |                   |
| IssuesAdapter.ts       |     100 |      100 |     100 |     100 |                   |
| LeadTime.ts            |     100 |      100 |     100 |     100 |                   |
| MeanTimeToRestore.ts   |     100 |      100 |     100 |     100 |                   |
| PullRequestsAdapter.ts |     100 |      100 |     100 |     100 |                   |
| ReleaseAdapter.ts      |     100 |      100 |     100 |     100 |                   |
| index.ts               |     100 |      100 |     100 |     100 |                   |
|------------------------|---------|----------|---------|---------|-------------------|
Test Suites: 13 passed, 13 total
Tests:       93 passed, 93 total
Snapshots:   0 total
Time:        6.607 s
Ran all test suites.
```

</v-after>


---
layout: full

---

# References

- Stryker Mutator (web): `https://stryker-mutator.io/`
- Test-Driven Development: `https://docs.google.com/presentation/d/1L5yPzxBlDJ5r56OHpUv1BLO4pkxFYT9V1yLUgosThq4/edit?usp=sharing`
  
![Book](/images/book-clipart-lg.png) {width=300px margin=30px}



---
layout: fact

---

# Thank you!




## ![Presentation](/images/QR.png) {width=150px margin=30px align=right}

---
layout: full
hidden: true

---

# What could go wrong?
    
    line 3: Covered by 18 tests<br/>
    line 4: Covered by 6 tests<br/>
     
````md magic-move {lines: true}
```ts {*|3|*}
// code
  async getLeadTime(filtered = false): Promise<number> {
    if (this.pulls.length === 0 || this.releases.length === 0) {
      return 0
    }
```

```ts {*|3|*}
// bug 1
  async getLeadTime(filtered = false): Promise<number> {
    if (this.pulls.length === 0 && this.releases.length === 0) {
      return 0
    }
```

```ts {*|3|*}
// bug 2
  async getLeadTime(filtered = false): Promise<number> {
    if (false || this.releases.length === 0) {
      return 0
    }
```

```ts {*|3|*}
// bug 3
  async getLeadTime(filtered = false): Promise<number> {
    if (this.pulls.length === 0 || true === 0) {
      return 0
    }
```

```ts {*|3|*}
// bug 4
  async getLeadTime(filtered = false): Promise<number> {
    if (this.pulls.length === 0 || this.releases.length === 0) {}
```

````

---
hide: true
---


````md magic-move {lines: true}
```ts {*|2|*}
// step 1
const author = reactive({
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery'
  ]
})
```

```ts {*|1-2|3-4|3-4,8}
// step 2
export default {
  data() {
    return {
      author: {
        name: 'John Doe',
        books: [
          'Vue 2 - Advanced Guide',
          'Vue 3 - Basic Guide',
          'Vue 4 - The Mystery'
        ]
      }
    }
  }
}
```

```ts
// step 3
export default {
  data: () => ({
    author: {
      name: 'John Doe',
      books: [
        'Vue 2 - Advanced Guide',
        'Vue 3 - Basic Guide',
        'Vue 4 - The Mystery'
      ]
    }
  })
}
```

Non-code blocks are ignored.

```vue
<!-- step 4 -->
<script setup>
const author = {
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery'
  ]
}
</script>
```
````


---
transition: slide-up
level: 2
hide: true

---

# Testing software

How do we know our stuff works?
Automated tests

- importance
- benefits
- metrics - test coverage
- Component/Unit tests
- Regression tests
- User Acceptance tests
- End-to-end test

---
hide: true
---

# Test quality

How do we know our tests are good?



---
hide: true
---

# Implementing in project/product development

- Using locally 
- In pull-requests
- For review
- As a quality check



---
hide: true
---

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/features/slide-scope-style
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Here is another comment.
-->

---
transition: slide-up
level: 2
hide: true
---

---
hide: true
---

# Testing software

How do we know our stuff works?
Automated tests

- importance
- benefits
- metrics - test coverage

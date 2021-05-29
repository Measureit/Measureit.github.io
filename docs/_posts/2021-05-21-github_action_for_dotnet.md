---
layout: post
title: "Github actions for .net"
subtitle: "Automate your software process with Github"
date: 2021-05-21 10:45:13 -0400
background: "/img/posts/devops/devops.jpg"
---

<h2 class="section-heading">Motivation</h2>
<p>Few month ago I decieded to publicy relase my <b color=red>dotnet library</b> by using Github actions. So far my all private projects was hosted in GitLab and it's time to use something else. </p>

<p>I always try touch few different solutions to grab some experience and grow as a software developer. This approach help changing the perspective during solving problems and increases decision-making.</p>

<p>To keep a concise structure of this blog I would like to treat it as a story in well known scrum methodology.</p>

<h2 class="section-heading">Acceptance criteria</h2>
<p>Process steps to automated:</p>

<b><input type="checkbox" disabled checked /> build </b>

<b><input type="checkbox" disabled checked /> test</b>

<b><input type="checkbox" disabled checked /> analyze source quality</b>

<b><input type="checkbox" disabled checked /> set version</b>

<b><input type="checkbox" disabled checked /> publish nugets</b>

<b><input type="checkbox" disabled checked /> generate release notes</b>

<h2 class="section-heading">Files</h2>

<p>I would like to split my automation process for 3 main blocks. Blocks definition will be put into separate files in directory .github\workflows\ </p>

*   **continouse-integration.yaml**
    > Trigger: push on master or develop
    >
    > Motivation: build and test source code
*   **code-analysis.yaml**
    > Trigger: cron job on master
    >
    > Motivation: generate code quality metrics
*   **publish.yaml**
    > Trigger: push with tag containing version number
    >
    > Motivation: publish nuget packages



<h2 class="section-heading">Build</h2>

{% include code-header.html %}
```bash
name: CI
on:
  push:
    branches:
      - develop
      - master
    paths-ignore:
      - '**.md'
  pull_request:
    branches:
      - develop
      - master
    paths-ignore:
      - '**.md'
jobs:
  build:
    runs-on: ubuntu-latest
    env:
      working-directory: ./src
      artifact-directory: ./bin
    timeout-minutes: 15
    steps:
    - name: Checkout
      uses: actions/checkout@v2
    - name: Build
      run: dotnet build --configuration Release
      working-directory: {% raw %} ${{env.working-directory}} {% endraw %}
    - name: Test
      run: dotnet test --configuration Release --no-build
      working-directory: {% raw %} ${{env.working-directory}} {% endraw %}
```

<h2 class="section-heading">Summary</h2>


<img class="img-fluid" src="https://source.unsplash.com/Mn9Fa_wQH-M/800x450" alt="Demo Image">
<span class="caption text-muted">...</span>

<p>Photographs <a href="http://www.freepik.com">designed by upklyak / Freepik</a>.</p>

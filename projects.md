---
layout: page
title: Projects
permalink: /projects/
---
  <head>
    <style>
      main {
        /* display: grid; 
        grid-auto-rows: auto; 
        grid-gap: 1em;  */
        padding: 1em 0;
        font-size: 95%;
        /* line-height: 1.5em; */
        font-family: 'Helvetica', 'Arial', sans-serif;
      }
      strong {
        color: blue;
      }
      p {
        margin: 0;
      }
      li {
        list-style-type: square;
      }
      h1, h2 {
        margin: 0;
      }
      ul {
        padding: 0 1em; 
        margin: 0.2em;
      }
      a {
        text-decoration: none;
      }
      code {
        background: none;
      }
      .address {
        display: flex;
        /* border-bottom: 2px solid #d9d9d9; */
        padding-bottom: 1em;
        justify-content: space-between; 
      }
      .linkIcons {
        align-self: end;
      }
      .mainSectionTitle {
        border-bottom: 1px solid;
        font-size: 105%;
      }
      .titleAndDate {
        display: flex;
        justify-content: space-between;
        flex-wrap: wrap;
      }
      .multirow {
        display: grid;
        grid-auto-rows: auto;
      }
      .multirow > :nth-child(n+2) {
        padding-top: 1em;
      }
      .experience > :nth-child(n+2) {
        padding-top: 1em;
      }
      .languageLists {
        width: 50%;
        display: flex;
        justify-content: space-between;
      }
      .multirowMulticolumn {
        display: grid; 
        grid-auto-rows: auto;
      }
      .multirowMulticolumn > :nth-child(n+3) {
        padding-top: 1em;
      }
      .projectLink {
        font-weight: bold;
        color: #333;
        border-bottom: 3px solid #4183C4;
        display: inline-block;
        line-height: 0.8;
      }
      .titleBold {
        font-weight: bold;
      }
      .italic {
        font-style: italic;
        text-align: left;
      }
      .separator {
        background-color: white;
        height: 15px;
      }
      @media only screen and (min-width: 850px)  {
        main {
          grid-template-columns: 14% auto;
          grid-column-gap: 5%;
        }
        .mainSectionTitle {
          border-bottom: none;
        }
        .flexList {
          display: flex;
          flex-direction: row; 
          justify-content: space-between;
        }
        .toolBox {
          display: flex; 
          flex-direction: row; 
          justify-content: space-between;
        }
        .toolBoxItem {
          width: 190px;
          margin: 0;
        }
        .separator {
          background-color: black;
          width: 2px;
          height: auto;
        }
      }
    </style>
  </head>
  <div>
    <!-- <section class="address">
      <h2></h2>
      <div class="linkIcons">
        <a href="https://github.com/a-bishop" ><i class="fab fa-github-square"></i></a>&nbsp;
        <a href="https://www.linkedin.com/in/andrew-n-bishop/" ><i class="fab fa-linkedin"></i></a>&nbsp;
      </div>
    </section> -->
    <main>
        <section>
            <a href="https://movie-recommendations.netlify.com" class="projectLink">Movie Recommendations</a>
            <p class="italic">Tech: React, Firebase, Styled Components &nbsp;<a href="https://github.com/a-bishop/movies-to-watch"><i class="fab fa-github-square gitHubLink"></i></a></p>
            <p>An app to save and share movie recommendations, using Firebase, React and the Open Movie Database API.</p>
            <br>
            <a href="https://sandbox.abishop.me" class="projectLink">Outdraw</a>
            <p class="italic">Tech: Feathers.js, HTML5 Canvas, Web Sockets &nbsp;<a href="https://github.com/a-bishop/feathers-draw"><i class="fab fa-github-square gitHubLink"></i></a></p>
            <p>A real-time collaborative drawing app, using HTML5 Canvas, Feathers.js and neDB, a file-based data store modeled on MongoDB.</p>
            <br>
            <a href="https://andrewnbishop.com/drum-machine" class="projectLink">Tone.js Drum Machine</a>
            <p class="italic">Tech: Tone.js, Web Components &nbsp;<a href="https://github.com/a-bishop/drum-machine"><i class="fab fa-github-square gitHubLink"></i></a></p>
            <p>A drum machine and arpeggiator built with the Tone.js library and lit-element, a base class for creating lightweight Web Components. Demo works best in the Chrome browser.</p>
            <br>
            <a href="https://andrewnbishop.com/population-stats" class="projectLink">Population Stats</a>
            <p class="italic">Tech: React, D3 &nbsp;<a href="https://github.com/a-bishop/population-stats"><i class="fab fa-github-square gitHubLink"></i></a></p>
            <p>A React single page application that allows users to compare up to six country populations in a bar graph. Uses the D3 library.</p>
            <br>
            <a href="https://github.com/a-bishop/podBlast" class="projectLink">Podblast</a>
            <p class="italic">Tech: Swift, XCode, ListenNotes API &nbsp;<a href="https://github.com/a-bishop/podBlast"><i class="fab fa-github-square gitHubLink"></i></a></p>
            <p>An iOS podcast search and recommendation app, consuming the ListenNotes API and featuring infinite scroll. Search by topic, podcast name, or by genre, and save favourite podcasts for later.</p>
            <br>
            <a href="https://github.com/a-bishop/battlesnake-samuel" class="projectLink" >Battlesnake</a>
            <p class="italic">Tech: JavaScript, Node, Express, lodash &nbsp;</p>
            <p>A snake AI written in Node to compete in the intermediate category at Battlesnake, a programming competition.</p>
            <br>
            <a href="https://js-messageboard.herokuapp.com/" class="projectLink" >JS Messageboard</a>
            <p class="italic">Tech: Node, React, MongoDB, Webpack, Bootstrap &nbsp;<a href="https://github.com/a-bishop/js-msgboard"><i class="fab fa-github-square gitHubLink"></i></a></p>
            <p>Isomorphic JavaScript messageboard application with a responsive front-end and custom authentication and authorization scheme. Built for Camosun program's second-year final Web Services project.</p>
            <br>
            <a href="https://github.com/a-bishop/micks-licks" class="projectLink" >Mick's Licks</a>
            <p class="italic">Tech: JavaScript, HTML, CSS, PHP, MySQL, Bootstrap, GitLab &nbsp;</p>
            <p>An e-commerce web application built with the LAMP stack to sell used vinyl records, with login verification, a cart system and online payment integration. Created with a team of three other developers as final project for Web Applications class.</p>
        </section>

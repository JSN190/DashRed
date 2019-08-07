<!--
Copyright (C) 2018 John Su

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as published
by the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License
along with this program.  If not, see https://www.gnu.org/licenses.
-->
<template>
  <div id="app" class="wrapper">
    <main class="main-container">
      <div class="main-group-1">
        <div class="main-logo-tag-group zoomIn animate">
          <div class="main-logo">
            Dash<span class="reddit-orange">Red</span>
          </div>
          <div class="main-tag-line">Get direct, proper shareable links to your reddit videos.</div>
        </div>
        <div id="main-input-container" class="bounceInUp animate">
          <div>
            <input
              id="main-input-url"
              v-model="inputUrl"
              placeholder="Type or paste your reddit link here..."
              @keyup.enter="submit"
              autofocus
              spellcheck="false"
            />
          </div>
          <div>
            <div id="main-input-submit" v-on:click="submit">
              <i class="fas fa-arrow-right"></i>
            </div>
          </div>
        </div>
      </div>
    </main>
    <footer class="fadeIn slow animate">
      <div>
        Released under
        <a href="https://github.com/Tyncture/DashRed">
          <i class="fab fa-osi"></i> GNU AGPL v3.0
        </a>.
        Created by
        <a href="https://github.com/Tyncture/DashRed">
          <i class="fab fa-github"></i> Tyncture
        </a>.
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  name: "DashRed",
  data: function() {
    return {
      inputUrl: ""
    };
  },
  methods: {
    animatePreSubmit: function() {
      const inputContainer = document.getElementById("main-input-container");
      const submitButton = document.getElementById("main-input-submit");
      submitButton.classList.add("main-input-submitting");
      inputContainer.classList.remove("bounceIn");
      inputContainer.classList.remove("bounceInUp");
      inputContainer.classList.remove("animated");
    },
    animatePostSubmit: function() {
      const inputContainer = document.getElementById("main-input-container");
      const submitButton = document.getElementById("main-input-submit");
      submitButton.classList.remove("main-input-submitting");
      inputContainer.classList.add("bounceIn");
      inputContainer.classList.add("animated");
    },
    submit: function() {
      this.animatePreSubmit();
      const regexRedditLink = /https:\/\/(www|old).reddit.com\/r\/[[a-zA-Z0-9]+\/comments\/[a-zA-Z0-9]+\/[a-zA-Z0-9_-]+/;
      const matched = this.inputUrl.trim().match(regexRedditLink)
        ? this.inputUrl.match(regexRedditLink)[0]
        : null;
      fetch(`${matched}.json`)
        .then(response => response.json())
        .then(
          data =>
            (this.inputUrl =
              data[0].data.children[0].data.media.reddit_video.fallback_url)
        )
        .finally(() => this.animatePostSubmit());
    }
  },
  mounted: function() {
    // Animate after page load to prevent page reload glitches
    const animated = document.getElementsByClassName("animate");
    setTimeout(
      () =>
        Array.from(animated).forEach(element =>
          element.classList.add("animated")
        ),
      1
    );
  }
};
</script>

<style lang="scss">
@import url("https://cdnjs.cloudflare.com/ajax/libs/normalize/8.0.0/normalize.min.css");
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.0/animate.min.css");
@import url("https://fonts.googleapis.com/css?family=Dosis:400,500+Arimo:400,700");
@import url("https://use.fontawesome.com/releases/v5.3.1/css/all.css");

* {
  font-family: Arimo, Arial, Helvetica, sans-serif;
  box-sizing: border-box;
}

.wrapper {
  height: 100vh;
  width: 100vw;
  background-color: #f7f7f7;
  display: flex;
  justify-content: center;
  align-items: center;
}

.main-container {
  width: 80vw;
  height: 500px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.main-group-1 {
  display: grid;
  grid-auto-flow: row;
  grid-gap: 20px;
  justify-items: center;
}

.main-logo-tag-group {
  display: grid;
  grid-auto-flow: row;
  grid-gap: 5px;
  justify-items: center;
}

.main-logo {
  font-size: 100px;
  line-height: 100px;
  font-family: Arimo, Arial, sans-serif;
  letter-spacing: -2px;
  font-weight: 500;
  color: #666666;
  text-align: center;
}

.main-tag-line {
  color: #8a8a8a;
  font-weight: 600;
  text-align: center;
}

#main-input-container {
  width: 550px;
  display: flex;
  align-items: center;
  border-radius: 1px;
  border: 0.25px solid #e8e8e8;
  box-shadow: 1px 1px 10px #e8e8e8;
  background-color: rgb(255, 254, 254);
  :first-child {
    flex-grow: 1;
  }
}

#main-input-url {
  display: flex;
  flex-direction: column;
  justify-content: center;
  color: #484848;
  font-size: 20px;
  width: 100%;
  border: none;
  height: 100%;
  padding: 8px 0px 8px 8px;
  &:active,
  &:focus {
    outline: none;
  }

  &::placeholder {
    color: #9c9c9c;
  }
}

#main-input-submit {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  background-color: rgb(250, 109, 109);
  color: white;
  border-radius: 100% !important;
  margin: 5px;
  font-size: 15px;
  font-weight: bold;
  width: 2rem;
  height: 2rem;
  transition: background-color 100ms ease-in-out;
  &:hover {
    background-color: rgb(255, 89, 89);
    cursor: pointer;
  }
}

.main-input-submitting {
  width: 1.5em !important;
  height: 1.5em !important;
  background-color: white !important;
  border-top: 2px solid rgb(255, 215, 215) !important;
  box-shadow: none !important;
  animation-name: spin;
  animation-duration: 900ms;
  animation-iteration-count: infinite;
  animation-timing-function: linear;
  animation-delay: 0;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.reddit-orange {
  color: rgb(255, 101, 101);
}

@media screen and (max-width: 600px) {
  .main-container {
    width: 80vw;
  }

  .main-group-1 {
    grid-gap: 5vw;
  }

  .main-logo {
    font-size: 20vw;
    line-height: 20vw;
  }

  .main-tag-line {
    font-size: 4vw;
  }

  #main-input-container {
    width: 90vw;
  }

  #main-input-url {
    font-size: 3vh;
  }
}

footer {
  position: absolute;
  bottom: 20px;
  font-family: Arimo, Helvetica, sans-serif;
  font-size: 11.25px;
  color: rgb(192, 192, 192);
  a {
    color: rgb(179, 179, 179);
    text-decoration: none;
  }
}
</style>

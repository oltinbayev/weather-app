<template>
  
  <div class="container mt-3">
    <h2>Search city</h2>
    <div class="card">
      <div class="card-body">
        <input type="text" v-model="city" class="form-control"/>
      </div> 
      <div class="card-footer">
        <button class="btn btn-primary" @click="weather">get Weather</button>
      </div>
    </div>
  </div>

  <div class='box'>
    <div class='wave -one'></div>
    <div class='wave -two'></div>
    <div class='wave -three'></div>
    <div class="weathercon"><i class='fas fa-sun' style='color: #d36326;'></i></div>
    <div class="info">
      <h2 class="location">{{resultName}}</h2>
      <p class="date">  </p>
      <h1 class="temp">{{temperature}} &deg;C | {{faringate}} &deg;F</h1>
      <p class="time">{{time}}</p>
    </div>
  </div>
  <span>Enable location to see yours! :)</span>
  
</template>

<script>

export default {
  data() {
    return {
      city: "",
      resultName: "",
      temperature: "",
      faringate: "",
      time:"",
      
      
    }
  },
  name: 'App',
  methods: {
    weather: async function() {
      this.baseUrl = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=063d6ad0744657d976b44acebae3ed63`;
      const response = await fetch(this.baseUrl);
      const data = await response.json();
      
      console.log(data);

      this.resultName = data['name'];
      this.temperature = parseInt(data['main']['temp']-273.15);
      this.faringate = parseFloat((data['main']['temp']-273.15)*9/5+32).toFixed(2);
      this.time = data['timezone']
  
    }
  }
}

</script>


<style>

  * {
    padding: 0;
    margin: 0;
    font-family: Quicksand;
  }

  body {
    background: #f3f2ef;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-wrap: wrap;
    height: 100vh;
    width: 100vw;
  }

  html,
  body {
    height: 100%;
  }

  html {
    background: #eee;
  }

  .box {
    width: 20vw;
    height: 60vh;
    border-radius: 5px;
    box-shadow: 0 2px 30px rgba(black, 0.2);
    background: darken(#eff3f9, 20%);
    position: relative;
    overflow: hidden;
    transform: translate3d(0, 0, 0);
    min-width: 200px;
    min-height: 350px;
  }

  .wave {
    opacity: 0.3;
    position: absolute;
    top: 120%;
    left: 50%;
    background: white;
    width: 500px;
    height: 500px;
    margin-left: -250px;
    margin-top: -250px;
    transform-origin: 50% 48%;
    border-radius: 43%;
    animation: drift 3000ms infinite linear;
    z-index: 1;
  }

  .wave.-three {
    animation: drift 5000ms infinite linear;
    z-index: 2 !important;
    opacity: 0.2;
  }

  .wave.-two {
    animation: drift 7000ms infinite linear;
    opacity: 0.1;
    z-index: 3 !important;
  }

  .box:after {
    content: "";
    display: block;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    z-index: 11;
    transform: translate3d(0, 0, 0);
  }

  @keyframes drift {
    from {
      transform: rotate(0deg);
    }
    from {
      transform: rotate(360deg);
    }
  }

  .info {
    position: absolute;
    bottom: 0;
    width: 100%;
    height: 45%;
    z-index: 4;
  }

  .location {
    text-align: center;
    font-weight: 800;
  }

  .date {
    text-align: center;
    margin-top: 5%;
    color: lighten(grey, 10%);
    font-size: 70%;
  }

  .temp {
    margin-top: 10%;
    text-align: center;
  }

  .weathercon {
    height: 55%;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 3em;
  }

  @media (max-width: 600px) {
    .box {
      width: 90vw;
      height: 80vh;
    }

    .wave {
      top: 85%;
    }

    .weathercon {
      font-size: 5em;
    }

    .info {
      font-size: 1.5rem;
    }
  }

  @media (max-height: 500px) {
    .box {
      height: 80vh;
    }

    .wave {
      top: 115%;
    }
  }

  body > span {
    width: 100vw;
    text-align: center;
    color: grey;
  }


</style>

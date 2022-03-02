<template>  
<div class="container">
  <div class="weather-side">
    <div class="weather-gradient"></div>
    <div class="date-container">
      <h3 class="date-dayname">{{dataBuilder(d)}}</h3><span class="date-day"></span><i class="location-icon" data-feather="map-pin"></i><span class="location">{{resultName}}</span>
    </div>
    <div class="weather-container"><i class="weather-icon" data-feather="sun"></i>
      <h1 class="weather-temp">{{temperature}}°C</h1>
      <h3 class="weather-desc">{{description}}</h3>
      <h3>test</h3>
    </div>
  </div>
  <div class="info-side">
    <div class="today-info-container">
      <div class="today-info">
        <div class="precipitation"> <span class="title">PRECIPITATION</span><span class="value">0 %</span>
          <div class="clear"></div>
        </div>
        <div class="humidity"> <span class="title">HUMIDITY</span><span class="value">{{humidity}} %</span>
          <div class="clear"></div>
        </div>
        <div class="wind" placeholder="0"> <span class="title">WIND</span><span class="value">{{wind}}km/h</span>
          <div class="clear"></div>
        </div>
      </div>
    </div>
    <div class="week-container">
      <ul class="week-list">
        <li class="active"><i class="day-icon" data-feather="sun"></i><span class="day-name">Tue</span><span class="day-temp">{{temperature}}°C</span></li>
        <li><i class="day-icon" data-feather="cloud"></i><span class="day-name">{{dataBuilder2(d)}}</span><span class="day-temp">{{nextDay}}°C</span></li>
        <li><i class="day-icon" data-feather="cloud-snow"></i><span class="day-name">{{dataBuilder3(d)}}</span><span class="day-temp">{{thirdDay}}°C</span></li>
        <li><i class="day-icon" data-feather="cloud-rain"></i><span class="day-name">{{dataBuilder4(d)}}</span><span class="day-temp">{{fourthDay}}°C</span></li>
        <div class="clear"></div>
      </ul>
    </div>
    <div class="location-container">
      <button class="location-button" @click="weather"> <i data-feather="map-pin"></i><span>Change location</span></button>
    </div>
    <div class="card-body">
          <input type="text" placeholder="What city?" v-model="city" class="form-control"/>
        </div> 
  </div>
</div>
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
		description:"",
		humidity:"",
		wind:"",
		iden:"",
		lat:0,
		lon:0,
		nextDay:"",
		
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
		this.time = data['timezone'];
		this.description = data['weather']['0']['description'];
		this.humidity = data['main']['humidity'];
		this.wind = data['wind']['speed'];
		this.lat = data['coord']['lat'];
		this.lon = data['coord']['lon'];
		this.iden = data['id'];
		
		this.daysUrl = `http://api.openweathermap.org/data/2.5/forecast?id=${this.iden}&appid=8615bec277d231bd28c7db06798d5a17`;
		const week = await fetch(this.daysUrl);
		const dataDays = await week.json();
		
		console.log(dataDays);

		this.nextDay = parseFloat((dataDays['list']['8']['main']['temp']-273.15)).toFixed(0);
		this.thirdDay = parseFloat((dataDays['list']['16']['main']['temp']-273.15)).toFixed(0);
		this.fourthDay = parseFloat((dataDays['list']['24']['main']['temp']-273.15)).toFixed(0);
		},

		dataBuilder() {
		let d = new Date();
		let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
		let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
		let month = months [d.getMonth()]
		let year = d.getFullYear()
		let day = days[d.getDay()]
		let date = d.getDate()
		return `${month} ${year} ${day} ${date}`
		},

		dataBuilder2(){
		let d = new Date();
		let days = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
		let day = days[d.getDay()+1]
		return `${day}`
		},

		dataBuilder3(){
		let d = new Date();
		let days = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
		let day = days[d.getDay()+2]
		return `${day}`
		},

		dataBuilder4(){
		let d = new Date();
		let days = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
		let day = days[d.getDay()+3]
		return `${day}`
		}
		
	}
}

</script>


<style>

  @import url('https://fonts.googleapis.com/css?family=Montserrat:400,700,900&display=swap');

:root {
	--gradient: linear-gradient( 135deg, #72EDF2 10%, #5151E5 100%);
}

* {
	-webkit-box-sizing: border-box;
	box-sizing: border-box;
	line-height: 1.25em;
}

.clear {
	clear: both;
}

body {
	margin: 0;
	width: 100%;
	height: 100vh;
	font-family: 'Montserrat', sans-serif;
	background-color: #343d4b;
	display: -webkit-box;
	display: -ms-flexbox;
	display: flex;
	-webkit-box-align: center;
	-ms-flex-align: center;
  align-items: center;
	-webkit-box-pack: center;
  -ms-flex-pack: center;
  justify-content: center;
}

.container {
	border-radius: 25px;
	-webkit-box-shadow: 0 0 70px -10px rgba(0, 0, 0, 0.2);
  box-shadow: 0 0 70px -10px rgba(0, 0, 0, 0.2);
	background-color: #222831;
	color: #ffffff;
	height: 450px;
}

.weather-side {
	position: relative;
	height: 100%;
	border-radius: 25px;
	background-image: url("https://images.unsplash.com/photo-1559963110-71b394e7494d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=675&q=80");
	width: 300px;
	-webkit-box-shadow: 0 0 20px -10px rgba(0, 0, 0, 0.2);
  box-shadow: 0 0 20px -10px rgba(0, 0, 0, 0.2);
	-webkit-transition: -webkit-transform 300ms ease;
	transition: -webkit-transform 300ms ease;
	-o-transition: transform 300ms ease;
	transition: transform 300ms ease;
	transition: transform 300ms ease, -webkit-transform 300ms ease;
	-webkit-transform: translateZ(0) scale(1.02) perspective(1000px);
  transform: translateZ(0) scale(1.02) perspective(1000px);
	float: left;
}

.weather-side:hover {
	-webkit-transform: scale(1.1) perspective(1500px) rotateY(10deg);
  transform: scale(1.1) perspective(1500px) rotateY(10deg);
}

.weather-gradient {
	position: absolute;
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	background-image: var(--gradient);
	border-radius: 25px;
	opacity: 0.8;
}

.date-container {
	position: absolute;
	top: 25px;
	left: 25px;
}

.date-dayname {
	margin: 0;
}

.date-day {
	display: block;
}

.location {
	display: inline-block;
	margin-top: 10px;
}

.location-icon {
	display: inline-block;
	height: 0.8em;
	width: auto;
	margin-right: 5px;
}

.weather-container {
	position: absolute;
	bottom: 25px;
	left: 25px;
}

.weather-icon.feather {
	height: 60px;
	width: auto;
}

.weather-temp {
	margin: 0;
	font-weight: 700;
	font-size: 4em;
}

.weather-desc {
	margin: 0;
}

.info-side {
	position: relative;
	float: left;
	height: 100%;
	padding-top: px;
}

.today-info {
	padding: 15px;
	margin: 0 25px 25px 25px;
/* 	box-shadow: 0 0 50px -5px rgba(0, 0, 0, 0.25); */
	border-radius: 10px;
}

.today-info>div:not(:last-child) {
	margin: 0 0 10px 0;
}

.today-info>div .title {
	float: left;
	font-weight: 700;
}

.today-info>div .value {
	float: right;
}

.week-list {
	list-style-type: none;
	padding: 0;
	margin: 10px 35px;
	-webkit-box-shadow: 0 30px 30px -5px rgba(0, 0, 0, 0.25);
box-shadow: 0 0 30px -5px grey;
	border-radius: 10px;
	background: none
}

.week-list>li {
	float: left;
	padding: 15px;
	cursor: pointer;
	-webkit-transition: 200ms ease;
	-o-transition: 200ms ease;
	transition: 200ms ease;
	border-radius: 10px;
}

.week-list>li:hover {
	-webkit-transform: scale(1.1);
	-ms-transform: scale(1.1);
  transform: scale(1.1);
	background: #fff;
	color: #222831;
	-webkit-box-shadow: 0 0 40px -5px rgba(0, 0, 0, 0.2);
  box-shadow: 0 0 40px -5px rgba(0, 0, 0, 0.2)
}

.week-list>li.active {
	background: #fff;
	color: #222831;
	border-radius: 10px;
}

.week-list>li .day-name {
	display: block;
	margin: 10px 0 0 0;
	text-align: center;
}

.week-list>li .day-icon {
	display: block;
	height: 30px;
	width: auto;
	margin: 0 auto;
}

.week-list>li .day-temp {
	display: block;
	text-align: center;
	margin: 10px 0 0 0;
	font-weight: 700;
}

.location-container {
	padding: 25px 35px;
}

.location-button {
	outline: none;
	width: 100%;
	-webkit-box-sizing: border-box;
  box-sizing: border-box;
	border: none;
	border-radius: 25px;
	padding: 10px;
	font-family: 'Montserrat', sans-serif;
	background-image: var(--gradient);
	color: #ffffff;
	font-weight: 700;
	-webkit-box-shadow: 0 0 30px -5px rgba(0, 0, 0, 0.25);
  box-shadow: 0 0 30px -5px rgba(0, 0, 0, 0.25);
	cursor: pointer;
	-webkit-transition: -webkit-transform 200ms ease;
	transition: -webkit-transform 200ms ease;
	-o-transition: transform 200ms ease;
	transition: transform 200ms ease;
	transition: transform 200ms ease, -webkit-transform 200ms ease;
}

.location-button:hover {
	-webkit-transform: scale(0.95);
  -ms-transform: scale(0.95);
  transform: scale(0.95);
}

.location-button .feather {
	height: 1em;
	width: auto;
	margin-right: 5px;
}
.card-body {
	width: 90%;
	margin-left: auto;
	margin-right: auto;
}
.form-control {
	border-radius: 25px;
}
</style>

<template>
<div id="app">
    <main>
        <div class="search-box">
            <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="confirm" />
        </div>
        <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
            <div class="location-box">
                <div class="location">
                    {{ weather.name }}, {{ weather.sys.country }}
                </div>
                <div class="date">{{ dateBuilder() }}</div>
            </div>
            <div class="weather-box">
                <div class="temp">
                    {{ Math.round(weather.main.temp - 273.15) + " °C" }}
                </div>
                <div class="weather">{{ weather.weather[0].main }}</div>
            </div>
        </div>
        <div id="validResult"></div>
    </main>
</div>
</template>

<script>
import defaultPhoto from "./assets/default-photo.jpg";

document.getElementById("app").style.backgroundImage = `url('${defaultPhoto}')`;

export default {
    name: "Wheather-App",
    data() {
        return {
            api_key: "3007bb628c77f3b75c60e192310ffb8e",
            url_base: "https://api.openweathermap.org/data/2.5/",
            query: "",
            weather: {},
            photo_base: "https://api.unsplash.com/",
            photo_authorize: "JFxFGcYccueRtWlDFckedMkNpQlvL_iVTziioT62p-8",
            photo: defaultPhoto,
        };
    },
    methods: {
        confirm(e) {
            if (e.key == "Enter") {
                this.fetchWeather();
                this.getImage();
            }
        },
        fetchWeather() {
            fetch(
                    `${this.url_base}weather?q=${this.query}&units=metrics&APPID=${this.api_key}`
                )
                .then((res) => {
                    return res.json();
                })
                .then(this.setResults);
        },
        setResults(results) {
            if (results.cod !== "404") {
                this.weather = results;
            } else
                document.getElementById('validResult').innerHTML = "<span>NOT FOUND</span>";
        },
        dateBuilder() {
            let date = new Date();
            let day = date.getDay();
            let month = date.getMonth();
            const monthNames = [
                "January",
                "February",
                "March",
                "April",
                "May",
                "June",
                "July",
                "August",
                "September",
                "October",
                "November",
                "December",
            ];
            let year = date.getFullYear();
            return `${day} ${monthNames[month]} ${year}`;
        },
        setPhoto(result) {
            if (result.results.length > 0) {
                let background = result.results[0].urls.full;
                this.photo = background;
                document.getElementById(
                    "app"
                ).style.backgroundImage = `url('${background}')`;
            } else {
                this.photo = defaultPhoto;
            }
        },
        getImage() {
            try {
                fetch(
                        `${this.photo_base}/search/photos/?client_id=${this.photo_authorize}&page=1&query=${this.query}>; rel="first"`
                    )
                    .then((res) => {
                        return res.json();
                    })
                    .then(this.setPhoto);
            } catch (error) {
                this.setPhoto();
            }
        },
    },
};
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: 0;
}

#app {
    background-attachment: fixed;
    background-position: center;
    transition: 0.4s;
    background-size: cover;
}

main {
    min-height: 100vh;
    padding: 25px;
}

.search-box,
.weather-wrap,
.weather-box {
    width: 100%;
    max-width: 350px;
    margin: 0 auto;
}

.weather-wrap {
    margin-top: 4rem;
}

.weather-box {
    margin-top: 2rem;
}

.search-bar {
    display: inline-block;
    box-sizing: border-box;
    width: 100%;
    padding: 1%;
    font-size: 1.2rem;
    background-color: lightyellow;
    color: black;
    outline: none;
}

.search-bar:focus {
    outline: none;
}

.location,
.temp,
.date,
.weather,
#validResult {
    text-align: center;
    font-weight: bold;
    text-shadow: 2px 2px black;
    color: #fff;
    letter-spacing: 1.3px;
}

.location {
    font-size: 2rem;
    margin-bottom: 0.6rem;
}

.temp {
    font-size: 3rem;
}

.date,
.weather {
    font-size: 1.6rem;
}

#validResult {
    font-size: 2rem;
    margin-top: 2rem;
}
</style>

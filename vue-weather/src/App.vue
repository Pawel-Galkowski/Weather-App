<template>
<div id="app">
    <main>
        <div class="search-box">
            <input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather" />
        </div>
        <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
            <div class="location-box">
                <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
                <div class="date">{{dateBuilder()}}</div>
            </div>
            <div class="wheather-box">
                <div class="temp">{{Math.round(weather.main.temp)}}</div>
                <div class="weather">{{weather.weather[0].main}}</div>
            </div>
        </div>
    </main>
</div>
</template>

<script>
export default {
    name: "Wheather-App",
    data() {
        return {
            api_key: "3007bb628c77f3b75c60e192310ffb8e",
            url_base: "https://api.openweathermap.org/data/2.5/",
            query: "",
            weather: {},
        };
    },
    methods: {
        fetchWeather(e) {
            if (e.key == "Enter") {
                fetch(
                        `${this.url_base}weather?q=${this.query}&units=metrics&APPID=${this.api_key}`
                    )
                    .then((res) => {
                        return res.json();
                    })
                    .then(this.setResults);
            }
        },
        setResults(results) {
            this.weather = results;
        },
        dateBuilder() {
            let date = new Date();
            let day = date.getDay();
            let month = date.getMonth();
            const monthNames = ["January", "February", "March", "April", "May", "June",
                "July", "August", "September", "October", "November", "December"
            ];
            let year = date.getFullYear();
            return `${day} ${monthNames[month]} ${year}`
        }
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
    background-position: bottom;
    transition: 0.4s;
    background-size: cover;
}

main {
    min-height: 100vh;
    padding: 25px;
}
</style>

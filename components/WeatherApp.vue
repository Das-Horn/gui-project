<script>
    export default {
        data : () => ({
            temperature : ""
        }),
        methods : {
            getFormattedDate(date) {
                return date.getFullYear()+"-"+((Number(date.getMonth()) + 1) < 10 ? "0" + (Number(date.getMonth())) : (Number(date.getMonth())) ) +"-"+date.getDate();
            },
        
            sleep(ms) {
                return new Promise(resolve => setTimeout(resolve, ms));
            },
        
            async hotCold(temp) {
                const background = document.querySelector(".cont");
                let i = 0;
        
                for(i=0; i < 80; i++) {
                    if(temp > 17) {
                        // hotter
                        background.style.background = `linear-gradient(107.46deg, #C1FFFF ${100 - i}%, #FFE6A5 100%)`;
                    } else {
                        // colder
                        background.style.background = `linear-gradient(107.46deg, #C1FFFF ${i}%, #FFE6A5 100%)`;
                    }
                    await this.sleep(20);
                }
            },
        
            async getWeather(pos) {
                const date = new Date;
        
                var data = await fetch(`https://api.open-meteo.com/v1/forecast?latitude=${pos.coords.latitude}&longitude=${pos.coords.longitude}&hourly=temperature_2m&start_date=${this.getFormattedDate(date)}&end_date=${this.getFormattedDate(date)}`)
                    .then((req) => {
                        return req.json();
                    });        
                this.temperature = `Tempature : ${data.hourly.temperature_2m[date.getHours()]}°C`;
                console.log(this.temperature);
                this.hotCold(data.hourly.temperature_2m[date.getHours()]);   
            },

        },
        mounted () {
            console.log("Getting Locaction...");    
            if (navigator) {
                navigator.geolocation.getCurrentPosition((pos) => {
                    console.log(`Current position : ${pos.coords.longitude}\t${pos.coords.latitude}`);
                    this.getWeather(pos);
                })
            } else {
                console.warn("Geolocation not supported in this context");
            }
        }
    }

</script>

<style>
    .div{
        /* background-color: rgb(42, 42, 42); */
        width:              50%;
        height:             50%;
        color:              #fff;
        font-family:        Arial, Helvetica, sans-serif;
        padding:            10px;
        font-size:          2em;
        background:         rgba(80, 80, 80, 0.33);
        box-shadow:         12px 12px 20px 8px rgba(0, 0, 0, 0.25);
        backdrop-filter:    blur(2px);
        display:            flex;
        justify-content:    center;
        align-items:        center;
        border-radius:      90px;
    }
</style>

<template> 
    <div class="div">
        <p id="result">
            <img v-if="temperature == true" src="/loading.gif" height="50%" />
            <p v-else>{{temperature}}</p>
        </p>
    </div>
</template>
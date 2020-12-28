<template>
    <section class="weather">

        <div class="error" v-if="error !== null">{{error}}</div>
        <div class="overlay" v-if="error !== null"></div>
        <div class="days">
            <span>days</span>
            <span v-for="(num, i) in 3" :key="i" 
                  @click="selectDay(i)" :class="{'active-num': selectedNum === i}">{{i + 1}}</span>
        </div>
        <div class="condition">
            <div class="info">
                <p class="time">{{weatherData.date}}</p>
                <p class="address"> <img src="../assets/images/placeholder.svg" alt="location">
                 {{location.region}} , {{location.country}}
                </p>

                <p class="temp-num">
                    <span>{{weatherData.day.avgtemp_c}}</span>
                    <img :src="weatherData.day.condition.icon" alt="icon">
                </p>
                <p class="temp-text">{{weatherData.day.condition.text}}</p>
                <p class="rain"><span>{{weatherData.day.daily_chance_of_rain}} </span> : is the daily chance of rain</p>
            </div>
            <div class="line"></div>       
            <div class="more-info">
                <p>max temprature : <span>{{weatherData.day.maxtemp_c}}</span></p>
                <p>min temprature : <span>{{weatherData.day.mintemp_c}}</span></p>
                <p>wind speed : <span>{{weatherData.day.maxwind_kph}} kph</span></p>
                <p>sun rise at : <span>{{weatherData.astro.sunrise}}</span></p>
                <p>sun set at : <span>{{weatherData.astro.sunset}}</span></p>
            </div>
        </div>
    </section>
</template>
<script>
import{ref} from 'vue'
export default {
    props:['weatherData', 'location','error'],
    emits:['getDay'],
    setup(props,context){
        const selectedNum= ref(0)
        function selectDay(i){
            selectedNum.value= i
            context.emit('getDay', i)
        }
        return{ selectedNum, selectDay}
    }
}
</script>
<style lang="scss">
.active-num{
    background-color: var(--secondary-color);
    color: rgb(33, 39, 48);
}
.weather{
    position: relative;
    width: 65vw;
    @media only screen and (max-width: 50em) {
        width: 80vw;
    } 
    background-color: var(--primary-color-2);
    margin: 10rem auto 0;
    border-radius: 3rem;
    overflow: hidden;
    & .days{
        //padding: 2rem 5rem 0;
        display: flex;
        justify-content: space-between;
        width: 50%;
        & span{
            position: relative;
            font-size: 2.3rem;
            font-weight: 700;
            padding: 1.5rem;
            cursor: pointer;
            transition: all .5s ease;
            
            &:hover{
                background-color: var(--secondary-color);
                color: rgb(33, 39, 48);
            }
            &:nth-child(1){
                background-color: var(--secondary-color);
                color: rgb(33, 39, 48);
                cursor: inherit;
            }
        }
    }
    & .condition{
        padding: 5rem;
        display: flex;
        @media only screen and (max-width: 50em) {
            flex-wrap: wrap;
        } 
        & .line{
            width: 3px;
            border-radius: 3px;
            align-self: stretch;
            background-color: var(--secondary-color);
            @media only screen and (max-width: 50em) {
                display: none;
            } 
        }
        & .info{
            flex-basis: 55%;
            @media only screen and (max-width: 50em) {
                flex-basis: 100%;
            } 
            & .address{
                font-size: 3rem;
                margin-top: 2rem;
                @media only screen and (max-width: 62.5em) {
                    font-size: 2.8rem;
                }
                & img{
                    width: 3rem;
                }
            }
            
            & .temp-num{
                font-size: 7rem;
                color: var(--secondary-color);
                margin: 3rem 0;
                display: flex;
                & img{
                    width: 10rem;
                    margin-left: 2rem;
                }
            }
            & .temp-text{
                font-size: 3rem;
                margin-bottom: 2rem;
            }
            & .rain{
                & span{
                    color: var(--secondary-color);
                    margin-right: 1rem;
                }
            }
        }
        & .more-info{
            margin-left: 6rem;
            align-self: center;
            @media only screen and (max-width: 50em) {
                flex-basis: 100%;
                margin-top: 10rem;
                margin-left: 0;
                display: flex;
                flex-wrap: wrap;
                justify-content: space-between;
            } 
            & p{
                letter-spacing: 1px;
                text-transform: capitalize;
                @media only screen and (max-width: 50em) {
                    flex: 0 1 45%;
                    //margin: 3rem 3rem 0 0;
                } 
                @media only screen and (max-width: 25em) {
                    flex: 0 1 100%;
                    //margin: 3rem 3rem 0 0;
                } 
                & span{
                    color: var(--secondary-color);
                    margin-left: 1rem;
                    font-size: 2.3rem;
                }
                &:not(:last-child){
                    margin-bottom: 2rem;
                }
            }
        }
    }
}
.error{
  background-color: var(--error-color);
  padding: 2rem;
  border-radius: 1rem;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 100;
}
.overlay{
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    background-color: rgba(0,0,0, .6);
}
</style>
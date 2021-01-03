<template>
    <div class="header">
        <div class="container">
            <div class="header__brand"> the weather</div>
            <input type="text" placeholder="your country"  v-model.trim="city" @keypress.enter="submitCity">
            <div class="header-actions">
                <button @click="resetCity" class="reset-city">
                    reset city 
                </button>
                <div class="input-group">
                    <label for="dark">
                        <transition name="fade" mode="out-in">
                        <img src="../assets/images/sun.svg" alt="sun" v-if="isDark">
                        <img src="../assets/images/moon.svg" alt="moon" v-else>
                        </transition>
                    </label>
                    <input type="checkbox" id="dark" @click="$emit('get-theme')">
                </div>
            </div>
            
        </div>
    </div>
</template>
<script>
import{ref, watch} from 'vue'
export default {
    props:['isDark'],
    setup(_, context){
        const city= ref('')
        const submitedCity= ref('')
        function submitCity() {
            submitedCity.value= city.value
        }
        function resetCity() {
            city.value= ''
            submitedCity.value= ''
        }
        
        watch(submitedCity,(newCity, oldCity)=>{
            context.emit("get-city", newCity)
        })
        
        return{ 
            city,
            submitCity,
            resetCity,
            
        }
    }
}
</script>
<style lang="scss">

.header{
    background-color: var(--primary-color-2);
    padding: 2.5rem 0;
    margin-bottom: 3rem;
    & .container{
        display: flex;
        justify-content: space-between;
        align-items: center;
        flex-wrap: wrap;
        & .header__brand{
            font-weight: 700;
            font-size: 3rem;
            text-transform: capitalize;
            letter-spacing: 2px;
        }
        & input{
            flex: 0 1 50%;
            padding: 1rem 2rem;
            border: none;
            border-radius: 5rem;
            color: rgb(33, 39, 48);
            font-size: inherit;
            font-family: inherit;
            transition: all .2s ;
            &:focus{
                outline: none;
                flex: 0 1 53%;
            }
            &::placeholder{
                color: #b9b9b9;
            }
            &[type='checkbox']{
                display: none;
            }
            @media only screen and (max-width: 50em) {
                flex: 0 1 100%;
                order: 3;
                margin-top: 2rem;
                &:focus{
                flex: 0 1 100%;
            }
            }
        }
        .header-actions{
            flex: 0 1 20%;
            display: flex;
            justify-content: space-between;
            @media only screen and (max-width: 50em) {
                flex: 0 1 25%;
            }
            @media only screen and (max-width: 37.5em) {
                flex: 0 1 40%;
            }
            & .input-group{
                & label{
                    width: 100%;
                }
                & img{
                    width: 5rem;
                    cursor: pointer;
                    transition: all .6s;
                }
            }
            .reset-city{
                background-color: var(--secondary-color);
                align-self: center;
                padding: 1rem 1.2rem;
                color: rgb(33, 39, 48);
                font-size: 1.6rem;
                letter-spacing: 1px;
                border-radius: 5rem;
                border: none;
                cursor: pointer;
                text-transform: capitalize;
                &:focus{
                    outline: none;
                }
            
            }
        }
        
    }

}


.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
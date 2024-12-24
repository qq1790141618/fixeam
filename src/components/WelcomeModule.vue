<template>
    <div
    class="welcome"
    >
        <div
        v-for="item, index in welcomeText"
        :key="index"
        class="welcome-text"
        >
            <span
            v-for="word in item"
            :key="word"
            class="welcome-word"
            :class="lightColor.indexOf(word) >= 0 ? 'light-color': ''"
            :style="{
                display: clienWidth < 600 && word == ' '? 'block':''
            }"
            >
                {{ word }}
            </span>
        </div>
    </div>
</template>

<script>
import { onMounted } from 'vue'
export default {
    setup(){
        let welcomeText = ['WELCOME TO FIXEAM', 'BY Limit Au']
        const lightColor = ['B', 'W', 'L', 'A', 'T', 'F' ]

        const jumpWord = () => {
            let welcomeTextBox = document.querySelectorAll('.welcome-text')
            let welcomeWordBox = document.querySelectorAll('.welcome-word')

            for (let i = 0; i < welcomeTextBox.length; i++) {
                welcomeTextBox[i].style.display = 'none'
            }
            for (let i = 0; i < welcomeWordBox.length; i++) {
                welcomeWordBox[i].style.transform = 'rotateX(180deg)'
            }

            setTimeout(() => {
                for (let i = 0; i < welcomeTextBox.length; i++) {
                    welcomeTextBox[i].style.display = 'inline-block'
                }

                welcomeTextBox[0].className = 'welcome-text'
                welcomeTextBox[1].className += ' hdd'

                welcomeWordBox = welcomeTextBox[0].querySelectorAll('.welcome-word')
                let welcomeWordBoxNumber = 0

                let rush = setInterval(() => {
                    if(welcomeWordBoxNumber >= welcomeWordBox.length){
                        clearInterval(rush)
                        
                        setTimeout(() => {
                            welcomeTextBox[1].className = 'welcome-text'
                            welcomeTextBox[0].className += ' hdd'

                            welcomeWordBox = welcomeTextBox[1].querySelectorAll('.welcome-word')
                            welcomeWordBoxNumber = 0

                            rush = setInterval(() => {
                                if(welcomeWordBoxNumber >= welcomeWordBox.length){
                                    clearInterval(rush)
                                } else {
                                    welcomeWordBox[welcomeWordBoxNumber].style.transform = 'rotateX(0deg)'
                                    welcomeWordBoxNumber++
                                }
                            }, 100)
                        }, 1500)
                    } else {
                        welcomeWordBox[welcomeWordBoxNumber].style.transform = 'rotateX(0deg)'
                        welcomeWordBoxNumber++
                    }
                }, 100)
            }, 500)
        }

        const clienWidth = document.documentElement.clientWidth
        
        onMounted(() => {
            jumpWord()

            setInterval(() => {
                jumpWord()
            }, 8000)
        })

        return{
            welcomeText,
            lightColor,
            clienWidth
        }
    }
}
</script>

<style>
@import url('https://fonts.font.im/css?family=El+Messiri:700');

.welcome{
    position: relative;
    height: 100vh;
    width: 100%;
}
.welcome-text{
    position: absolute;
    top: 45%;
    left: 50%;
    transform: translate(-50%, -50%);
    white-space: nowrap;
    padding: 30px 200px;
    background-color: #fff;
    overflow: hidden;
}
.hdd{
    padding: 0;
    width: 0;
    height: 0;
}
.welcome-word{
    display: inline-block;
    min-width: 24px;
    font-weight: 700;
    font-size: 85px;  
    font-family: 'El Messiri', sans-serif;
    transform-origin: 50% 42%;
    transform: rotateX(180deg);
    transition: all 0.5s ease;
    color: #555;
}
.light-color{
    color: #F7BA1E;
}

@media (max-width: 600px) {
    .welcome-word{
        font-size: 60px;
    }
}
</style>
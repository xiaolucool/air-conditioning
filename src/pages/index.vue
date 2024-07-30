<template>
    <div class="index">
        <div class="ear">
            <div class="ear-item">
                <div class="ear-bg"></div>
            </div>
            <div class="ear-item">
                <div class="ear-bg"></div>
            </div>
        </div>
        <div class="content">
            <div v-show="!show" :class="checkShow ? 'show show-l' : 'show show-r'">
                {{ checkShow ? '❄' : '☀' }}
            </div>
            <div v-show="!show" class="num">{{ num }}<sup>&deg;</sup></div>
            <div class="eye">
                <div class="eye-item"></div>
                <div>ω</div>
                <div class="eye-item"></div>
            </div>
            <div class="face">
                <div class="face-item"></div>
                <div class="face-item"></div>
            </div>
            <div class="strip"></div>
            <div class="strip-air"></div>
        </div>
    </div>
    <audio ref="audioRef">
        <source :src="mp3[mp3Num]" type="audio/mpeg">
        Your browser does not support the audio element.
    </audio>

    <div class="center" style="transition: opacity 300ms ease-in-out 0s; opacity: 1;">
        <div v-show="!show" class="wind-effect">
            <div class="wind-line" style="background-color: rgb(187, 187, 187); transform: rotate(10deg);">
            </div>
            <div class="wind-line mx-20" style="background-color: rgb(187, 187, 187);"></div>
            <div class="wind-line" style="background-color: rgb(187, 187, 187); transform: rotate(-10deg);">
            </div>
        </div>
    </div>
    <div class="btn">
        <button @click="check(1)" style="background-color: #1976D2;">❄</button>
        <button :class="show ? 'def' : 'active'" @click="onSwitch"><svg xmlns="http://www.w3.org/2000/svg" width="14"
                height="14" viewBox="0 0 1024 1024">
                <path fill="#ffffff"
                    d="M352 159.872V230.4a352 352 0 1 0 320 0v-70.528A416.128 416.128 0 0 1 512 960a416 416 0 0 1-160-800.128" />
                <path fill="#ffffff" d="M512 64q32 0 32 32v320q0 32-32 32t-32-32V96q0-32 32-32" />
            </svg></button>
        <button @click="check(2)" style="background-color: #FFA500;">☀</button>
    </div>
    <div class="btn2">
        <button @click="add">+</button>
        <button @click="reduce">-</button>
    </div>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue'
import di from '@/assets/audio/di.mp3'
import ac from '@/assets/audio/ac-work.mp3'
import air from '@/assets/audio/air-extractor-fan.mp3'
// 开关
let show = ref(true)
// 显示
let checkShow = ref(true)
// 温度
let num = ref(25)
// 音频索引
let mp3Num = ref(0)
// 音频源
const mp3 = [di, ac, air]
// 音频元素
const audioRef = ref<HTMLAudioElement | null>(null)

// 默认颜色对象

interface Theme {
    skinColor: string
    borderColor: string
    numBgCold: string
    eyeColor: string
    faceColor: string
    earColor: string
}

let theme = ref<Theme>({
    skinColor: '#E0A387',
    borderColor: '#442119',
    numBgCold: '#85D3EB',
    eyeColor: '#4D2422',
    faceColor: '#FDBD8D',
    earColor: '#E0A387'

})
const themeList: Array<Theme> = [
    {
        skinColor: '#E0A387',
        borderColor: '#442119',
        numBgCold: '#85D3EB',
        eyeColor: '#4D2422',
        faceColor: '#FDBD8D',
        earColor: '#E0A387'
    }, {
        skinColor: '#FEFEFE',
        borderColor: '#442119',
        numBgCold: '#85D3EB',
        eyeColor: '#4D2422',
        faceColor: '#FFBDBE',
        earColor: '#49221D'
    }
]


const props = defineProps<{
    selectedOption: number
}>()
const onColor = () => {
    let index = props.selectedOption
    theme.value = themeList[index]
}
onColor()

defineExpose({onColor})
const add = () => {
    if (show.value) return
    mp3Num.value = 0
    if (audioRef.value) {
        audioRef.value.load()
        audioRef.value.play()
    }
    if (num.value == 32) return
    num.value++

    setTimeout(() => {
        mp3Num.value = 2
        if (audioRef.value) {
            audioRef.value.load()
            audioRef.value.play()
        }
    }, 1000)
}
const reduce = () => {
    if (show.value) return
    mp3Num.value = 0
    if (audioRef.value) {
        audioRef.value.load()
        audioRef.value.play()
    }
    if (num.value == 0) return
    num.value--

    setTimeout(() => {
        mp3Num.value = 2
        if (audioRef.value) {
            audioRef.value.load()
            audioRef.value.play()
        }
    }, 1000)
}
const onSwitch = () => {
    show.value = !show.value
    if (show.value) {
        mp3Num.value = 0
        if (audioRef.value) {
            audioRef.value.load()
            audioRef.value.play()
        }
    } else {
        mp3Num.value = 1
        if (audioRef.value) {
            audioRef.value.load()
            audioRef.value.play()
            setTimeout(() => {
                if (show.value) return
                mp3Num.value = 2
                if (audioRef.value) {
                    audioRef.value.load()
                    audioRef.value.play()
                }
            }, 8500)
        }
    }

}
const check = (num: number) => {
    if (show.value) return
    mp3Num.value = 0
    if (num == 1) {
        checkShow.value = true
    } else {
        checkShow.value = false
    }

    if (audioRef.value) {
        audioRef.value.load()
        audioRef.value.play()
    }
}
</script>

<style scoped>
.index {
    margin: 30px;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.content {
    width: 300px;
    height: 120px;
    background-color: v-bind('theme.skinColor');
    border: 4px solid v-bind('theme.borderColor');
    border-radius: 25px;
    display: flex;
    flex-direction: column;
    align-items: center;
    position: relative;
}

.num {
    position: absolute;
    right: 20px;
    top: 10px;
    color: white;
    font-size: 12px;
    background-color: v-bind('theme.numBgCold');
    padding: 0px 4px 2px 4px;
    border-radius: 10px;
}

.show {
    position: absolute;
    right: 50px;
    top: 6px;
    font-size: 20px;
}

.show-l {
    color: #59B2C1;
}

.show-r {
    color: #FFE200;
}

.ear {
    margin: 0 auto;
    width: 200px;
    display: flex;
    justify-content: space-between;
    margin-bottom: -25px;
}

.ear-item {
    width: 50px;
    height: 50px;
    background-color: v-bind('theme.earColor');
    border: 4px solid v-bind('theme.borderColor');
    border-radius: 50%;
    padding: 5px;
}

.ear-bg {
    width: 34px;
    height: 34px;
    background-color: v-bind('theme.borderColor');
    border-radius: 50%;
}

.eye {
    margin-top: 25px;
    width: 80px;
    display: flex;
    justify-content: space-between;
}

.eye-item {
    width: 16px;
    height: 16px;
    background-color: v-bind('theme.eyeColor');
    border-radius: 50%;
}

.face {
    margin-top: -5px;
    width: 160px;
    display: flex;
    justify-content: space-between;
}

.face-item {
    width: 40px;
    height: 40px;
    background-color: v-bind('theme.faceColor');
    border-radius: 50%;
}

.strip {
    width: 85%;
    height: 18px;
    border-radius: 25px;
    background-color: v-bind('theme.eyeColor');
    border: 4px solid v-bind('theme.borderColor');
    position: relative;
}

.strip-air {
    width: 84.5%;
    height: 18px;
    border-radius: 25px;
    border: 4px solid v-bind('theme.borderColor');
    margin-top: -12px;
}

.strip::after {
    content: ")";
    color: v-bind('theme.borderColor');
    font-size: 20px;
    font-weight: bolder;
    position: absolute;
    top: 10px;
    left: -4px;
    transform: rotate(30deg);
}

.strip::before {
    content: "(";
    color: v-bind('theme.borderColor');
    font-size: 20px;
    font-weight: bolder;
    position: absolute;
    top: 10px;
    right: -4px;
    transform: rotate(-30deg);
}

.center {
    height: 80px;
}

.wind-effect {
    display: flex;
    align-items: center;
    justify-content: center;
}

.wind-line {
    width: 5px;
    height: 60px;
    margin: 0 20px;
}

.btn {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.btn2 {
    display: flex;
    flex-direction: column;
    align-items: center;

    button {
        margin: 10px;
    }
}

button {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    border: none;
    margin: 0 10px;
}

.active {
    background-color: #F33531;
}

.def {
    background-color: #43A047;
}
</style>
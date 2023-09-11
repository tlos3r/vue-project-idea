<template>
    <h1>Simple Music Player</h1>
    <section class="music-play">
        <h3>{{ current.title }} - {{ current.artist }}</h3>
        <audio @timeupdate="changeTimelinePosition" ref="audioRef">
            <source :src="current.src" />
        </audio>
        <!-- <input  @change="changeSeek" ref="timelineRef" type="range" class="timeline" max="100" value="0" /> -->
        <button @click="prev">Prev</button>
        <button @click="play" v-if="!isPlayed">Play</button>
        <button @click="pause" v-else>Stop</button>
        <button @click="next">Next</button>
    </section>
    <section class="playlist">
        <h3>Danh sách âm nhạc</h3>
        <ul>
            <li v-for="song in songs" :key="song.src" @click="play(song)">{{ song.title }} - {{ song.artist }}</li>
        </ul>
    </section>
</template>

<script setup lang="ts">
import { ref, reactive, onBeforeMount, onUpdated } from "vue";
const current: {
    title: string;
    artist: string;
    src: string;
} = reactive({
    title: "",
    artist: "",
    src: "",
});
const songs = ref([
    {
        title: "Anh không cố ý",
        artist: "Ogenus , Limitnxss",
        src: new URL("./assets/AnhKhongCoY.mp3", import.meta.url).href,
    },
    {
        title: "Don't côi",
        artist: "Mr.Nợ , Ronboongz",
        src: new URL("./assets/DontCoi.mp3", import.meta.url).href,
    },
    {
        title: "Khi cơn mưa dần phai",
        artist: "Tez0suy , Myra Trần",
        src: new URL("./assets/KhiConMuaDanPhai.mp3", import.meta.url).href,
    },
]);
const player = new Audio();
const index = ref(2);
const isPlayed = ref(false);
const audioRef = ref();
const timelineRef = ref();

// play function event
const play = (song: any) => {
    if (typeof song.src != "undefined") {
        current.title = song.title;
        current.artist = song.artist;
        current.src = song.src;
        player.src = current.src;
    }
    player.play();
    isPlayed.value = true;
};
// pause function event
const pause = () => {
    player.pause();
    isPlayed.value = false;
};

const next = () => {
    index.value++;
    console.log(index.value);
    console.log(songs.value.length);
    if (index.value === songs.value.length) {
        index.value = 0;
        current.title = songs.value[index.value].title;
        current.artist = songs.value[index.value].artist;
        current.src = songs.value[index.value].src;
        player.src = current.src;
        player.play();
    } else {
        current.title = songs.value[index.value].title;
        current.artist = songs.value[index.value].artist;
        current.src = songs.value[index.value].src;
        player.src = current.src;
        player.play();
        console.log(index.value);
    }
    isPlayed.value = true;
};

const prev = () => {
    index.value--;
    console.log(index.value);
    console.log(songs.value.length);
    player.pause();
    if (index.value < 0) {
        index.value = songs.value.length - 1;
        console.log(index.value);
        current.title = songs.value[index.value].title;
        current.artist = songs.value[index.value].artist;
        current.src = songs.value[index.value].src;
        player.src = current.src;
        player.play();
    } else {
        current.title = songs.value[index.value].title;
        current.artist = songs.value[index.value].artist;
        current.src = songs.value[index.value].src;
        player.src = current.src;
        player.play();
    }
    isPlayed.value = true;
};

const changeSeek = () => {
    const time = (timelineRef.value.value * audioRef.value.duration) / 100;
    audioRef.value.currentTime = time;
};

const changeTimelinePosition = () => {
    const percentagePosition = (100 * audioRef.value.currentTime) / audioRef.value.duration;
    timelineRef.value.style.backgroundSize = `${percentagePosition}% 100%`;
    timelineRef.value.value = percentagePosition;
};
onBeforeMount(() => {
    current.title = songs.value[index.value].title;
    current.artist = songs.value[index.value].artist;
    current.src = songs.value[index.value].src;
    player.src = songs.value[index.value].src;
});
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Comfortaa:wght@400;500&family=Orbitron:wght@400;600&display=swap");
body {
    font-family: "Comfortaa", sans-serif;
    height: 100vh;
}
h1 {
    text-align: center;
}
.music-play {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: row;
    flex-wrap: wrap;
    flex: 1 0 50%;
}
.music-play h3 {
    flex: 0 1 100%;
    text-align: center;
}
.music-play button {
    margin-right: 10px;
    padding: 10px 20px;
    background-color: #2cccff;
    border: 1px solid transparent;
    color: #fff;
    font-size: 16px;
}
.music-play button:hover {
    background-color: #8edef9;
    cursor: pointer;
}
.playlist {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    font-size: 18px;
    font-weight: 500;
    margin-top: 50px;
}
.playlist > ul {
    list-style-type: none;
}
.playlist ul li {
    cursor: pointer;
}
.timeline {
    -webkit-appearance: none;
    width: 100px;
    height: 0.5em;
    background-color: #e5e5e5;
    border-radius: 5px;
    background-size: 0% 100%;
    background-image: linear-gradient(blue, blue);
    background-repeat: no-repeat;
    margin-right: 5px;
}
.timeline::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 1em;
    height: 1em;
    border-radius: 50%;
    cursor: pointer;
    opacity: 0;
    transition: all 0.1s;
    background-color: blue;
}

.timeline::-moz-range-thumb {
    -webkit-appearance: none;
    width: 1em;
    height: 1em;
    border-radius: 50%;
    cursor: pointer;
    opacity: 0;
    transition: all 0.1s;
    background-color: blue;
}

.timeline::-ms-thumb {
    -webkit-appearance: none;
    width: 1em;
    height: 1em;
    border-radius: 50%;
    cursor: pointer;
    opacity: 0;
    transition: all 0.1s;
    background-color: blue;
}

.timeline::-webkit-slider-thumb:hover {
    background-color: #2cccff;
}

.timeline:hover::-webkit-slider-thumb {
    opacity: 1;
}

.timeline::-moz-range-thumb:hover {
    background-color: #2cccff;
}

.timeline:hover::-moz-range-thumb {
    opacity: 1;
}

.timeline::-ms-thumb:hover {
    background-color: #2cccff;
}

.timeline:hover::-ms-thumb {
    opacity: 1;
}

.timeline::-webkit-slider-runnable-track {
    -webkit-appearance: none;
    box-shadow: none;
    border: none;
    background: transparent;
}

.timeline::-moz-range-track {
    -webkit-appearance: none;
    box-shadow: none;
    border: none;
    background: transparent;
}

.timeline::-ms-track {
    -webkit-appearance: none;
    box-shadow: none;
    border: none;
    background: transparent;
}
</style>

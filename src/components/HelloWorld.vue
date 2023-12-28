<template>
    <div>
        <div class="music-buttons">
            <!-- 각각의 음악을 제어하는 버튼들 -->
            <button @click="toggleMusic(1)">{{ isPlaying[1] ? "펑크 STOP" : "펑크" }}</button>
            <button @click="toggleMusic(2)">{{ isPlaying[2] ? "디스코 STOP" : "디스코" }}</button>
            <button @click="toggleMusic(3)">{{ isPlaying[3] ? "하이퍼팝 STOP" : "하이퍼팝" }}</button>
            <button @click="toggleMusic(4)">{{ isPlaying[4] ? "재즈 STOP" : "재즈" }}</button>
            <button @click="toggleMusic(5)">{{ isPlaying[5] ? "KDA STOP" : "KDA" }}</button>
            <button @click="toggleMusic(6)">{{ isPlaying[6] ? "마에스트로 STOP" : "마에스트로" }}</button>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            // 각 음악 트랙에 대한 오디오 요소 및 재생 여부 상태 데이터
            audio: {
                1: null,
                2: null,
                3: null,
                4: null,
                5: null,
                6: null,
            },
            isPlaying: {
                1: false,
                2: false,
                3: false,
                4: false,
                5: false,
                6: false,
            },
        }
    },
    methods: {
        playWithIncreasingVolume(musicNumber) {
            // 음악을 증가하는 볼륨과 함께 재생하는 함수
            const audio = this.audio[musicNumber]
            const targetVolume = 0.1 // 목표 음량
            const duration = 3000 // 3초동안 음량을 증가시킵니다.
            const intervalTime = 50 // 100ms마다 음량을 조절합니다.
            const volumeStep = targetVolume / (duration / intervalTime) // 각 간격마다 증가할 음량

            let currentVolume = 0 // 현재 음량
            audio.volume = currentVolume
            audio.play() // 음악을 재생합니다.

            this.isPlaying[musicNumber] = true

            const volumeInterval = setInterval(() => {
                if (currentVolume >= targetVolume) {
                    clearInterval(volumeInterval) // 시간이 다 되면 간격을 지웁니다.
                } else {
                    currentVolume += volumeStep // 음량을 증가시킵니다.
                    audio.volume = currentVolume
                }
            }, intervalTime)

            // 음악이 끝날 때마다 재생을 중단합니다.
            audio.onended = () => {
                this.isPlaying[musicNumber] = false
            }

            // 음악 재생
            audio.currentTime = 0 // 음악을 처음으로 되감습니다.
            audio.play()
        },

        toggleMusic(musicNumber) {
            // 음악 재생을 토글하는 함수
            const audio = this.audio[musicNumber]
            if (this.isPlaying[musicNumber]) {
                audio.pause()
                this.isPlaying[musicNumber] = false
            } else {
                this.playWithIncreasingVolume(musicNumber)
            }
        },
    },
    mounted() {
        // 각 음악에 대한 오디오 요소 초기화 및 설정
        this.audio[1] = new Audio("/music/PunkMusic.mp3")
        this.audio[1].loop = true
        this.audio[1].onended = () => {
            this.isPlaying[1] = false
        }

        this.audio[2] = new Audio("/music/DiscoMusic.mp3")
        this.audio[2].loop = true
        this.audio[2].onended = () => {
            this.isPlaying[2] = false
        }

        this.audio[3] = new Audio("/music/HyperPopMusic.mp3")
        this.audio[3].loop = true
        this.audio[3].onended = () => {
            this.isPlaying[3] = false
        }

        this.audio[4] = new Audio("/music/JazzMusic.mp3")
        this.audio[4].loop = true
        this.audio[4].onended = () => {
            this.isPlaying[4] = false
        }

        this.audio[5] = new Audio("/music/K_DAMusic.mp3")
        this.audio[5].loop = true
        this.audio[5].onended = () => {
            this.isPlaying[5] = false
        }

        this.audio[6] = new Audio("/music/MaestroJhinMusic.mp3")
        this.audio[6].loop = true
        this.audio[6].onended = () => {
            this.isPlaying[6] = false
        }
    },
}
</script>

<style>
.music-buttons {
    display: flex;
    justify-content: space-around;
    margin-top: 20px; /* 버튼과 상위 요소 사이의 간격 조절 */
}

/* 버튼 스타일링 (옵션) */
button {
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
    background-color: #3498db;
    color: #fff;
    border: none;
    border-radius: 5px;
}

button:hover {
    background-color: #2980b9;
}
</style>

<template>
    <div>
        <div class="music-buttons">
            <!-- 각각의 음악을 제어하는 버튼들 -->
            <button @click="toggleMusic(1)">{{ isPlaying[1] ? "펑크 STOP" : "펑크 Early" }}</button>
            <button @click="toggleMusic(2)">{{ isPlaying[2] ? "펑크 STOP" : "펑크 Late" }}</button>
            <button @click="toggleMusic(3)">{{ isPlaying[3] ? "디스코 STOP" : "디스코 Early" }}</button>
            <button @click="toggleMusic(4)">{{ isPlaying[4] ? "디스코 STOP" : "디스코 Late" }}</button>
            <button @click="toggleMusic(5)">{{ isPlaying[5] ? "하이퍼팝 STOP" : "하이퍼팝 Early" }}</button>
            <button @click="toggleMusic(6)">{{ isPlaying[6] ? "하이퍼팝 STOP" : "하이퍼팝 Late" }}</button>
            <button @click="toggleMusic(7)">{{ isPlaying[7] ? "재즈 STOP" : "재즈 Early" }}</button>
            <button @click="toggleMusic(8)">{{ isPlaying[8] ? "재즈 STOP" : "재즈 Late" }}</button>
            <button @click="toggleMusic(9)">{{ isPlaying[9] ? "KDA STOP" : "KDA Early" }}</button>
            <button @click="toggleMusic(10)">{{ isPlaying[10] ? "KDA STOP" : "KDA Late" }}</button>
            <button @click="toggleMusic(11)">{{ isPlaying[11] ? "마에스트로 STOP" : "마에스트로 Early" }}</button>
            <button @click="toggleMusic(12)">{{ isPlaying[12] ? "마에스트로 STOP" : "마에스트로 Late" }}</button>
        </div>
        <div class="control-buttons">
            <button @click="startSelectedMusic">Start</button>
            <button @click="stopAllMusic">Stop All</button>
        </div>
        <!-- 선택된 음악 실행을 위한 버튼 -->
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
                7: null,
                8: null,
                9: null,
                10: null,
                11: null,
                12: null,
            },
            isPlaying: {
                1: false,
                2: false,
                3: false,
                4: false,
                5: false,
                6: false,
                7: false,
                8: false,
                9: false,
                10: false,
                11: false,
                12: false,
            },

            // 선택된 음악들을 저장하기 위한 배열
            selectedMusic: [], // 선택된 음악 번호를 저장할 배열
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

        // 선택된 음악 동시 실행을 위한 함수
        startSelectedMusic() {
            this.selectedMusic.forEach((musicNumber) => {
                if (!this.isPlaying[musicNumber]) {
                    this.playWithIncreasingVolume(musicNumber)
                }
            })
        },

        toggleMusic(musicNumber) {
            // 음악 재생을 토글하는 함수
            const audio = this.audio[musicNumber]
            if (this.isPlaying[musicNumber]) {
                audio.pause()
                this.isPlaying[musicNumber] = false
                // 선택된 음악 배열에서 해당 음악 제거
                const index = this.selectedMusic.indexOf(musicNumber)
                if (index > -1) {
                    this.selectedMusic.splice(index, 1)
                }
            } else {
                // 선택된 음악 배열에 해당 음악 추가
                this.selectedMusic.push(musicNumber)
            }
        },

        stopAllMusic() {
            // 모든 음악을 중지하는 함수
            this.selectedMusic.forEach((musicNumber) => {
                if (this.isPlaying[musicNumber]) {
                    this.audio[musicNumber].pause()
                    this.isPlaying[musicNumber] = false
                }
            })
            this.selectedMusic = [] // 선택된 음악 배열 비우기
        },
    },
    mounted() {
        // 각 음악에 대한 오디오 요소 초기화 및 설정
        this.audio[1] = new Audio("/music/punk_early_main.ogg")
        this.audio[1].onended = () => {
            this.isPlaying[1] = false
        }

        this.audio[2] = new Audio("/music/punk_late_main.ogg")
        this.audio[2].onended = () => {
            this.isPlaying[3] = false
        }

        this.audio[3] = new Audio("/music/disco_early_main.ogg")
        this.audio[3].onended = () => {
            this.isPlaying[3] = false
        }

        this.audio[4] = new Audio("/music/disco_late_main.ogg")
        this.audio[4].onended = () => {
            this.isPlaying[4] = false
        }

        this.audio[5] = new Audio("/music/hyperpop_early.ogg")
        this.audio[5].onended = () => {
            this.isPlaying[5] = false
        }

        this.audio[6] = new Audio("/music/hyperpop_late.ogg")
        this.audio[6].onended = () => {
            this.isPlaying[6] = false
        }

        this.audio[7] = new Audio("/music/jazz_early_main.ogg")
        this.audio[7].onended = () => {
            this.isPlaying[7] = false
        }

        this.audio[8] = new Audio("/music/jazz_late_main.ogg")
        this.audio[8].onended = () => {
            this.isPlaying[8] = false
        }

        this.audio[9] = new Audio("/music/kda_early_main.ogg")
        this.audio[9].onended = () => {
            this.isPlaying[9] = false
        }
        this.audio[10] = new Audio("/music/kda_late_main.ogg")
        this.audio[10].onended = () => {
            this.isPlaying[10] = false
        }

        this.audio[11] = new Audio("/music/maestro_early.ogg")
        this.audio[11].onended = () => {
            this.isPlaying[11] = false
        }

        this.audio[12] = new Audio("/music/maestro_late.ogg")
        this.audio[12].onended = () => {
            this.isPlaying[12] = false
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

.control-buttons {
    display: flex;
    justify-content: center;
    margin-top: 20px;
    text-align: center;
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
    margin: 0 10px; /* 버튼 간 간격 조절을 위한 설정 */
}

button:hover {
    background-color: #2980b9;
}

.clicked-button {
    background-color: #ffcc00 !important; /* 변경할 배경색 */
}
</style>

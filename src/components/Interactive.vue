<template>
    <div class="score">Accuracy: {{ this.totalCorrectGuesses + '/' + this.totalGuesses }}</div>
    <div class="remainingRooms">remainingRooms = {{ Object.keys(facesAndRooms).length }}</div>
    <div class="youWin" v-if="win">
        <div>YOU WIN!!!</div>
        <button @click="startOver()">Play Again</button>
    </div>
    <div class="facesAndRoom">
        <div class="faces">
            <div class="person" v-for="(person, key) in facesAndRoomsForGuessing">
                <img :src="'/faces/' + person.personImage" class="personImage" @click="personClicked(key)" />
                <div class="personName">{{ person.personName }}</div>
            </div>
        </div>
        <div class="roomImageContainer">
            <div class="viewerWrapper">
                <div id="viewer"></div>
            </div>
        </div>
    </div>
</template>

<script>
import '@photo-sphere-viewer/core/index.css';
import facesAndRoomsFromFile from '../assets/facesAndRooms.json'
import { Viewer } from '@photo-sphere-viewer/core';

export default {
    data() {
        return {
            facesAndRooms: {...facesAndRoomsFromFile},
            facesAndRoomsForGuessing: null,
            roomForGuessing: null,
            correctAnswerKey: null,
            totalGuesses: 0,
            totalCorrectGuesses: 0,
            viewer: null,
            win: false
        }
    },
    mounted() {
        this.setNewRound();
    },
    methods: {
        setNewRound() {
            this.facesAndRoomsForGuessing = this.getRandomFacesAndRooms();
            if (Object.keys(this.facesAndRoomsForGuessing).length === 0) {
                this.win = true;
                return;
            }
            this.correctAnswerKey = this.getRandomKey(this.facesAndRoomsForGuessing);

            if (this.viewer) {
                this.viewer.setPanorama('/rooms/' + this.facesAndRoomsForGuessing[this.correctAnswerKey].roomImage);
            } else {
                this.viewer = new Viewer({
                    container: document.querySelector('#viewer'),
                    panorama: '/rooms/' + this.facesAndRoomsForGuessing[this.correctAnswerKey].roomImage,
                });
            }

        },
        getRandomFacesAndRooms() {
            const entries = Object.entries(this.facesAndRooms);
            const shuffled = entries.sort(() => 0.5 - Math.random());
            const selected = shuffled.slice(0, 8);
            return Object.fromEntries(selected);
        },
        getRandomKey(obj) {
            const keys = Object.keys(obj);
            return keys[Math.floor(Math.random() * keys.length)];
        },
        personClicked(key) {
            this.totalGuesses++;
            if (key === this.correctAnswerKey) {
                this.totalCorrectGuesses++;
                delete this.facesAndRooms[key];
                this.setNewRound();
            }
        },
        startOver() {
            this.facesAndRooms = facesAndRoomsFromFile;
            this.win = false;
            this.totalCorrectGuesses = 0;
            this.totalGuesses = 0;
            this.setNewRound();
        }
    }
}
</script>

<style scoped>
.youWin,.score,.remainingRooms {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
}

.about {
    padding: 2% 7% 4%;
    font-family: inter;
    font-weight: 400;
}

.facesAndRoom {
    display: flex;
    justify-content: space-between;
}

.faces {
    display: flex;
    width: 50%;
    overflow-x: auto;
    height: fit-content;
    align-items: center;
    padding: 2%;
}

.personImage {
    width: 200px;
    border: 5px solid white;
    cursor: pointer;
}

.personName {
    font-family: inter;
    text-align: center;
    color: black;
}

.personImage.current {
    border: 5px solid red;
}

.room {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    container-type: inline-size;
    padding: 2%;
    font-family: inter;
    font-weight: 700;
    text-transform: uppercase;
}

.roomName {
    text-align: center;
    padding: 0 0 4%;
}

.roomImage {
    height: 70cqw;
}

.roomImageContainer {
    width: 50%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.viewerWrapper {
    height: 500px;
    width: 500px;
}

#viewer {
    width: 100%;
    height: 100%;

}

@media (max-width: 1024px) {
    .facesAndRoom {
        flex-direction: column-reverse;
    }

    .faces {
        width: 100%;
    }

    .roomImageContainer {
        width: 100%;
        height: 500px;
    }

    .viewerWrapper {
        width: 90%;
        height: 100%;
    }
}
</style>
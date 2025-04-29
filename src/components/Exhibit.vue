<template>
    <div class="facesAndRoom">
        <div class="faces">
            <div class="person" v-for="(person, key) in facesAndRooms">
                <img :src="'/faces/' + person.personImage" class="personImage" @click="personClicked(key)"
                    :class="{ 'current': key === currentPersonKey }" />
                <div class="personName">{{ person.personName }}</div>
            </div>
        </div>
        <div class="roomImageContainer">
            <div class="viewerWrapper">
                <div id="viewer"></div>
            </div>
            <div>{{ facesAndRooms[currentPersonKey].personName }}'s Room</div>
        </div>
    </div>
</template>

<script>
import '@photo-sphere-viewer/core/index.css';
import facesAndRooms from '../assets/facesAndRooms.json'
import { Viewer } from '@photo-sphere-viewer/core';

export default {
    data() {
        return {
            facesAndRooms: facesAndRooms,
            currentPersonKey: (Object.keys(facesAndRooms)[0]) ? Object.keys(facesAndRooms)[0] : null,
            viewer: null
        }
    },
    mounted() {
        this.viewer = new Viewer({
            container: document.querySelector('#viewer'),
            panorama: '/rooms/' + this.facesAndRooms[this.currentPersonKey].roomImage,
        });
    },
    methods: {
        startInteractiveMode() {
            alert("Let's just focus on exhibit mode for now");
        },
        startExhibitMode() {
            alert("These buttons don't do anything right now. We're locked into exhibit mode")
        },
        personClicked(key) {
            this.currentPersonKey = key;
            if (this.viewer) {
                this.viewer.setPanorama('/rooms/' + this.facesAndRooms[key].roomImage);
            }
        }
    }
}
</script>

<style scoped>
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
    border: 5px solid green;
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
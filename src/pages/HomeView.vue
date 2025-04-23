<template>
    <div class="pageContainer">
        <div class="topBar">
            <div>Bedrooms</div>
            <div>Mode:
                <button @click="startInteractiveMode"
                    :class="{ 'selectedMode': mode === 'interactive' }">Interactive</button>
                <button @click="startExhibitMode" :class="{ 'selectedMode': mode === 'exhibit' }">Exhibit</button>
            </div>
            <div v-if="mode === 'interactive'">Score: TODO/TODO</div>
        </div>
        <div class="about">{{ facesAndRooms[currentPersonKey].about }}</div>
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
            mode: 'exhibit',
            currentPersonKey: (Object.keys(facesAndRooms)[0]) ? Object.keys(facesAndRooms)[0] : null,
            viewer: null
        }
    },
    mounted() {
        console.dir(facesAndRooms);
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
.pageContainer {
    height: 100%;
    width: 100%;
    background: black;
    color: white;
    overflow-y: auto;
}

.topBar {
    display: flex;
    justify-content: space-between;
}

button.selectedMode {
    border: 5px solid green;
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
}

.personImage {
    width: 200px;
    border: 5px solid white;
    cursor: pointer;
}

.personImage.current {
    border: 5px solid red;
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
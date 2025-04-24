<template>
    <div class="pageContainer">
        <div class="topBar">
            <div class="logo">Bedrooms</div>
            <div class="modes">
                <p>Mode:</p>
                <button @click="startInteractiveMode"
                    :class="{ 'selectedMode': mode === 'interactive' }">Interactive</button>
                <button @click="startExhibitMode" :class="{ 'selectedMode': mode === 'exhibit' }">Exhibit</button>
            </div>
        </div>
        <div class="aboutBedrooms">
            <p>
                Someone once said that "the eyes are the window of the soul." Clearly,
                they had never stepped inside a bedroom. <i>Bedrooms</i> is a portrait photography project
                that explores identity through a new lens (two fisheye lenses, to be exact). <br><br> Using a
                Garmin Virb 360 and a Panasonic Lumix GH5, each 360˚ photo captures the...
            </p>
        </div>
        <Exhibit v-if="mode === 'exhibit'"></Exhibit>
        <Interactive v-if="mode === 'interactive'"></Interactive>
    </div>
</template>

<script>
import Exhibit from '@/components/Exhibit.vue';
import Interactive from '@/components/Interactive.vue';

export default {
    data() {
        return {
            mode: 'exhibit',
        }
    },
    mounted() { },
    methods: {
        startInteractiveMode() {
            this.mode = 'interactive';
        },
        startExhibitMode() {
            this.mode = 'exhibit';
        },
    },
    components: { Exhibit, Interactive }
}
</script>

<style scoped>
@font-face {
    font-family: inter;
    src: url('../assets/inter/InterVariable.woff2') format('truetype');
}

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
    height: 9cqi;
    container-type: inline-size;
    padding: 2%;
}

.logo {
    font-size: 8cqi;
    font-family: inter;
    font-weight: 900;
    text-transform: uppercase;
}

.modes {
    display: flex;
    align-items: center;
    font-family: inter;
    font-weight: 700;
}

.modes p {
    padding-right: 5%;
}

.aboutBedrooms {
    display: flex;
    justify-content: center;
    padding: 2% 7%;
    font-size: 1.3rem;
    font-family: inter;
}

.aboutBedrooms p {
    align-items: center;
    max-width: 600px;
    text-align: justify;
    hyphens: auto;
    padding: 0;
    margin: 0;
}

.about {
    padding: 2% 7% 4%;
    font-family: inter;
    font-weight: 400;
}

button.selectedMode {
    color: white;
    background-color: rgb(89, 152, 89);
    /* border: rgb(89,152,89); */
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
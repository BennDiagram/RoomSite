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
        <div class="about">{{ peopleAndRooms[currentPersonKey].about }}</div>
        <div class="peopleAndRoom">
            <div class="people">
                <div class="person" v-for="(person, key) in peopleAndRooms">
                    <img :src="'/people/' + person.personImage" class="personImage" @click="personClicked(key)"
                        :class="{ 'current': key === currentPersonKey }" />
                    <div class="personName">{{ person.personName }}</div>
                </div>
            </div>
            <div class="room">
                <img :src="'/rooms/' + peopleAndRooms[currentPersonKey].roomImage" class="roomImage" />
                <div class="roomName">{{ peopleAndRooms[currentPersonKey].roomName }}</div>
            </div>
        </div>
    </div>
</template>

<script>
import peopleAndRooms from '../assets/peopleAndRooms.json'

export default {
    data() {
        return {
            peopleAndRooms: peopleAndRooms,
            mode: 'exhibit',
            currentPersonKey: (Object.keys(peopleAndRooms)[0]) ? Object.keys(peopleAndRooms)[0] : null
        }
    },
    mounted() {
        console.dir(peopleAndRooms);
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
        }
    }
}
</script>

<style scoped>
.pageContainer {
    height: 100%;
    width: 100%;
    background: black;
    color: white
}

.topBar {
    display: flex;
    justify-content: space-between;
}

button.selectedMode {
    border: 5px solid green;
}

.peopleAndRoom {
    display: flex;
    justify-content: space-between;
}

.people {
    display: flex;
}

.personImage {
    height: 200px;
    border: 5px solid white;
    cursor: pointer;
}

.personImage.current {
    border: 5px solid red;
}

.roomImage {
    height: 200px;
}

@media (max-width: 1024px) {
    .peopleAndRoom {
        flex-direction: column-reverse;
    }

    .people {
        overflow-x: auto;
    }
}
</style>
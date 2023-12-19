<script setup>
import { ref } from 'vue';

const emit = defineEmits(['lessonSubmit']);
const props = defineProps({
    newId: Number
});

const newLessonData = ref({
    name: '',
    durationMinutes: 45,
    durationHours: 0,
    category: '',
})

const handleSubmit = () => {
    const resultArr = [];

    const lessonToAdd = {
        name: newLessonData.value.name,
        duration: Number(newLessonData.value.durationMinutes + newLessonData.value.durationHours * 60),
        id: props.newId,
        category: newLessonData.value.category
    }

    if (lessonToAdd.duration > 60) {
        resultArr.push(...divideLongLesson(lessonToAdd))
    } else {
        resultArr.push(lessonToAdd);
    }

    emit('lessonSubmit', resultArr);

    newLessonData.value.name = '';
    newLessonData.value.durationMinutes = 45;
    newLessonData.value.durationHours = 0;
    newLessonData.value.category = '';
}

const divideLongLesson = (longLesson) => {
    const dividedLessons = [];

    const numberOfFullLessons = Math.floor(longLesson.duration / 60);

    const lastLessonDuration = longLesson.duration % 60;

    for (let i = 0; i < numberOfFullLessons; i++) {
        dividedLessons.push({
            name: longLesson.name,
            duration: 45,
            id: props.newId,
            category: longLesson.category
        })
    }

    if (lastLessonDuration > 0) {
        dividedLessons.push({
            name: longLesson.name,
            duration: lastLessonDuration,
            id: props.newId,
            category: longLesson.category
        });
    }

    return dividedLessons;
}
</script>

<template>
    <form @submit.prevent="handleSubmit">
        <input type="text" v-model="newLessonData.name" placeholder="name">
        <input type="number" v-model="newLessonData.durationHours" placeholder="duration in hours">
        <input type="number" v-model="newLessonData.durationMinutes" placeholder="duration in minutes">
        <input type="text" v-model="newLessonData.category" placeholder="category">
        <button type="submit">Add lesson</button>
    </form>
</template>
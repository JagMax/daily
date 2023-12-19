<script setup>
import { computed, ref } from 'vue';
import LessonsForm from './LessonsForm.vue';

const lessons = ref([
    {
        id: 0, name: 'lesson 1', duration: 45, category: 'test'
    },
    {
        id: 1, name: 'lesson 2', duration: 45, category: 'programming'
    },
    {
        id: 2, name: 'lesson 3', duration: 45, category: 'sport'
    },
    {
        id: 3, name: 'lesson 4', duration: 45, category: 'cleaning'
    },
    {
        id: 4, name: 'lesson 5', duration: 45, category: 'cooking'
    },
    {
        id: 5, name: 'lesson 6', duration: 45, category: 'test'
    },
]);

const categories = computed(() => new Set(lessons.value.map(item => item.category)));

const totalMinutesToStudy = computed(() => lessons.value.reduce((acc, current) => current.duration + acc, 0) + (lessons.value.length - 1) * 15);

const handleLessonSubmit = (lessonArr) => {
    lessons.value.push(...lessonArr);
}

</script>

<template>
    <div class="lessons">
        <h2>Lessons</h2>

        <ul class="categories">
            <li class="category" v-for="category in categories">
                <span>color</span> - {{ category }}
            </li>
        </ul>

        <p>Hours to study today: {{ Math.floor(totalMinutesToStudy / 60) }} hours {{ totalMinutesToStudy % 60 }} minutes</p>

        <ul>
            <template v-for="lesson, index in lessons" :key="lesson.id">
                <li>
                    <span>{{ lesson.name }}</span> ({{ lesson.duration }} min) - {{ lesson.category }}
                </li>
                <li v-if="index !== lessons.length - 1">
                    break (15 min)
                </li>
            </template>
        </ul>

        <LessonsForm @lessonSubmit="handleLessonSubmit" :newId="lessons.length" />
    </div>
</template>

<style lang="scss" scoped></style>
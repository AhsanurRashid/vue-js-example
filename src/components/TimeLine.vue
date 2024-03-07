<script setup>
import { ref, reactive } from 'vue'
const timelines = reactive([
    {
        date: '19-02-2024',
        time: '7:03 PM',
        name: 'Joan Marwa',
        status: 'active'
    },
    {
        date: '19-02-2024',
        time: '7:03 PM',
        name: 'Joan Marwa',
        status: 'in-active'
    },
    {
        date: '19-02-2024',
        time: '7:03 PM',
        name: 'Joan Marwa',
        status: 'in-active'
    },
    {
        date: '19-02-2024',
        time: '7:03 PM',
        name: 'Joan Marwa',
        status: 'in-active'
    }
]
)

const handeIncrease = () =>{
    const inactiveIndex = timelines.findIndex(item => item.status === 'in-active')
    if (inactiveIndex !== -1) {
        timelines[inactiveIndex].status = 'active';
    }
}
</script>

<template>
    <section id="timeline">
        <div class="timeline_header">
            <h1>TimeLine component</h1>
        </div>
        <div class="timeline_body">
            <div v-for="(timeline, index) in timelines" :key="timeline" class="timeline_warpper">
                <div class="timeline_left" @click="checkLength(index)" >
                    <div :class="`status ${timeline?.status === 'active' ? 'bg-green-500' : 'bg-slate-200'}`">
                        <div v-if="index > 0" :class="`progress_bar ${timeline?.status === 'active' ? 'bg-green-500' : 'bg-slate-200'}`"></div>
                    </div>
                </div>
                <div class="timeline_right">
                    <div :class="`date_and_time ${ timeline?.status === 'active' ? 'text-slate-700' : 'text-slate-400'}`">
                        <div>{{ timeline?.date }}</div>
                        <div>{{ timeline?.time }}</div>
                    </div>
                    <div :class="`name ${ timeline?.status === 'active' ? 'text-slate-700' : 'text-slate-400'}`">{{ timeline?.name }} stage has been changed</div>
                </div>
            </div>
            <div class="flex items-center gap-4 justify-center text-white">
                <div class="bg-gray-500 px-3 py-1 cursor-pointer shadow rounded">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5">
                        <path stroke-linecap="round" stroke-linejoin="round" d="m18.75 4.5-7.5 7.5 7.5 7.5m-6-15L5.25 12l7.5 7.5" />
                    </svg>
                </div>
                <div @click="handeIncrease" class="bg-sky-500 px-3 py-1 cursor-pointer shadow rounded">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5">
                        <path stroke-linecap="round" stroke-linejoin="round" d="m5.25 4.5 7.5 7.5-7.5 7.5m6-15 7.5 7.5-7.5 7.5" />
                    </svg>
                </div>
            </div>  
        </div>
    </section>
</template>

<style scoped>
#timeline {
    @apply md:px-0 px-4;
}
.timeline_header {
    @apply text-center text-lg tracking-wide font-semibold mt-4;
}

.timeline_body {
    @apply bg-slate-100 rounded-md px-6 py-8 xl:w-1/3 md:w-1/2 w-full mx-auto mt-4 shadow;
}

.timeline_warpper {
    @apply w-full flex;
}

.timeline_left {
    @apply w-[7%];
}

.timeline_right {
    @apply mb-10;
}

.status {
    @apply w-[15px] h-[15px] relative rounded-full;
}

.progress_bar {
    @apply w-[4px] h-[72px] absolute bottom-0 left-[5.5px];
}

.date_and_time {
    @apply text-sm font-semibold uppercase leading-3 tracking-wide flex items-center gap-2 mb-2;
}

.name {
    @apply text-xs leading-3 tracking-wide flex items-center gap-2;
}
</style>
<template>

    <div class="prose max-w-none text-slate-500 mb-10">
        <h1 class="mt-12" style="text-align: left;">Истории успеха</h1>
        <p style="text-align: justify;">Разбор реальных историй: от лечения хронических заболеваний до помощи в
            стрессовых ситуациях. Наглядно о том,
            как персональный подход гомеопата меняет жизнь пациентов.</p>
    </div>


    <div class="my-4 flex flex-col items-start gap-4 sm:flex-row animate__animated animate__faster"
        v-for="({ images, to, title, description, icon }, i) in $children"
        :class="{ animate__fadeInRight: fade[i], invisible: !fade[i], }"
        v-intersection-observer="([{ isIntersecting }]) => { fade[i] = isIntersecting }">
        <div @mouseover="slide[i] = true" @mouseleave="slide[i] = false" :class="`bg-[url(${images[0].url})]`"
            class="flex flex-auto w-full h-48 sm:w-48 sm:h-24 bg-center bg-cover overflow-hidden rounded">
            <transition enter-active-class="animate__animated animate__slideInUp animate__faster"
                leave-active-class="animate__animated animate__slideOutUp  animate__faster">
                <router-link v-if="slide[i]" class="bg-white/85 flex-auto flex justify-center items-center" :to="to">
                    <el-button size="large" circle="" tag="router-link" :to="to">
                        <icon :icon="icon"></icon>
                    </el-button>
                </router-link>
            </transition>
        </div>
        <router-link :to="to" class="flex w-full min-w-0 gap-4 text-base" @mouseover="mouseenter[i] = true"
            @mouseout="mouseenter[i] = false">
            <icon :icon="icon" class="size-10 animate__animated animate__slow text-emerald-500"
                :class="{ animate__shakeY: mouseenter[i], 'text-sky-800': mouseenter[i] }"></icon>
            <div class="flex flex-col items-start justify-center w-full min-w-0 gap-0 text-base">
                <h3 class="mb-4 text-lg leading-6 text-slate-700" :class="{ '!text-sky-800': mouseenter[i] }">
                    {{ title }}
                </h3>
                <p class="text-slate-500">{{ description }}</p>
            </div>
        </router-link>



    </div>

    <el-button class="not-prose" tag="router-link" :to="parent.to" :icon="Back" type="success">на
        главную</el-button>
</template>



<script setup>
import { inject, ref, watch } from "vue";
import { Back } from '@element-plus/icons-vue';

import { vIntersectionObserver } from "@vueuse/components";

const { id } = defineProps(["id"]),
    { $children, parent } = inject("pages")[id],
    fade = ref(Array($children.length).fill(true)),
    slide = ref([]),
    mouseenter = ref([]),
    once = true,
    deep = true;

watch(slide, () => { __unocss_runtime.extract("bg-white/85") }, { deep, once });
</script>

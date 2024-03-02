<script setup lang="ts">
import { onMounted, defineProps, ref } from "vue";
import Highlight from 'highlight.js'
import '../assets/css/theme.css';

import Copy from './Icons/Copy.vue'
import CopyChecked from "./Icons/CopyChecked.vue";

const lang = ref('')
const code = ref('')
const singleCopy = ref(false)
const props = defineProps<{
    lang: string
    code: string
    title?: string
    disableCopy?: boolean
}>()

onMounted(() => {
    lang.value = props.lang?.toUpperCase() ?? ''
    lang.value = lang.value.includes('-') ? lang.value.replace('-', ' ') : lang.value

    code.value = Highlight.highlight(props.code, {
        language: props.lang
    }).value;

})

function copyCode() {
    navigator.clipboard.writeText(props.code)
    singleCopy.value = true
}
</script>
<template>
    <div class="mx-auto wrapper-code-highlight mt-6">
        <div class="header-code-higlight" :class="props.disableCopy ? 'border-disable-copy' : 'border-copy'">
            <div :class="{ 'mr-6': !props.disableCopy, 'border-disable-copy' : props.disableCopy }" class="wrapper-section-code-highlight flex items-center">
                <div class="wrapper-title-code-highlight center h-full">
                    <span>
                        {{ props.title?.toUpperCase() ?? lang }}
                    </span>
                </div>
            </div>
            <div v-if="!props.disableCopy" @click="copyCode"
                class="wrapper-copy-code-highlight h-full px-4 flex bg-[#334155] cursor-pointer">
                <Copy v-if="!singleCopy" />
                <CopyChecked v-else />
            </div>
        </div>
        <pre v-if="!props.disableCopy"><code class="hljs" v-html="code" :class="'lang-' + `${props.lang}`"></code></pre>
        <pre
            v-else><code class="hljs" v-html="code" @copy.prevent @paste.prevent @click.right.prevent :class="'lang-' + `${props.lang}`"></code></pre>
    </div>
</template>
<style>
@import url('https://fonts.cdnfonts.com/css/general-sans?styles=135310');

.wrapper-title-code-highlight {
    font-family: 'General Sans', sans-serif;
}

pre code {
    border-radius: 0 0 10px 10px !important;
    margin: 0 !important;
}

.wrapper-code-highlight {
    background-color: #334155;
    border-radius: 12px;
}

.wrapper-section-code-highlight {
    background-color: #334155;
    border-radius: 10px 10px 10px 0;
    width: 100%;
    height: 100%;
    font-size: 14px;
}

.wrapper-title-code-highlight {
    padding: 0 16px;
    font-weight: 500;
    font-size: 12px;
    border-radius: 10px 0 0 0;
    background-color: #2563EB;
}

.wrapper-copy-code-highlight {
    padding: 0 16px;
    font-weight: 500;
    border-radius: 0 10px 0 10px;
}

.header-code-higlight {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #272822;
    height: 40px;
    color: white;
    width: 100%;
}

.border-copy {
    border-radius: 10px 10px 0 0;
}

.border-disable-copy {
    border-radius: 10px 10px 0 0;
}

.h-full {
    height: 100%;
}

.center {
    display: flex;
    align-items: center;
}

.mr-6 {
    margin-right: 6px;
}
</style>
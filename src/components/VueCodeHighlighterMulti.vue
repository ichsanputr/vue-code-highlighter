<script setup lang="ts">
import { onMounted, defineProps, ref, } from "vue";
import Highlight from 'highlight.js'
import '../assets/css/theme.css';

import Copy from './Icons/Copy.vue'
import CopyChecked from "./Icons/CopyChecked.vue";

const props = defineProps<{
    code: Array<{
        code: string
        lang: string
        title?: string
    }>
    disableCopy?: boolean
}>()

const currentCode = ref<number>(0)
const ready = ref<Boolean>(false)
const rendered = ref<Array<string>>([])
const copy = ref<Boolean>(false)
const code = ref<Array<{
    code: string
    lang: string
    title?: string
}>>([])

onMounted(async () => {
    props.code.forEach((v) => {
        code.value.push(Object.assign({}, v))
    })

    code.value[0].code = Highlight.highlight(code.value[0].code, {
        language: code.value[0].lang
    }).value;

    rendered.value.push(code.value[0].lang)
    ready.value = true
})

function copyCode() {
    const codeToCopy = props.code.filter((v) => v.lang == code.value[currentCode.value].lang)
    navigator.clipboard.writeText(codeToCopy[0].code)
    copy.value = true
}

function changeCurrentCode(index: number) {
    currentCode.value = index

    if (!rendered.value.includes(code.value[index].lang)) {
        code.value[index].code = Highlight.highlight(code.value[index].code, {
            language: code.value[index].lang
        }).value;

        rendered.value.push(code.value[index].lang)
    }

    copy.value = false
}
</script>
<template>
    <div v-if="ready" class="mx-auto wrapper-codemulti-highlight mt-6">
        <div class="header-codemulti-highlight">
            <div class="wrapper-section-codemulti-highlight flex items-center" :class="{ 'mr6': !props.disableCopy, 'border-disable-copy' : props.disableCopy }">
                <div @click="changeCurrentCode(key)" v-for="(item, key) in props.code"
                    :class="{ 'rounded-left-corner': key == 0, 'active': code[currentCode].lang == item.lang }"
                    class="wrapper-title-codemulti-highlight center h-full">
                    <span>
                        {{ item.title ? item.title.toUpperCase() : item.lang.toUpperCase() }}
                    </span>
                </div>
            </div>
            <div v-if="!props.disableCopy" @click="copyCode" class="wrapper-copy-codemulti-highlight h-full px-4 flex bg-[#334155] cursor-pointer">
                <Copy v-if="!copy" />
                <CopyChecked v-else />
            </div>
        </div>
        <pre v-if="!disableCopy"><code class="hljs" v-html="code[currentCode].code"></code></pre>
        <pre v-else><code @copy.prevent @paste.prevent @click.right.prevent class="hljs" v-html="code[currentCode].code"></code></pre>
    </div>
</template>
<style>
pre code {
    border-radius: 0 0 10px 10px !important;
    margin: 0 !important;
}

.wrapper-title-codemulti-highlight {
    font-family: 'General Sans', sans-serif;
}

.wrapper-codemulti-highlight {
    background-color: #334155;
    border-radius: 12px;
}

.wrapper-section-codemulti-highlight {
    background-color: #334155;
    border-radius: 10px 10px 10px 0;
    width: 100%;
    height: 100%;
    font-size: 14px;
}

.wrapper-title-codemulti-highlight {
    cursor: pointer;
    padding: 0 16px;
    font-weight: 500;
    font-size: 12px;
}

.wrapper-copy-codemulti-highlight {
    padding: 0 16px;
    font-weight: 500;
    border-radius: 0 10px 0 10px;
}

.header-codemulti-highlight {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #272822;
    height: 40px;
    color: white;
    width: 100%;
    border-radius: 10px 10px 0 0;
}

.rounded-left-corner {
    border-radius: 10px 0 0 0;
}

.no-rounded {
    border-radius: 0 !important;
}

.active {
    background-color: #2563EB;
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

.mr6 {
    margin-right: 6px;
}
</style>
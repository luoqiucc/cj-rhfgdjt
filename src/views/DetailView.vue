<template>
    <div class="view detail">
        <q-layout view="hHh lpR fFf">

            <q-header elevated class="bg-primary text-white">
                <q-toolbar>
                    <q-toolbar-title>
                        <q-avatar>
                            <img :src="baseUrl + '/images/logo.jpg'" alt="logo">
                        </q-avatar>
                        群刊 · {{ magazine.title }}
                    </q-toolbar-title>

                    <q-btn dense flat round icon="menu" @click="toggleRightDrawer"/>
                </q-toolbar>
            </q-header>

            <q-drawer show-if-above v-model="rightDrawerOpen" side="right" bordered>
                <q-img :src="baseUrl + magazine.cover"/>

                <q-banner class="bg-primary text-white">
                    <div class="text-h6">
                        {{ magazine.title }}
                    </div>
                </q-banner>

                <q-list class="q-mb-lg">
                    <q-item-label header class="text-subtitle1">{{ magazine.subtitle }}</q-item-label>
                </q-list>

                <q-list class="q-mb-lg">
                    <q-item-label header class="text-subtitle1">领衔主演</q-item-label>
                    <q-item clickable v-ripple v-for="item in magazine.starring">
                        <q-item-section avatar>
                            <q-avatar>
                                <img :src="baseUrl + item.avatar" alt="avatar">
                            </q-avatar>
                        </q-item-section>
                        <q-item-section class="text-subtitle1">
                            {{ item.user }}
                        </q-item-section>
                    </q-item>
                </q-list>

                <q-list class="q-mb-lg">
                    <q-item-label header class="text-subtitle1">全部期刊</q-item-label>
                    <q-item v-for="item in list">
                        <q-item-section>
                            <q-card flat bordered>
                                <q-card-section horizontal>
                                    <q-card-section class="q-pt-xs">
                                        <div class="text-h5 q-mt-sm q-mb-xs">{{ item.title }}</div>
                                        <div class="text-caption text-grey">
                                            {{ item.subtitle }}
                                        </div>
                                    </q-card-section>

                                    <q-card-section class="col-5 flex flex-center">
                                        <q-img
                                                class="rounded-borders"
                                                :src="baseUrl + item.cover"
                                        />
                                    </q-card-section>
                                </q-card-section>

                                <q-separator/>

                                <q-card-actions>
                                    <a :href="baseUrl + '/detail/' + item.id">
                                        <q-btn flat color="primary">
                                            阅读
                                        </q-btn>
                                    </a>
                                </q-card-actions>
                            </q-card>
                        </q-item-section>
                    </q-item>
                </q-list>

                <q-list>
                    <q-item-label header class="text-subtitle1"></q-item-label>
                    <q-item>
                        <q-item-section>
                            <a target="_blank" href="https://github.com/zhangao204/chat-journal">
                                <q-btn round color="black">
                                    <img src="@/assets/icons/github.svg" class="github" alt="github">
                                </q-btn>
                            </a>
                        </q-item-section>
                    </q-item>
                </q-list>

            </q-drawer>

            <q-page-container class="q-pa-md">
                <q-banner v-if="isHidden" inline-actions rounded class="bg-orange text-white r18">
                    <b>! 注意</b> 该文档为限制级，可能包含一些敏感内容。

                    <template v-slot:action>
                        <q-btn @click="isHidden=false" flat label="仍然显示"/>
                    </template>
                </q-banner>

                <div :class="{active: isHidden}">
                    <div v-for="item in magazine.chat">
                        <ChatFrame class="chat-frame" :chat="item.session"/>
                    </div>
                </div>

                <StoutstormLogo class="logo"/>
            </q-page-container>
        </q-layout>
    </div>
</template>

<script setup>
import {onMounted, ref} from 'vue'
import baseUrl from '@/config/base-url.js'
import list from '@/data'
import ChatFrame from '@/components/ChatFrame.vue'
import StoutstormLogo from "@/components/StoutstormLogo.vue";

const props = defineProps({
    id: String
})

const magazine = ref({})
const isHidden = ref(true)

onMounted(() => {
    switch (props.id) {
        case 'c202301':
            import('@/data/c202301').then((data) => {
                magazine.value = data.default
                if (!magazine.value['r18']) {
                    isHidden.value = false
                }
            })
            break
        case 'c202302':
            import('@/data/c202302').then((data) => {
                magazine.value = data.default
                if (!magazine.value['r18']) {
                    isHidden.value = false
                }
            })
            break
        default:
            import('@/data/c202301').then((data) => {
                magazine.value = data.default
                if (!magazine.value['r18']) {
                    isHidden.value = false
                }
            })
    }
})

const rightDrawerOpen = ref(false)
const toggleRightDrawer = () => {
    rightDrawerOpen.value = !rightDrawerOpen.value
}
</script>

<style scoped lang="sass">
.logo
  margin: 90px 20px 30px 20px
  justify-content: center

.github
  padding: 6px 8px 3px 6px
  width: 42px
  height: 42px

.r18
  max-width: 680px
  margin: 30px auto

.active
  filter: blur(5px)
  height: 60vh
  overflow: hidden
</style>
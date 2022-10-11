<script setup>
import { watch } from 'vue'
import { draft, put, del } from '../state.js'
import { Calendar, DatePicker } from 'v-calendar'
import 'v-calendar/dist/style.css'
const props = defineProps(['modelValue'])
const emits = defineEmits(['update:modelValue'])

async function submit () {
  if (draft.new) {
    const doc = { ...draft }
    delete doc.new
    await put(Math.random().toString(36).substr(2, 10), doc)
    return emits('update:modelValue', false)
  }
  await put(draft._id, { ...draft })
  emits('update:modelValue', false)
}

</script>

<template>
  <Transition name="fade">
    <div class="fixed top-0 left-0 w-screen h-screen bg-black flex items-center justify-center" v-if="props.modelValue" style="background-color: rgba(0, 0, 0, 0.5);" @click="emits('update:modelValue', false)">
      <div class="fixed relative px-10 py-8 bg-white rounded flex flex-col justify-center items-center" @click.stop="">
        <div class="flex items-center">
          <div class="text-gray-500">Course Name</div>
          <input v-model="draft.name" class="all-transition border rounded m-2 px-2 focus:border-blue-500">
        </div>
        <div class="flex items-center">
          <div class="text-gray-500">Course Code</div>
          <input v-model="draft.code" class="all-transition border rounded m-2 px-2 focus:border-blue-500 font-mono">
        </div>
        <div class="flex items-center">
          <div class="text-gray-500 m-2">Language</div>
          <input class="m-1" type="radio" id="Chinese" value="Chinese" v-model="draft.lang">
          <label class="m-1" for="Chinese">Chinese</label>
          <input class="m-1" type="radio" id="English" value="English" v-model="draft.lang">
          <label class="m-1" for="English">English</label>
          <input class="m-1" type="radio" id="Bilingual" value="Bilingual" v-model="draft.lang">
          <label class="m-1" for="Bilingual">Bilingual</label>
        </div>
        <div class="flex items-center">
          <div class="text-gray-500">Teacher</div>
          <input v-model="draft.teacher" class="all-transition border rounded m-2 px-2 focus:border-blue-500">
        </div>
        <div class="flex items-center">
          <div class="text-gray-500">Date&Time</div>
          <DatePicker class="ml-2" v-model="draft.date" mode="dateTime" is24hr :model-config="{ type: 'number' }">
            <template v-slot="{ inputValue, inputEvents }">
              <input
                class="px-2 py-1 border rounded focus:outline-none focus:border-blue-300"
                :value="inputValue"
                v-on="inputEvents"
              />
            </template>
          </DatePicker>
        </div>
        <div class="flex items-center">
          <div class="text-gray-500">Location</div>
          <select v-model="draft.location" class="m-2 text-center border rounded hover:border-blue-500 all-transition">
            <option value="Teaching Building No.1 Lecture Hall" selected>Teaching Building No.1 Lecture Hall</option>
            <option value="Research Building Lecture Hall">Research Building Lecture Hall</option>
            <option value="Library Conference Hall and Activity Room">Library Conference Hall and Activity Room</option>
          </select>
        </div>
        <div class="flex items-center">
          <div class="text-gray-500">Duration</div>
          <div class="font-mono">/hour(s)</div>
          <input v-model="draft.duration" type=number class="all-transition border rounded m-2 px-2 focus:border-blue-500 w-12">
        </div>
        <div class="flex items-center justify-center">
          <button class="all-transition px-2 text-blue-500 font-bold rounded m-1 border border-blue-500 hover:text-white hover:bg-blue-500" @click="submit">submit</button>
          <button class="all-transition px-2 text-red-500 font-bold rounded m-1 border border-red-500 hover:text-white hover:bg-red-500" v-if="!draft.new" @click="del(draft._id); emits('update:modelValue', false)">delete</button>
          <button class="all-transition px-2 text-yellow-500 font-bold rounded m-1 border border-yellow-500 hover:text-white hover:bg-yellow-500" @click="emits('update:modelValue', false)">cancel</button>
        </div>
      </div>
    </div>
  </Transition>
</template>
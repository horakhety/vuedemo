<template>
    <v-dialog v-model="dialog" max-width="30rem">

        <template v-slot:activator="{ props }">
            <v-btn v-bind="props" icon="mdi-plus"/>
        </template>

        <v-card title="New item">
            <v-form @submit.prevent="createItem()">

                <v-card-text>
                    <v-row>
                        <v-col>
                            <v-text-field
                                label="Username"
                                v-model="newItem.username" />
                        </v-col>
                    </v-row>
                    <v-row>
                        <v-col>
                            <v-text-field
                                label="Password"
                                v-model="newItem.password" />
                        </v-col>
                    </v-row>
                </v-card-text>
                <v-card-actions>
                    <v-spacer />
                    <v-btn color="primary" @click="dialog = false">Close</v-btn>
                    <v-btn color="primary" type="submit" :loading="loading">Create item</v-btn>
                </v-card-actions>
            </v-form>
        </v-card>
    </v-dialog>
</template>

<script setup lang="ts">

import { ref, reactive } from 'vue'
import { db } from '@/idb'

const dialog = ref(false)
const loading = ref(false)

const newItem = reactive({
    username: '',
    password: ''
})

const emit = defineEmits(['itemCreated'])


const createItem = async () => {
    loading.value = true
    try {
        await db.items.add({
          ...newItem, description: "", name: ""
        });
        emit('itemCreated')
        dialog.value = false
    } catch (error) {
        console.error(error)
        alert('Failed to create item')
    } finally {
        loading.value = false
    }
}

</script>

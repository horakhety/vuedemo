<template>
    <v-card>
        <v-toolbar flat>
            <v-toolbar-title>Items</v-toolbar-title>
            <v-spacer/>
            <NewItemDialog @itemCreated="getItems()"/>
        </v-toolbar>
        <v-divider />
        <v-card-text>
            <v-table :loading="loading">
                <thead>
                    <tr>
                        <th class="text-left">ID</th>
                        <th class="text-left">Username</th>
                        <th class="text-left">Password</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="item in items" :key="item._id">
                        <td>{{ item._id || 'Missing ID'}}</td>
                        <td>
                            <RouterLink :to="{name: 'item', params: {_id: item._id}}">
                                {{ item.username || 'Missing username'}}
                            </RouterLink>
                        </td>
                        <td>{{ item.password || 'Missing password'}}</td>
                    </tr>
                </tbody>
            </v-table>
        </v-card-text>
    </v-card>
</template>

<script setup lang="ts">

import NewItemDialog from '@/components/NewItemDialog.vue'

import { ref, onMounted} from 'vue'
import { db } from '@/idb'

import Item from '@/types/item'

const items = ref<Item[]>([])
const loading = ref<boolean>(false)

const getItems = async () => {

    loading.value = true
    try {
        items.value = await db.items.toArray()
    } catch (error) {
        console.error(error)
        alert('Failed to query items')
    } finally {
        loading.value = false
    }
}

onMounted( () => {
    getItems()
})

</script>

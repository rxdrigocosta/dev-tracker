<template>
    <section class="section">
        <div class="columns is-multiline">
            <div class="column is-4" v-for="(project, key) in projects" :key="key">
                <div class="box is-fullheight">
                    <div class="is-flex is-align-items-center is-justify-content-space-between">
                        <div>
                            <h2 class="title is-5">{{ project.name }}</h2>
                            <p class="subtitle is-6 has-text-grey">{{ project.description }}</p>
                        </div>
                        <div class="is-flex is-flex-direction-column is-align-items-end">
                            <router-link class="button is-primary is-small mb-1" :to="'/projects/' + project.id + '/edit'">
                                <span class="icon is-small">
                                    <font-awesome-icon icon="edit" />
                                </span>
                            </router-link>
                            <button class="button is-danger is-small" @click="removeProject(project)">
                                <span class="icon is-small">
                                    <font-awesome-icon icon="trash" />
                                </span>
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<script lang="ts" setup>
import { useStore } from '@/store'
import { computed } from 'vue'
import useNotify from '@/hooks/notifier'
import IProject from '@/interfaces/IProject'

const store = useStore()
const notify = useNotify()

store.dispatch('fetchProjects')

const projects = computed<IProject[]>(() => store.getters.projects)

const removeProject = (project: IProject): void => {
    store.dispatch('removeProject', project)
        .then(() => {
            notify.success('Project removed successfully')
        }).catch((error) => {
            notify.error(error.message)
        })
}
</script>

<style scoped>
.is-fullheight {
    height: 100%;
}
</style>
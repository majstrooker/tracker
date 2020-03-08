<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
        />

        <q-toolbar-title>
          Tracker
        </q-toolbar-title>

        <div>v{{ trackerVersion }}</div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      content-class="bg-grey-1"
    >
      <q-list>
        <q-item-label
          header
          class="text-grey-8"
        >
          Menu
        </q-item-label>
        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import EssentialLink from 'components/EssentialLink'

export default {
  name: 'MainLayout',

  components: {
    EssentialLink
  },

  data () {
    return {
      leftDrawerOpen: false,
      trackerVersion: '1.0.0',
      essentialLinks: [
        {
          title: 'Docs',
          caption: 'Tracker help documentation',
          icon: 'school',
          link: ''
        },
        {
          title: 'Projects',
          caption: 'VO Projects',
          icon: 'project',
          link: ''
        },
        {
          title: 'Vessels',
          caption: 'All tracked vessels',
          icon: 'vessel',
          link: ''
        }
      ]
    }
  }
}
</script>

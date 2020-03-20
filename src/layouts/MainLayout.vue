<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>

        <q-toolbar-title>
          Слежу за свои капиталом
        </q-toolbar-title>

        <div>Quasar v{{ 1 }}</div>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>

export default {
  name: 'MainLayout',


  data () {
    return {

    }
  }
}
</script>

<template>
  <q-layout view="lHr lpR lfr">
    <q-header class="bg-primary text-white" height-hint="98">
      <q-toolbar>
        <q-toolbar-title>UAS-Praktikum</q-toolbar-title>
        <q-tabs align="left">
          <q-route-tab to="/" label="weather" />
        </q-tabs>
      </q-toolbar>
    </q-header>
    <q-page-container>
      <router-view />
    </q-page-container>
    <q-footer class="bg-primary text-white" height-hint="98">
      <q-toolbar>
        <q-toolbar-title>
          Copyright © 2022 FadhlurRohman 223510401 ®. All rights reserved.
        </q-toolbar-title>
      </q-toolbar>
    </q-footer>
  </q-layout>
</template>

<script setup></script>

<style>
.q-page-container {
  background-color: #83b4ff;
}
.q-toolbar {
  padding: 0 75px !important;
}
.q-toolbar__title:first-child {
  padding-left: 32px !important;
}
.q-tab {
  padding: 0 20px !important;
  min-height: 75px !important;
}
</style>

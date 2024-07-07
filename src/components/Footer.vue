<!-- src/components/Footer.vue -->
<template>
  <footer class="footer">
    <div class="footer-content">
      <p>© 2024 To do list. Todos os direitos reservados.</p>
      <div class="icon-container">
        <a v-for="(icon, index) in icons" :key="index" :href="icon.link" target="_blank" class="icon-link">
          <v-icon class="mr-4">{{ icon.name }}</v-icon>
        </a>
      </div>
    </div>
  </footer>
</template>

<script>
export default {
  data() {
    return {
      icons: [
        { name: 'mdi-email-outline', link: 'mailto:offlinematheus@gmail.com' },
        { name: 'mdi-instagram', link: 'https://www.instagram.com/aquino_zn/' },
        { name: 'mdi-github', link: 'https://github.com/aquinodev2022' }
      ]
    }
  }
}
</script>

<style scoped>
.footer {
  background-color: #43A047;
  color: #fff;
  text-align: center;
  padding: 30px;
}

.footer-content {
  display: flex;
  align-items: center;
  justify-content: center;
}

.icon-container {
  display: flex;
  margin-left: 25px;
}

.icon-container .icon-link {
  display: flex;
  align-items: center;
  text-decoration: none;
}

.icon-container .v-icon {
  margin-right: 16px;
  color: #fff;
}
</style>

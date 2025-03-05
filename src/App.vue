<template>
  <div class="container">
    <h1 class="title">
      Centro de Notificaciones
    </h1>

    <div class="notifications-panel">
      <div class="notification" v-for="(notification, index) in notifications" :key="index">
        <div class="notification-content">
          <span class="notification-message">{{ notification.message }}</span>
          <span class="notification-time">{{ notification.time }}</span>
        </div>
      </div>
      <div v-if="notifications.length === 0" class="no-notifications">
        No hay notificaciones nuevas
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { io } from 'socket.io-client';

const notifications = ref([]);

const mySucursal = ref('4');
const myUser = ref('6');

onMounted(() => {
  const socket = io('http://localhost:3000');

  socket.on('newNotification', (notification) => {
    if (notification.id_sucursal === mySucursal.value && notification.rol_to_notify === myUser.value) {
      notifications.value.unshift({
        message: notification.case_id,
        time: new Date().toLocaleTimeString()
      });
    }
  });
});
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
}

.title {
  color: #2c3e50;
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}

.notifications-panel {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
  padding: 1rem;
  max-height: 500px;
  overflow-y: auto;
}

.notification {
  padding: 1rem;
  border-bottom: 1px solid #eee;
  transition: background-color 0.3s ease;
}

.notification:last-child {
  border-bottom: none;
}

.notification:hover {
  background-color: #f8f9fa;
}

.notification-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.notification-message {
  color: #2c3e50;
  font-size: 1rem;
}

.notification-time {
  color: #6c757d;
  font-size: 0.875rem;
}

.no-notifications {
  text-align: center;
  color: #6c757d;
  padding: 2rem;
  font-style: italic;
}
</style>

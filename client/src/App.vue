<template>
  <div class="app">
    <aside class="sidebar" :class="{ collapsed: sidebarCollapsed }">

      <!-- Brand -->
      <div class="sidebar-brand">
        <div class="brand-mark">
          <svg width="28" height="28" viewBox="0 0 28 28" fill="none">
            <rect width="28" height="28" rx="6" fill="#2563eb"/>
            <path d="M7 14h14M14 7v14" stroke="white" stroke-width="2" stroke-linecap="round"/>
          </svg>
        </div>
        <span class="brand-name" v-show="!sidebarCollapsed">{{ t('nav.companyName') }}</span>
        <button class="collapse-btn" @click="sidebarCollapsed = !sidebarCollapsed" :title="sidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'">
          <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
            <path
              :d="sidebarCollapsed ? 'M6 3l5 5-5 5' : 'M10 3L5 8l5 5'"
              stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"
            />
          </svg>
        </button>
      </div>

      <!-- Nav links -->
      <nav class="sidebar-nav">
        <router-link to="/">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none">
            <rect x="1.5" y="1.5" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <rect x="10.5" y="1.5" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <rect x="1.5" y="10.5" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
            <rect x="10.5" y="10.5" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
          </svg>
          <span v-show="!sidebarCollapsed">{{ t('nav.overview') }}</span>
        </router-link>
        <router-link to="/inventory">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none">
            <path d="M2.25 5.25h13.5v10.5a1.5 1.5 0 01-1.5 1.5H3.75a1.5 1.5 0 01-1.5-1.5V5.25z" stroke="currentColor" stroke-width="1.5"/>
            <path d="M1.5 2.25h15a.75.75 0 01.75.75v2.25H.75V3a.75.75 0 01.75-.75z" stroke="currentColor" stroke-width="1.5"/>
            <path d="M7.5 9h3" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span v-show="!sidebarCollapsed">{{ t('nav.inventory') }}</span>
        </router-link>
        <router-link to="/orders">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none">
            <path d="M6 2.25h6a.75.75 0 01.75.75v.75H5.25V3A.75.75 0 016 2.25z" stroke="currentColor" stroke-width="1.5"/>
            <path d="M3.75 3.75H2.25A1.5 1.5 0 00.75 5.25v10.5A1.5 1.5 0 002.25 17.25h13.5A1.5 1.5 0 0017.25 15.75V5.25a1.5 1.5 0 00-1.5-1.5H14.25" stroke="currentColor" stroke-width="1.5"/>
            <path d="M5.25 9h7.5M5.25 12h5.25" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span v-show="!sidebarCollapsed">{{ t('nav.orders') }}</span>
        </router-link>
        <router-link to="/spending">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none">
            <rect x="0.75" y="3.75" width="16.5" height="10.5" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
            <circle cx="9" cy="9" r="2.25" stroke="currentColor" stroke-width="1.5"/>
            <path d="M3.75 6.75v4.5M14.25 6.75v4.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span v-show="!sidebarCollapsed">{{ t('nav.finance') }}</span>
        </router-link>
        <router-link to="/demand">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none">
            <path d="M1.5 13.5l4.5-4.5 3 3 5.25-6 2.25 2.25" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M1.5 16.5h15" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span v-show="!sidebarCollapsed">{{ t('nav.demandForecast') }}</span>
        </router-link>
        <router-link to="/reports">
          <svg width="18" height="18" viewBox="0 0 18 18" fill="none">
            <path d="M10.5 1.5H4.5A1.5 1.5 0 003 3v12a1.5 1.5 0 001.5 1.5h9A1.5 1.5 0 0015 15V6l-4.5-4.5z" stroke="currentColor" stroke-width="1.5"/>
            <path d="M10.5 1.5V6H15" stroke="currentColor" stroke-width="1.5"/>
            <path d="M6 12.75v-2.25M9 12.75V9M12 12.75v-1.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
          </svg>
          <span v-show="!sidebarCollapsed">Reports</span>
        </router-link>
      </nav>

      <!-- Filters (hidden when collapsed) -->
      <div class="sidebar-filters" v-show="!sidebarCollapsed">
        <FilterBar />
      </div>

      <!-- Footer: language + profile -->
      <div class="sidebar-footer">
        <LanguageSwitcher v-show="!sidebarCollapsed" />
        <ProfileMenu
          @show-profile-details="showProfileDetails = true"
          @show-tasks="showTasks = true"
        />
      </div>

    </aside>

    <main class="main-content">
      <router-view />
    </main>

    <ProfileDetailsModal
      :is-open="showProfileDetails"
      @close="showProfileDetails = false"
    />

    <TasksModal
      :is-open="showTasks"
      :tasks="tasks"
      @close="showTasks = false"
      @add-task="addTask"
      @delete-task="deleteTask"
      @toggle-task="toggleTask"
    />
  </div>
</template>

<script>
import { ref, onMounted, computed, watch } from 'vue'
import { api } from './api'
import { useAuth } from './composables/useAuth'
import { useI18n } from './composables/useI18n'
import FilterBar from './components/FilterBar.vue'
import ProfileMenu from './components/ProfileMenu.vue'
import ProfileDetailsModal from './components/ProfileDetailsModal.vue'
import TasksModal from './components/TasksModal.vue'
import LanguageSwitcher from './components/LanguageSwitcher.vue'

export default {
  name: 'App',
  components: {
    FilterBar,
    ProfileMenu,
    ProfileDetailsModal,
    TasksModal,
    LanguageSwitcher
  },
  setup() {
    const { currentUser } = useAuth()
    const { t } = useI18n()
    const showProfileDetails = ref(false)
    const showTasks = ref(false)
    const apiTasks = ref([])

    const sidebarCollapsed = ref(
      localStorage.getItem('sidebar-collapsed') === 'true'
    )
    watch(sidebarCollapsed, val => localStorage.setItem('sidebar-collapsed', String(val)))

    // Merge mock tasks from currentUser with API tasks
    const tasks = computed(() => {
      return [...currentUser.value.tasks, ...apiTasks.value]
    })

    const loadTasks = async () => {
      try {
        apiTasks.value = await api.getTasks()
      } catch (err) {
        console.error('Failed to load tasks:', err)
      }
    }

    const addTask = async (taskData) => {
      try {
        const newTask = await api.createTask(taskData)
        // Add new task to the beginning of the array
        apiTasks.value.unshift(newTask)
      } catch (err) {
        console.error('Failed to add task:', err)
      }
    }

    const deleteTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const isMockTask = currentUser.value.tasks.some(t => t.id === taskId)

        if (isMockTask) {
          // Remove from mock tasks
          const index = currentUser.value.tasks.findIndex(t => t.id === taskId)
          if (index !== -1) {
            currentUser.value.tasks.splice(index, 1)
          }
        } else {
          // Remove from API tasks
          await api.deleteTask(taskId)
          apiTasks.value = apiTasks.value.filter(t => t.id !== taskId)
        }
      } catch (err) {
        console.error('Failed to delete task:', err)
      }
    }

    const toggleTask = async (taskId) => {
      try {
        // Check if it's a mock task (from currentUser)
        const mockTask = currentUser.value.tasks.find(t => t.id === taskId)

        if (mockTask) {
          // Toggle mock task status
          mockTask.status = mockTask.status === 'pending' ? 'completed' : 'pending'
        } else {
          // Toggle API task
          const updatedTask = await api.toggleTask(taskId)
          const index = apiTasks.value.findIndex(t => t.id === taskId)
          if (index !== -1) {
            apiTasks.value[index] = updatedTask
          }
        }
      } catch (err) {
        console.error('Failed to toggle task:', err)
      }
    }

    onMounted(loadTasks)

    return {
      t,
      sidebarCollapsed,
      showProfileDetails,
      showTasks,
      tasks,
      addTask,
      deleteTask,
      toggleTask
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
  background: #f8fafc;
  color: #1e293b;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.app {
  display: flex;
  flex-direction: row;
  min-height: 100vh;
  background: #f8fafc;
}

.sidebar {
  width: 240px;
  min-height: 100vh;
  height: 100vh;
  background: #ffffff;
  border-right: 1px solid #e2e8f0;
  display: flex;
  flex-direction: column;
  position: sticky;
  top: 0;
  overflow-y: auto;
  overflow-x: hidden;
  flex-shrink: 0;
  transition: width 0.2s ease;
  z-index: 100;
}

.sidebar.collapsed {
  width: 56px;
}

.sidebar-brand {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 1.25rem 1rem;
  border-bottom: 1px solid #f1f5f9;
  min-height: 64px;
}

.sidebar.collapsed .sidebar-brand {
  justify-content: center;
  padding: 1.25rem 0;
}

.brand-mark {
  flex-shrink: 0;
  display: flex;
  align-items: center;
}

.brand-name {
  font-size: 0.9375rem;
  font-weight: 700;
  color: #0f172a;
  white-space: nowrap;
  overflow: hidden;
  flex: 1;
}

.collapse-btn {
  flex-shrink: 0;
  width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: none;
  border: 1px solid #e2e8f0;
  border-radius: 4px;
  color: #64748b;
  cursor: pointer;
  padding: 0;
  transition: all 0.15s ease;
  margin-left: auto;
}

.collapse-btn:hover {
  background: #f1f5f9;
  color: #0f172a;
}

.sidebar.collapsed .collapse-btn {
  margin-left: 0;
}

.sidebar-nav {
  padding: 0.75rem 0.5rem;
  flex: 0 0 auto;
}

.sidebar-nav a {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.625rem 0.75rem;
  border-radius: 6px;
  color: #64748b;
  text-decoration: none;
  font-size: 0.875rem;
  font-weight: 500;
  transition: all 0.15s ease;
  white-space: nowrap;
  margin-bottom: 2px;
}

.sidebar-nav a:hover {
  background: #f1f5f9;
  color: #0f172a;
}

.sidebar-nav a.router-link-active,
.sidebar-nav a.active {
  background: #eff6ff;
  color: #2563eb;
}

.sidebar-nav a svg {
  flex-shrink: 0;
}

.sidebar.collapsed .sidebar-nav a {
  justify-content: center;
  padding: 0.625rem;
  gap: 0;
}

.sidebar-filters {
  padding: 0.75rem 0.5rem 0;
  border-top: 1px solid #f1f5f9;
  flex: 1 1 auto;
  overflow-y: auto;
}

.sidebar-footer {
  margin-top: auto;
  padding: 0.75rem 0.5rem;
  border-top: 1px solid #f1f5f9;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.main-content {
  flex: 1;
  min-width: 0;
  padding: 1.5rem 2rem;
  overflow: auto;
}

.page-header {
  margin-bottom: 1.5rem;
}

.page-header h2 {
  font-size: 1.875rem;
  font-weight: 700;
  color: #0f172a;
  margin-bottom: 0.375rem;
  letter-spacing: -0.025em;
}

.page-header p {
  color: #64748b;
  font-size: 0.938rem;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.25rem;
  margin-bottom: 1.5rem;
}

.stat-card {
  background: white;
  padding: 1.25rem;
  border-radius: 10px;
  border: 1px solid #e2e8f0;
  transition: all 0.2s ease;
}

.stat-card:hover {
  border-color: #cbd5e1;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
}

.stat-label {
  color: #64748b;
  font-size: 0.875rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 0.625rem;
}

.stat-value {
  font-size: 2.25rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
}

.stat-card.warning .stat-value {
  color: #ea580c;
}

.stat-card.success .stat-value {
  color: #059669;
}

.stat-card.danger .stat-value {
  color: #dc2626;
}

.stat-card.info .stat-value {
  color: #2563eb;
}

.card {
  background: white;
  border-radius: 10px;
  padding: 1.25rem;
  border: 1px solid #e2e8f0;
  margin-bottom: 1.25rem;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  padding-bottom: 0.875rem;
  border-bottom: 1px solid #e2e8f0;
}

.card-title {
  font-size: 1.125rem;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
}

.table-container {
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

thead {
  background: #f8fafc;
  border-top: 1px solid #e2e8f0;
  border-bottom: 1px solid #e2e8f0;
}

th {
  text-align: left;
  padding: 0.5rem 0.75rem;
  font-weight: 600;
  color: #475569;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

td {
  padding: 0.5rem 0.75rem;
  border-top: 1px solid #f1f5f9;
  color: #334155;
  font-size: 0.875rem;
}

tbody tr {
  transition: background-color 0.15s ease;
}

tbody tr:hover {
  background: #f8fafc;
}

.badge {
  display: inline-block;
  padding: 0.313rem 0.75rem;
  border-radius: 6px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.025em;
}

.badge.success {
  background: #d1fae5;
  color: #065f46;
}

.badge.warning {
  background: #fed7aa;
  color: #92400e;
}

.badge.danger {
  background: #fecaca;
  color: #991b1b;
}

.badge.info {
  background: #dbeafe;
  color: #1e40af;
}

.badge.increasing {
  background: #d1fae5;
  color: #065f46;
}

.badge.decreasing {
  background: #fecaca;
  color: #991b1b;
}

.badge.stable {
  background: #e0e7ff;
  color: #3730a3;
}

.badge.high {
  background: #fecaca;
  color: #991b1b;
}

.badge.medium {
  background: #fed7aa;
  color: #92400e;
}

.badge.low {
  background: #dbeafe;
  color: #1e40af;
}

.loading {
  text-align: center;
  padding: 3rem;
  color: #64748b;
  font-size: 0.938rem;
}

.error {
  background: #fef2f2;
  border: 1px solid #fecaca;
  color: #991b1b;
  padding: 1rem;
  border-radius: 8px;
  margin: 1rem 0;
  font-size: 0.938rem;
}
</style>

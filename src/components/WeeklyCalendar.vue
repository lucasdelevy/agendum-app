<template>
  <div class="weekly-calendar">
    <div class="calendar-header">
      <button @click="previousWeek" class="nav-arrow">‹</button>
      <h2>Week {{ weekNumber }} - {{ monthYear }}</h2>
      <button @click="nextWeek" class="nav-arrow">›</button>
    </div>
    <div class="calendar-container">
      <div class="time-column">
        <div class="day-header"></div>
        <div v-for="hour in hours" :key="hour" class="time-slot">{{ hour }}</div>
      </div>
      <div class="days-grid">
        <div class="day-headers">
          <div v-for="day in weekDays" :key="day.date" class="day-header" :class="{ 'current-day-header': day.isToday }">
            <div class="day-name">{{ day.dayName }}</div>
            <div class="day-number">{{ day.dayNumber }}</div>
          </div>
        </div>
        <div class="calendar-body">
          <div v-for="day in weekDays" :key="day.date" class="day-column" :class="{ 'current-day-column': day.isToday }" style="position: relative;">
            <div v-for="hour in hours" :key="hour" class="hour-cell"></div>
            <div class="current-time-bar" :style="{ top: currentTimePosition + 'px' }"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'WeeklyCalendar',
  data() {
    return {
      currentDate: new Date()
    }
  },
  computed: {
    weekDays() {
      const days = []
      const startOfWeek = this.getStartOfWeek(this.currentDate)
      const today = new Date()
      
      for (let i = 0; i < 7; i++) {
        const date = new Date(startOfWeek)
        date.setDate(startOfWeek.getDate() + i)
        
        days.push({
          date: date.toISOString(),
          dayName: date.toLocaleDateString('en-US', { weekday: 'short' }),
          dayNumber: date.getDate(),
          isToday: date.toDateString() === today.toDateString()
        })
      }
      return days
    },
    hours() {
      const hours = []
      for (let i = 0; i < 24; i++) {
        hours.push(i.toString().padStart(2, '0') + ':00')
      }
      return hours
    },
    currentHour() {
      return new Date().getHours()
    },
    currentTimePosition() {
      const now = new Date()
      const hours = now.getHours()
      const minutes = now.getMinutes()
      const hourHeight = 40
      return (hours * hourHeight) + (minutes / 60 * hourHeight)
    },
    weekNumber() {
      return this.getWeekNumber(this.currentDate)
    },
    monthYear() {
      return this.currentDate.toLocaleDateString('en-US', { month: 'long', year: 'numeric' })
    }
  },
  methods: {
    getStartOfWeek(date) {
      const d = new Date(date)
      const day = d.getDay()
      const diff = d.getDate() - day
      return new Date(d.setDate(diff))
    },
    getWeekNumber(date) {
      const d = new Date(date)
      d.setHours(0, 0, 0, 0)
      d.setDate(d.getDate() + 3 - (d.getDay() + 6) % 7)
      const week1 = new Date(d.getFullYear(), 0, 4)
      return 1 + Math.round(((d - week1) / 86400000 - 3 + (week1.getDay() + 6) % 7) / 7)
    },
    previousWeek() {
      this.currentDate.setDate(this.currentDate.getDate() - 7)
      this.currentDate = new Date(this.currentDate)
    },
    nextWeek() {
      this.currentDate.setDate(this.currentDate.getDate() + 7)
      this.currentDate = new Date(this.currentDate)
    },

  }
}
</script>

<style scoped>
.weekly-calendar {
  max-width: 1200px;
  margin: 0 auto;
}

.calendar-header {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
  gap: 20px;
}

.nav-arrow {
  background: #007bff;
  color: white;
  border: none;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  font-size: 20px;
  cursor: pointer;
}

.nav-arrow:hover {
  background: #0056b3;
}

.calendar-container {
  display: flex;
  border: 1px solid #ddd;
  background: white;
  border-radius: 8px;
  overflow: hidden;
}

.time-column {
  width: 80px;
  border-right: 1px solid #eee;
}

.time-column .day-header {
  height: 60px;
  border-bottom: 1px solid #eee;
}

.time-slot {
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;
  color: #666;
  border-bottom: 1px solid #f5f5f5;
}

.days-grid {
  flex: 1;
}

.day-headers {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}

.day-header {
  height: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border-right: 1px solid #eee;
  border-bottom: 1px solid #eee;
}

.current-day-header {
  background: #e3f2fd;
}

.day-name {
  font-weight: bold;
  color: #666;
  font-size: 12px;
}

.day-number {
  font-size: 16px;
  font-weight: bold;
}

.current-day-header .day-number {
  color: #1976d2;
}

.calendar-body {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}

.day-column {
  border-right: 1px solid #eee;
}

.current-day-column {
  background: #f0f8ff;
}

.hour-cell {
  height: 40px;
  border-bottom: 1px solid #f5f5f5;
}

.current-time-bar {
  position: absolute;
  left: 0;
  right: 0;
  height: 1px;
  border-top: 1px dashed #dc3545;
  z-index: 10;
  opacity: 0.7;
}
</style>
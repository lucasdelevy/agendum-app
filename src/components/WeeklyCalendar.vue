<template>
  <div class="weekly-calendar">
    <div class="calendar-header">
      <button @click="previousWeek" class="nav-arrow">‹</button>
      <h2>Week {{ weekNumber }} - {{ monthYear }}</h2>
      <button @click="nextWeek" class="nav-arrow">›</button>
    </div>
    <div class="calendar-grid">
      <div v-for="day in weekDays" :key="day.date" class="day-cell" :class="{ 'current-day': day.isToday }">
        <div class="day-name">{{ day.dayName }}</div>
        <div class="day-number">{{ day.dayNumber }}</div>
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
    }
  }
}
</script>

<style scoped>
.weekly-calendar {
  max-width: 800px;
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

.calendar-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 1px;
  border: 1px solid #ddd;
}

.day-cell {
  background: white;
  border: 1px solid #eee;
  padding: 15px;
  text-align: center;
  min-height: 80px;
}

.day-name {
  font-weight: bold;
  color: #666;
  margin-bottom: 5px;
}

.day-number {
  font-size: 18px;
  font-weight: bold;
}

.current-day {
  background: #e3f2fd !important;
  border-color: #2196f3 !important;
}

.current-day .day-number {
  color: #1976d2;
}
</style>
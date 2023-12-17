<script setup lang="ts">
import {computed, ref} from "vue";

interface TimeTableEntry {
  [hour: number]: number[];
}

interface DailySchedule {
  [dayType: string]: TimeTableEntry;
}

interface TimeTable {
  [station: string]: DailySchedule;
}

const timeTable = ref<TimeTable>({
  "横関": {
    "平日": {
      5: [17, 56],
      6: [6, 12, 16, 26, 36, 39, 40, 46, 56],
      7: [2, 4, 9, 11, 19, 24, 29, 34, 36, 40, 43, 58],
      8: [0, 8, 13, 18, 30, 37, 50, 55],
      9: [0, 5, 15, 25, 39, 40, 48, 55],
      10: [8, 10, 15, 25, 35, 39, 48, 55],
      11: [10, 15, 25, 40, 49, 55],
      12: [10, 25, 40, 46, 55],
      13: [10, 25, 40, 51, 55],
      14: [10, 25, 27, 40, 55],
      15: [10, 25, 40, 55],
      16: [0, 5, 15, 18, 30, 45, 49, 55],
      17: [8, 15, 25, 30, 40, 49, 55],
      18: [0, 5, 10, 15, 20, 25, 30, 40, 50, 55],
      19: [15, 20, 25, 35, 48, 55],
      20: [10, 15, 25, 40, 50],
      21: [9, 20, 40],
      22: [5, 35, 50],
    },
    "土曜日": {
      5: [],
      6: [11, 25, 41, 50, 56],
      7: [10, 20, 30, 35, 40, 50],
      8: [0, 7, 15, 20, 30, 40, 50, 55],
      9: [5, 10, 30, 40, 50],
      10: [0, 10, 20, 30, 40, 50, 55],
      11: [0, 10, 25, 33, 40, 55],
      12: [10, 25, 33, 40, 55],
      13: [10, 25, 33, 40, 55],
      14: [10, 25, 40, 55],
      15: [10, 25, 40, 55],
      16: [6, 10, 25, 40, 55],
      17: [10, 25, 33, 40, 55],
      18: [10, 25, 32, 40, 55],
      19: [12, 24, 29, 47],
      20: [12, 24, 35],
      21: [6, 35],
    },
    "日祝日": {
      5: [],
      6: [11, 26, 41, 50, 56],
      7: [10, 20, 30, 35, 40, 50],
      8: [0, 7, 15, 20, 30, 40, 50, 55],
      9: [5, 10, 30, 40, 50],
      10: [0, 10, 30, 40, 50],
      11: [0, 10, 25, 33, 40, 55],
      12: [10, 25, 33, 40, 55],
      13: [10, 25, 33, 40, 55],
      14: [10, 25, 33, 40, 55],
      15: [10, 25, 40, 55],
      16: [6, 10, 25, 40, 55],
      17: [10, 25, 33, 40, 55],
      18: [10, 25, 32, 40, 55],
      19: [12, 24, 29, 47],
      20: [12, 24, 35],
      21: [6, 35],
    },
  },
  "横関東口": {
    "平日": {
      5: [],
      6: [23, 55],
      7: [10, 37],
      8: [7, 28, 47],
      9: [2, 18, 51],
      10: [11, 36, 51],
      11: [11, 36, 51],
      12: [11, 36, 51],
      13: [5, 11, 36, 51],
      14: [11, 36, 51],
      15: [11, 36, 51],
      16: [11, 31, 51],
      17: [5, 12, 47],
      18: [22, 42],
      19: [2, 17, 57],
      20: [21, 52],
      21: [21],
    },
    "土曜日": {
      5: [],
      6: [55],
      7: [8, 37, 57],
      8: [17, 37, 57],
      9: [17, 37, 51],
      10: [11, 37, 51],
      11: [11, 37, 51],
      12: [11, 37, 51],
      13: [11, 37, 51],
      14: [11, 37, 51],
      15: [11, 37, 51],
      16: [11, 37, 51],
      17: [12, 37, 52],
      18: [17, 37, 52],
      19: [17, 37],
      20: [7, 37],
      21: [7],
    },
    "日祝日": {
      5: [],
      6: [55],
      7: [8, 37, 57],
      8: [17, 37, 57],
      9: [17, 37, 51],
      10: [11, 17, 37, 42, 51],
      11: [11, 12, 37, 42, 51],
      12: [11, 12, 37, 42, 51],
      13: [11, 12, 37, 42, 51],
      14: [11, 12, 37, 42, 51],
      15: [11, 12, 37, 42, 51],
      16: [11, 12, 37, 42, 51],
      17: [12, 12, 37, 42, 52],
      18: [12, 17, 37, 42, 52],
      19: [17, 37, 42],
      20: [7, 37],
      21: [7],
    },
  },
})
const kind = ["土曜日", "日祝日", "平日"]
const H = computed(() => {
  return (new Date()).getHours()
})
const nextH = computed(() => {
  return H.value + 1;
})
const Hs = computed(() => {
  return [H.value, nextH.value]
})
const reload = () => {
  window.location.reload();
}

function getTimetableForHours(station: string, dayType: string, hours: number[]) {
  const stationTimetable = timeTable.value[station][dayType];
  const filteredTimetable: TimeTableEntry = {};

  hours.forEach(hour => {
    if (stationTimetable[hour]) {
      filteredTimetable[hour] = stationTimetable[hour];
    }
  });

  return filteredTimetable;
}

const yokoHeiH = computed(() => {
  return getTimetableForHours("横関", "平日", Hs.value)
})
const hHeiH = computed(() => {
  return getTimetableForHours("横関東口", "平日", Hs.value)
})
const yokoDoH = computed(() => {
  return getTimetableForHours("横関", "土曜日", Hs.value)
})
const hDoH = computed(() => {
  return getTimetableForHours("横関東口", "土曜日", Hs.value)
})
const yokoNiH = computed(() => {
  return getTimetableForHours("横関", "日祝日", Hs.value)
})
const hNiH = computed(() => {
  return getTimetableForHours("横関東口", "日祝日", Hs.value)
})

</script>

<template>
  <div class="wrapper">
    <h1>
      <button @click="reload">
        {{ (new Date()).toLocaleTimeString() }}
        <svg xmlns="http://www.w3.org/2000/svg" height="24" width="24" viewBox="0 0 512 512"><!--!Font Awesome Free 6.5.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license/free Copyright 2023 Fonticons, Inc.--><path d="M142.9 142.9c62.2-62.2 162.7-62.5 225.3-1L327 183c-6.9 6.9-8.9 17.2-5.2 26.2s12.5 14.8 22.2 14.8H463.5c0 0 0 0 0 0H472c13.3 0 24-10.7 24-24V72c0-9.7-5.8-18.5-14.8-22.2s-19.3-1.7-26.2 5.2L413.4 96.6c-87.6-86.5-228.7-86.2-315.8 1C73.2 122 55.6 150.7 44.8 181.4c-5.9 16.7 2.9 34.9 19.5 40.8s34.9-2.9 40.8-19.5c7.7-21.8 20.2-42.3 37.8-59.8zM16 312v7.6 .7V440c0 9.7 5.8 18.5 14.8 22.2s19.3 1.7 26.2-5.2l41.6-41.6c87.6 86.5 228.7 86.2 315.8-1c24.4-24.4 42.1-53.1 52.9-83.7c5.9-16.7-2.9-34.9-19.5-40.8s-34.9 2.9-40.8 19.5c-7.7 21.8-20.2 42.3-37.8 59.8c-62.2 62.2-162.7 62.5-225.3 1L185 329c6.9-6.9 8.9-17.2 5.2-26.2s-12.5-14.8-22.2-14.8H48.4h-.7H40c-13.3 0-24 10.7-24 24z"/></svg>
      </button>
    </h1>
    <div>
      <h2>平日</h2>
      <table>
        <tr>
          <th>H</th>
          <th>横関</th>
          <th>横関東口</th>
        </tr>
        <tr v-for="(row,i) in yokoHeiH" key="i">
          <td>{{ i }}</td>
          <td>{{ row.join(" ") }}</td>
          <td>{{ hHeiH ? hHeiH[i].join(" ") : "" }}</td>
        </tr>
      </table>
    </div>
    <div>
      <h2>土曜日</h2>
      <table>
        <tr>
          <th>H</th>
          <th>横関</th>
          <th>横関東口</th>
        </tr>
        <tr v-for="(row,i) in yokoDoH" key="i">
          <td>{{ i }}</td>
          <td>{{ row.join(" ") }}</td>
          <td>{{ hDoH ? hDoH[i].join(" ") : "" }}</td>
        </tr>
      </table>
    </div>
    <div>
      <h2>日祝日</h2>
      <table>
        <tr>
          <th>H</th>
          <th>横関</th>
          <th>横関東口</th>
        </tr>
        <tr v-for="(row,i) in yokoNiH" key="i">
          <td>{{ i }}</td>
          <td>{{ row.join(" ") }}</td>
          <td>{{ hNiH ? hNiH[i].join(" ") : "" }}</td>
        </tr>
      </table>
    </div>
    <div>
      <hr/>
      <h1>全体</h1>
    </div>
    <div v-for="(w,i) in kind" key="i">
      <h2>{{w}}</h2>
      <table>
        <tr>
          <th>H</th>
          <th>横関</th>
          <th>横関東口</th>
        </tr>
        <tr v-for="(r,x) in timeTable['横関'][w]" key="x">
          <td>{{ x }}</td>
          <td>{{ r.join(" ") }}</td>
          <td>{{ timeTable['横関東口'][w][x] ? timeTable['横関東口'][w][x].join(" ") : "" }}</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<style scoped>
body {
  background-color: #181818;
  color: white;
}
table {
  border-collapse: collapse;
  width: 100%;
  border-spacing: 0;
}
th, td {
  border: 1px solid white;
  font-size: 1rem;
  padding: 0.5rem;
}
button {
  font-size: 2rem;
  padding: 0.5rem;
  border-radius: 8px;
}
</style>

<template>
  <q-page class="column items-center justify-center q-pa-md">
    <div class="tasks-section">
      <q-select
        v-model="selectedTask"
        :options="tasks"
        label="Select Task"
        emit-value
        map-options
        color="primary"
        class="task-select"
      />
      <q-btn
        @click="navigateToTask"
        label="Cek Tugas"
        color="primary"
        class="task-btn"
      />
    </div>
    <div class="input-section">
      <q-input v-model="newLocation" label="Enter Location" />
      <q-btn @click="addWeatherWidget" label="Add Widget" color="primary" />
      <q-select
        v-model="widgetStyle"
        :options="['Detailed', 'Compact']"
        label="Widget Style"
        color="primary"
        style="margin-left: 10px"
      />
    </div>
    <div class="widgets-section">
      <div
        v-for="(weather, index) in weatherStore.weatherWidgets"
        :key="index"
        :class="[
          'weather-widget',
          widgetStyle ? widgetStyle.toLowerCase() : '',
        ]"
      >
        <q-btn
          @click="removeWidget(index)"
          icon="close"
          color="negative"
          size="xs"
          class="remove-btn"
        />
        <div v-if="widgetStyle === 'Detailed'" class="detailed-widget">
          <div class="current-weather">
            <div class="location">
              <h2>{{ weather.name }} Weather</h2>
            </div>
            <div class="current-details">
              <q-icon
                :name="getWeatherIcon(weather.weather[0].main)"
                size="80px"
              />
              <div class="temp">{{ convertTemp(weather.main.temp) }}</div>
              <div class="description">
                {{ weather.weather[0].description }}
              </div>
            </div>
          </div>
        </div>
        <div v-else class="compact-widget">
          <q-icon :name="getWeatherIcon(weather.weather[0].main)" size="40px" />
          <div class="location">{{ weather.name }}</div>
          <div class="temp">{{ convertTemp(weather.main.temp) }}</div>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from "vue";
import { QInput, QBtn, QIcon, QSelect } from "quasar";
import { useWeatherStore } from "../stores/weatherStore";

const weatherStore = useWeatherStore();

const newLocation = ref("");
const widgetStyle = ref("Detailed");
const selectedTask = ref(null);

const tasks = ref([
  { label: "Tugas 1", value: "https://adli.netlify.app/" },
  {
    label: "Tugas 2",
    value: "https://tugas2fadhlur.netlify.app/",
  },
  { label: "Tugas 3", value: "https://223510401-fadhlur.netlify.app/" },
  {
    label: "Tugas 4",
    value: "https://tugas4prakpbk.netlify.app/",
  },
  { label: "Tugas 5", value: "https://pbk-lurrr.netlify.app/" },
  {
    label: "Tugas 6",
    value: "https://tugas6.netlify.app/",
  },
]);

const weatherIconMapping = {
  Clear: "wb_sunny",
  Clouds: "cloud",
  Rain: "grain",
  Drizzle: "grain",
  Thunderstorm: "flash_on",
  Snow: "ac_unit",
  Mist: "blur_on",
  Smoke: "blur_on",
  Haze: "blur_on",
  Dust: "blur_on",
  Fog: "blur_on",
  Sand: "blur_on",
  Ash: "blur_on",
  Squall: "blur_on",
  Tornado: "toys",
};

const convertTemp = (tempInCelsius) => {
  return `${tempInCelsius.toFixed(1)} °C / ${(
    (tempInCelsius * 9) / 5 +
    32
  ).toFixed(1)} °F`;
};

const getWeatherIcon = (weatherMain) => {
  return weatherIconMapping[weatherMain] || "wb_cloudy";
};

const addWeatherWidget = async () => {
  if (newLocation.value.trim() !== "") {
    await weatherStore.fetchWeather(newLocation.value);
    newLocation.value = "";
  }
};

const removeWidget = (index) => {
  weatherStore.removeWidget(index);
};

const navigateToTask = () => {
  if (selectedTask.value) {
    window.location.href = selectedTask.value;
  }
};
</script>

<style scoped>
.tasks-section {
  position: absolute;
  top: 10px;
  left: 10px;
  display: flex;
  align-items: center;
  gap: 10px;
  background: #1e2a38;
  padding: 10px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  z-index: 10;
}

.task-select {
  flex-grow: 1;
}

.task-btn {
  align-self: flex-start;
  margin-top: 10px;
}

.input-section {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 20px;
  padding: 12px;
  background-color: #e0f2f1; /* Light Teal */
  border-radius: 10px;
}

.widgets-section {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
}

.weather-widget {
  position: relative;
  background-color: #1e2a38;
  color: white;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
  transition: transform 0.3s, box-shadow 0.3s;
}

.weather-widget:hover {
  transform: translateY(-10px);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
}

.weather-widget.detailed {
  width: 90%;
  max-width: 700px;
}

.weather-widget.compact {
  width: 150px;
  height: 150px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
}

.current-weather {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
}

.current-weather .location h2 {
  margin: 0;
}

.current-details {
  display: flex;
  align-items: center;
  gap: 20px;
}

.current-details .temp {
  font-size: 2em;
}

.remove-btn {
  position: absolute;
  top: 10px;
  right: 10px;
}

.compact-widget .temp {
  font-size: 1em;
}

.compact-widget .location {
  font-size: 0.9em;
  margin-top: 10px;
}
</style>

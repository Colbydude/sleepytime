<template>
  <div class="row">
    <div class="twelve columns" align="center">
      <h2 style="text-align: center; font-size:150%;">sleepyti.me - Vue Edition</h2>
      <div id="start">
        <p><span style="font-size: 150%;">I have to wake up at...</span></p>
        <form @submit.prevent="calculate" style="text-align: center; margin-top: 20px;">
          <select v-model="hour" name="hour" required>
            <option :value="null">(hour)</option>
            <option v-for="hour in hourOptions" :key="hour">{{ hour }}</option>
          </select>
          <select v-model="minute" name="minute" required>
            <option :value="null">(minute)</option>
            <option v-for="minute in minuteOptions" :key="minute">{{ minute }}</option>
          </select>
          <select v-model="ampm" name="ampm">
            <option>AM</option>
            <option>PM</option>
          </select>
          <br>
          <button type="submit">calculate</button>
        </form>
      </div>
      <div id="results" v-if="res1 && res2 && res3 && res4">
        <p style="font-size: 130%">You should try to <b>fall asleep</b> at one of the following times:</p>
        <div style="font-size: 150%">
          <span id="result1" style="color: rgb(1, 223, 116);" title="Six Cycles: Nine Hours of Sleep">
            {{ formatDate(res1) }}
          </span>
          <i>or</i>
          <span id="result2" style="color: rgb(1, 223, 116);" title="Five Cycles: Seven and a half Hours of Sleep">
            {{ formatDate(res2) }}
          </span>
          <i>or</i>
          <span id="result3" style="color: rgb(1, 223, 116);" title="Four Cycles: Six Hours of Sleep">
            {{ formatDate(res3) }}
          </span>
          <i>or</i>
          <span id="result4" style="color: rgb(1, 223, 116);" title="Three Cycles: Four and a half Hours of Sleep">
            {{ formatDate(res4) }}
          </span>
        </div>
        <br>
        <p style="color: rgb(0, 128, 255);">
          Please keep in mind that you should be <b>falling asleep</b> at these times.<br>
          The average human takes <b>fourteen minutes</b> to fall asleep, so plan accordingly!
        </p>
        <p style="color: rgb(153, 102, 204);">
          sleepyti.me works by counting backwards in <b>sleep cycles</b>.<br>
          Waking up in the middle of a sleep cycle leaves you feeling tired and groggy, but waking up <i>in between</i> cycles wakes you up feeling refreshed and alert!
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SleepTime',

  data: () => ({
    // Seed data
    hourOptions: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
    minuteOptions: ['00', '05', '10', '15', '20', '25', '30', '35', '40', '45', '50', '55'],
    amPmOptions: ['AM', 'PM'],

    // Form data
    hour: null,
    minute: null,
    ampm: 'AM',

    // Results
    res1: null,
    res2: null,
    res3: null,
    res4: null,
  }),

  methods: {
    calculate () {
      let hourToUse = this.hour;
      const setTime = new Date();

      if (this.hour == 12) {
        hourToUse = 0;
      }

      if (this.ampm == 'AM') {
        setTime.setHours(hourToUse);
      } else if (this.ampm == 'PM') {
        setTime.setHours(hourToUse + 12);
      }

      setTime.setMinutes(this.minute);
      this.res4 = new Date(setTime.getTime() - 270 * 60000);
      this.res3 = new Date(this.res4.getTime() - 90 * 60000);
      this.res2 = new Date(this.res3.getTime() - 90 * 60000);
      this.res1 = new Date(this.res2.getTime() - 90 * 60000);
    },

    formatDate(date) {
      let formatted = '';
      let pm = false;

      if (date.getHours() > 12) {
        formatted = date.getHours() - 12;
        pm = true;
      } else if (date.getHours() < 12 && date.getHours() != 0) {
        formatted = date.getHours();
      } else if (date.getHours() == 0) {
        formatted = '12';
      } else if (date.getHours() == 12) {
        formatted = '12';
        pm = true;
      }

      if (date.getMinutes() < 10) {
        formatted = formatted + ':0' + date.getMinutes();
      } else {
        formatted = formatted + ':' + date.getMinutes();
      }

      if (pm == true) {
        formatted = formatted + ' PM';
      } else {
        formatted = formatted + ' AM';
      }

      return formatted;
    }
  }
}
</script>

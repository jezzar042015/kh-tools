<template>
  <div id="main" @mousemove="handleMouseMovements">
    <PreviewInfo :part="live.part"></PreviewInfo>
    <transition name="fade" v-show="view.controls">
      <SideBar :parts="parts" :activeMeeting="live.meeting" @switch-meeting="switchMeeting"></SideBar>
    </transition>
    <TimerDisplay :seconds="live.preview" :alert="alert" @click="setControls"></TimerDisplay>
    <transition name="fade" v-show="view.popup">
      <PopUps :part="live.nextPart"></PopUps>
    </transition>
  </div>
</template>

<script>
import meetings from './lib/parts'
import TimerDisplay from './components/TimerDisplay.vue';
import SideBar from './components/SideBar.vue';
import PopUps from './components/PopUps.vue';
import PreviewInfo from './components/PreviewInfo.vue';

export default {
  components: {
    TimerDisplay,
    SideBar,
    PopUps,
    PreviewInfo
  },
  data() {
    return {
      parts: [],
      live: {
        preview: 0,
        seconds: 0,
        running: false,
        partid: null,
        basis: null,
        part: null,
        partIndex: 0,
        nextPart: null,
        meeting: 'mwb',
      },
      view: {
        controls: true,
        activeInTs: 0,
        popup: false,
      },
      alert: {
        warning: false,
        overtime: false,
      }
    }
  },
  async mounted() {
    await this.autoloadParts()
    const parts = meetings[this.live.meeting]
    this.parts = parts;
    await this.selectPart(parts[0].id)
    await this.selectNextPart()
  },
  provide() {
    return {
      setControls: this.setControls,
      setPopup: this.setPopUp,
      setTimerSeconds: this.setTimerSeconds,
      selectPart: this.selectPart,
      selectNextPart: this.selectNextPart,
      view: this.view,
      live: this.live,
      start: this.startTimer,
      stop: this.stopTimer,
      restart: this.restartTimer,
      fwd: this.forwardTimer,
      bwd: this.backwardTimer,
    }
  },
  methods: {
    async autoloadParts() {
      const weekday = new Date().getDay()
      const isWeekend = [0,6].includes(weekday)
      this.live.meeting = isWeekend ? 'wt' : 'mwb'; 
    },
    async switchMeeting(code) {
      if (this.live.running) return
      this.live.meeting = code
      this.parts = meetings[this.live.meeting]
      await this.selectPart(this.parts[0].id)
      await this.selectNextPart()
    },
    setControls() {
      this.view.controls = !this.view.controls
    },
    setPopUp(setView) {
      this.view.popup = setView
    },
    setTimerSeconds(s = 0) {
      s = isNaN(s) ? 0 : s;
      this.live.seconds = s
    },
    async selectPart(id) {
      if (this.live.running) return

      this.live.partid = id
      this.parts.forEach((p, i) => {
        p.active = p.id == id
        if (p.id == this.live.partid) {
          this.live.seconds = p.consumed ?? 0
          this.live.preview = p.consumed ?? 0
          this.live.part = p
          this.live.partIndex = i
        }
      })
    },
    async selectNextPart() {
      const ci = this.live.partIndex;
      const partsLen = this.parts.length
      let increment = 1;

      while (
        this.parts[ci + increment]
        && !this.parts[ci + increment].timed
        && partsLen >= ci + increment
      ) {
        increment++;
      }

      this.live.nextPart = this.parts[ci + increment];
    },
    async startTimer() {
      this.live.running = true
      this.live.basis = new Date()
      this.live.preview = this.live.seconds
      this.startPreview()
      this.setControls()
    },
    startPreview() {
      const storedSeconds = this.live.preview
      this.intervalId = setInterval(() => {
        if (this.live.running) {
          const tc = new Date().getTime();
          const tb = this.live.basis.getTime();

          const timeDiff = Math.abs(tc - tb);
          const secondsDifference = timeDiff / 1000;
          this.live.preview = secondsDifference + storedSeconds;

          this.alertsWatcher(this.live.preview)

          // controls view setter
          if (this.view.controls) {
            this.view.activeInTs += 100
            if (this.view.activeInTs > 3000) {
              this.view.controls = false
              this.view.activeInTs = 0
            }
          }

        } else {
          this.stopTimer();
        }
      }, 100);
    },
    async getIntervalTs() {
      const tc = Date.now();
      const tb = this.live.basis.getTime()

      const timeDiff = Math.abs(tc - tb)
      const secondsDifference = timeDiff / 1000
      this.live.seconds += secondsDifference
      this.live.preview = this.live.seconds
    },
    async stopTimer() {
      this.live.running = false
      await this.getIntervalTs()
      await this.storePartTimerProgress()
      clearInterval(this.intervalId);
      this.alert.warning = false
      this.alert.overtime = false
      this.startNextWatcher()
    },
    async storePartTimerProgress() {
      this.parts.forEach(p => {
        if (p.id == this.live.partid) {
          p.consumed = this.live.seconds
        }
      })
    },
    startNextWatcher() {
      const stopWatcher = () => {
        clearInterval(this.nextPartWatcher);
      }

      this.nextPartWatcher = setInterval(() => {
        let suggestable = !this.live.running && !this.view.popup

        if (suggestable) {
          this.selectNextPart();
          this.view.popup = this.live.nextPart

        }

        stopWatcher()
      }, 500)

    },
    alertsWatcher(interval) {
      const limit = this.live.part.limit * 60;
      if (limit === 60) {
        this.alert.warning = interval >= 50 && interval < 60;
        this.alert.overtime = interval >= 59;
      } else {
        const warningThreshold = (limit > 180) ? limit - 60 : limit - 30;

        this.alert.warning = interval >= warningThreshold && interval < limit;
        this.alert.overtime = interval >= limit;
      }
    },
    restartTimer() {
      this.live.seconds = 0;
      this.live.preview = 0;
      this.live.basis = new Date()
    },
    forwardTimer() {
      this.live.basis.setSeconds(
        this.live.basis.getSeconds() - 15
      );
    },
    backwardTimer() {
      const tc = Date.now();
      const tb = this.live.basis.getTime()


      this.live.basis.setSeconds(
        this.live.basis.getSeconds() + 5
      );

      if (tb > tc) {
        this.live.basis = new Date()
      }
    },
    handleMouseMovements() {
      this.view.activeInTs = 0
    }
  },
  watch: {
    'view': {
      handle() {
        if (!this.view) return;
        if (!this.view.controls) this.view.activeInTs = 0
      },
      deep: false
    }
  },
  beforeUnmount() {
    clearInterval(this.intervalId);
    clearInterval(this.nextPartWatcher);
  },
}
</script>


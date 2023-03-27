<template>
    <div class="timepiece__main">
      <div v-for="(timepiece, index) in timepiece__main" :key="index" class="timepiece__block">
        <div v-bind:class="{'timepiece__active' : timepiece__main[index].play}" class="block__time">{{ time(timepiece.time) }}</div>
        <div v-bind:class="{'block__line_active' : timepiece__main[index].play}" class="block__line"></div>
        <div class="timepiece__control-console">
          <svg v-if="timepiece__main[index].play === false" class="control-console__btn" @click="startTimepiece(index)" width="17" height="20" viewBox="0 0 17 20" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M0 20V0L17 10L0 20Z" fill="#9E9E9E"/>
          </svg>
          <svg v-if="timepiece__main[index].play === true" class="control-console__btn" @click="pauseTimepiece(index)" width="10" height="20" viewBox="0 0 10 20" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect v-bind:class="{'timepiece__active' : timepiece__main[index].play}" x="7" width="3" height="20" fill="#9E9E9E"/>
            <rect v-bind:class="{'timepiece__active' : timepiece__main[index].play}" width="3" height="20" fill="#9E9E9E"/>
          </svg>
          <svg class="control-console__btn"  @click="removeTimepiece(index)" width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect v-bind:class="{'timepiece__active': timepiece__main[index].play}" width="20" height="20" fill="#9E9E9E"/>
          </svg>        
        </div>      
      </div>
      <div class="timepiece__add" @click="addTimepiece()">
        <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
          <rect x="8.5" width="3" height="20" fill="#9E9E9E"/>
          <rect y="11.5" width="3" height="20" transform="rotate(-90 0 11.5)" fill="#9E9E9E"/>
        </svg>
      </div>
  </div>
</template>

<style>
@import url('https://fonts.cdnfonts.com/css/gotham-pro');

.timepiece__main {
  display: flex;
  flex-wrap: wrap;
  max-width: 825px;
  margin: 0 auto;
  padding-top: 72px;
}
.timepiece__block {
  background: #696969;
  width: 225px;
  height: 120px;
  margin: 22.5px 25px;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: center;
}
.block__time {
  color: #9E9E9E;
  font-family: 'Gotham Pro';
  font-weight: 400;
  font-size: 22px;
  text-align: center;
}
.block__line {
  width: 223px;
  height: 1px;
  border-bottom: 0.3px solid #9E9E9E;
}
.block__line_active {
  border-bottom: 0.3px solid #fff;
}
.timepiece__control-console  {
  width: 85px;
  display: flex;
  justify-content: space-between;
}
.control-console__btn {  
  cursor: pointer;
  width: 20px;
  height: 20px;
}
.control-console__btn:first-child {
  width: 17px;
}
.timepiece__active {
  fill: #fff;
  color: #fff;
}
.timepiece__add {
  width: 225px;
  height: 120px;
  margin: 22.5px 25px;
  background: #696969;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
@media screen and (min-width: 1024px) {
  .timepiece__main {
  padding-top: 49.5px;
}
}
@media screen and (min-width: 768px) and (max-width: 1023px) {
  .timepiece__main {
    width: 550px;
}
.timepiece__block:nth-child(1), 
.timepiece__block:nth-child(2) {
  margin-top: 0;
}
}
@media screen and (min-width: 320px) and (max-width: 767px) {
  .timepiece__main {
    justify-content: center;
    max-width: 275px;
  }
  .timepiece__block:first-child {
    margin-top: 0;
  }
}
</style>

<script>
export default {
  data() {
    return {
      timepiece__main: [
        {
          play: false,
          time: 0,
          count: 0,
          intervalId: null,
        },
        {
          play: false,
          time: 0,
          count: 0,
          intervalId: null,
        },
        {
          play: false,
          time: 0,
          count: 0,
          intervalId: null,
        },
        {
          play: false,
          time: 0,
          count: 0,
          intervalId: null,
        },
      ],
    };
  },
  methods: {
    time(number) {
      const hour = Math.floor(number / 3600000);
      const minute = Math.floor((number - hour * 3600000) / 60000);
      const second = Math.floor((number - hour * 3600000 - minute * 60000) / 1000);
      if (minute <= 0 && hour <= 0) {
        return `${String(second).padStart(2, '0')}`;
      } else if (minute >= 0 && hour <= 0) {
        return `${String(minute).padStart(2, '0')}:${String(second).padStart(2, '0')}`;
      } else if (minute >= 0 && hour >= 0) {
        return `${String(hour).padStart(2, '0')}:${String(minute).padStart(2, '0')}:${String(second).padStart(2, '0')}`;
      }      
    },
    startTimepiece(timepieceId) {
      const timepiece = this.timepiece__main[timepieceId];
      if (!timepiece.play) {
        timepiece.play = true;
        timepiece.count = Date.now();
        timepiece.intervalId = setInterval(() => {
          const now = Date.now();
          const gone = now - timepiece.count;
          timepiece.time += gone;
          timepiece.count = now;
        }, 10);
      }
    },
    pauseTimepiece(timepieceId) {
      const timepiece = this.timepiece__main[timepieceId];
      if (timepiece.play) {
        timepiece.play = false;
        clearInterval(timepiece.intervalId);
      }
    },
    removeTimepiece(timepieceId) {
      const timepiece = this.timepiece__main[timepieceId];
      timepiece.time = 0;
      timepiece.count = 0;
      timepiece.play = false;
      clearInterval(timepiece.intervalId);
    },
    addTimepiece() {
      this.timepiece__main.push({
        time: 0,
        intervalId: null,
        count: 0,
        play: false,
      });
    },
  },
};
</script>
<template>
  <div class="home">
    <div class="course-table">
      <div class="course-table-main">
        <div class="calender">
          <div class="calender-top">
            <span
              class="last-arrow"
              @click="changeMonth('pre')"
            >
              <svg-icon
                icon-class="calender_arrow_left"
                style="font-size: 18px;"
              />
            </span>
            <span class="current-date"> {{currentYear}}年{{currentMonth}}月</span>
            <span
              class="next-arrow"
              @click="changeMonth('next')"
            >
              <svg-icon
                style="font-size: 18px;"
                icon-class="calender_arrow_right"
              />
            </span>
            <span class="ch-dot dot" />
            <span class="course-type">中教课</span>
            <span class="en-dot dot" />
            <span class="course-type">外教课</span>
          </div>
          <ul class="calender-week">
            <li
              v-for='(val,index) in weeks'
              :key="index"
              class="weekdays"
              :style="index % 7 === 6 ? 'margin-right: 0;' : ''"
            >
              <span>
                {{val}}
              </span>
            </li>
          </ul>
          <ul class="calender-days">
            <li
              v-for='(val,key) in days'
              :key="key"
              class="days"
              :style="key % 7 === 6 ? 'margin-right: 0;' : ''"
              @click="changeDay(val)"
            >
              <span :class='chooseClass(val.day)'>
                {{val.day !== '' ? val.day.getDate() : null}}
              </span>
              <span
                v-if="val.day !== '' && val.day.getDate() === 7"
                class="ch-dot dot dot-local"
              />
              <span
                v-if="val.day !== '' && val.day.getDate() === 8"
                class="en-dot dot dot-local"
              />
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      currentYear: 2019,//当前年份
      currentMonth: 12, // 当前月份
      currentDay: 1, // 当前月分第一天
      currentWeek: 1, //当前为星期几
      weeks: ["S", "M", "T", "W", "T", "F", "S"],// 星期名显示列表
      days: [], // 当前月份天数集合
      chooseDay: '',
    }
  },
  created () {
    this.init()
  },
  methods: {
    init (data) {
      let day
      if (data) {
        day = new Date(data)
      } else {
        let now = new Date()
        day = new Date(this.formDate(now.getFullYear(), now.getMonth() + 1, 1))
      }
      this.currentDay = day.getDate()
      this.currentYear = day.getFullYear()
      this.currentMonth = day.getMonth() + 1
      this.currentWeek = day.getDay()
      if (!this.currentWeek) {
        this.currentWeek = 7
      }
      this.days = []
      for (let empty = 0; empty < this.currentWeek; empty++) {
        if (this.currentWeek !== 7) {
          this.days.push({
            day: ''
          })
        }
      }
      let str = this.formDate(this.currentYear, this.currentMonth, this.currentDay)
      //判断当前月份有多少天
      let maxDate = new Date(this.currentYear, this.currentMonth, 0).getDate()
      for (let i = 1; i < maxDate + 1; i++) {
        let d = new Date(str)
        d.setDate(i)
        this.days.push({
          day: d
        })
      }
      if (this.chooseDay === '') {
        // 获取当前日期
        let currday = new Date()
        this.chooseDay = currday
      }
    },
    //其他月和其他天加class名'other'，今天加class名'active'
    chooseClass (day) {
      if (day === '') return "other"
      if (day.getMonth() + 1 !== this.currentMonth) return "other";
      if (day !== '') {
        let str = this.formDate(this.chooseDay.getFullYear(), this.chooseDay.getMonth(), this.chooseDay.getDate())
        let str1 = this.formDate(day.getFullYear(), day.getMonth(), day.getDate())
        if (str === str1) {
          return "active";
        } else {
          return "other"
        }
      }
    },
    // 更换日期查询课程
    changeDay (day) {
      this.chooseDay = day.day
    },
    //切换月份
    changeMonth (a) {
      let d = new Date(this.formDate(this.currentYear, this.currentMonth, 1))
      let maxDate = new Date(this.currentYear, this.currentMonth, 0).getDate()
      a === "pre" ? d.setDate(0) : d.setDate(maxDate + 1)
      this.init(this.formDate(d.getFullYear(), d.getMonth() + 1, 1))
    },
    //返回字符串个格式的日期
    formDate (year, month, day) {
      return year + "-" + month + "-" + day;
    },
  }
}
</script>

<style lang="less" scoped>
.public-calender-set {
  width: 312px;
  margin: 0 auto;
  display: flex;
  text-align: center;
  .weekdays,
  .days {
    width: 30px;
    font-size: 14px;
    margin-right: 16px;
  }
}
.public-day-set {
  display: block;
  width: 22px;
  height: 22px;
  line-height: 22px;
  margin: 20px auto 0;
  font-size: 16px;
}
.dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  margin: 0 6px;
}
.ch-dot {
  background: #eaff24ff;
}
.en-dot {
  background: #fff;
}
.course-table {
  width: 100%;
  position: relative;
  background: url("../../assets/time/calendar-bg@2x.png") no-repeat;
  background-position: 0 100%;
  background-size: cover;
  box-shadow: -2px 31px 51px 0px rgba(47, 47, 77, 0.16);
  .course-table-main {
    width: 100%;
    .calender {
      width: 100%;
      margin: 0 auto;
      box-sizing: border-box;
      color: #fff;
      .calender-top {
        display: flex;
        justify-content: center;
        padding-top: 23px;
        align-items: center;
        .current-date {
          font-size: 24px;
          padding: 0 12px;
        }
        .next-arrow {
          padding-right: 27px;
        }
        .course-type {
          font-size: 13px;
          font-weight: 400;
        }
      }
      .calender-week {
        .public-calender-set();
        margin-top: 25px;
        .weekdays {
          line-height: 17px;
        }
      }
      .calender-days {
        .public-calender-set();
        padding-bottom: 64px;
        flex-wrap: wrap;
        .days {
          position: relative;
          .other {
            .public-day-set();
          }
          .active {
            .public-day-set();
            background: #fff;
            border-radius: 6px;
            color: #3ed6e2ff;
          }
          .dot-local {
            position: absolute;
            bottom: -8px;
            left: 6px;
          }
        }
      }
    }
  }
}
</style>
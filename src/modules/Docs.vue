<template>
  <div id="app">
    <div class="container">
      <logo></logo>
      <hello :msg="msg" :show-link="false"></hello>
      <calendar
        v-model="value"
        showDateOnly
        :onDrawDate="onDrawDate"
        :disabled-days-of-week="disabled"
        :format="format"
        :clear-button="clear"
        :input-disabled="true"
        :placeholder="placeholder"
      ></calendar>
      <lorem :len="5"></lorem>

      <calendar
        v-model="value"
        :input-disabled="true"
        showDateOnly
        minDate="2017-12-29"
        maxDate="2018-01-27"
        @drawdate="onDrawDate2"
        :disabled-days-of-week="disabled"
        :format="format"
        :clear-button="clear"
        :placeholder="placeholder"
      ></calendar>
      <lorem :len="5"></lorem>

      <div class="col-sm-offset-8">
        <form class="form-horizontal">
          <div class="form-group">
            <label for="abcd" class="col-sm-5 control-label">Start Date:</label>
            <div class="col-sm-5">
              <calendar
                class="pull-left"
                element-id="abcd"
                v-model="value"
                :transfer="true"
                :disabled-days-of-week="disabled"
                :format="format"
                :clear-button="clear"
                :placeholder="placeholder"
                :pane="2"
              ></calendar>
            </div>
          </div>
        </form>
      </div>
      <div class="col-sm-offset-8x">
        <form class="form-horizontal">
          <div class="form-group">
            <label for="abcdd" class="col-sm-5 control-label"
              >Start Date:</label
            >
            <div class="col-sm-5">
              <calendar
                class="pull-left"
                element-id="abcdd"
                v-model="value"
                :transfer="true"
                :disabled-days-of-week="disabled"
                :format="format"
                :clear-button="clear"
                :placeholder="placeholder"
                :pane="2"
              ></calendar>
            </div>
          </div>
        </form>
      </div>

      <lorem :len="10"></lorem>
      <div style="height: 300px;">
        <calendar
          v-model="value"
          :disabled-days-of-week="disabled"
          :format="format"
          :clear-button="clear"
          :placeholder="placeholder"
          :pane="2"
          :has-input="false"
          :on-day-click="onDayClick1"
        ></calendar>
        <p>{{ date1 }}</p>
      </div>
      <lorem :len="3"></lorem>

      <div style="height: 300px;">
        <!-- range-bus example -->
        <calendar
          v-model="value"
          :disabled-days-of-week="disabled"
          :format="format"
          :clear-button="clear"
          :placeholder="placeholder"
          :pane="2"
          :range-bus="getBus"
          :range-status="1"
        ></calendar>
        <calendar
          v-model="value2"
          :value="value2"
          :disabled-days-of-week="disabled"
          :format="format"
          :clear-button="clear"
          :placeholder="placeholder"
          :pane="2"
          :range-bus="getBus"
          :range-status="2"
        ></calendar>
      </div>
      <lorem :len="3"></lorem>

      <div style="height: 300px;">
        <calendar
          v-model="value"
          :disabled-days-of-week="disabled"
          :format="format"
          :clear-button="clear"
          :placeholder="placeholder"
          :pane="2"
          :has-input="false"
          :on-day-click="onDayClick2"
          :special-days="_dateMap"
        >
        </calendar>
        <p>{{ date2 }}</p>
      </div>
      <lorem :len="3"></lorem>
      <div style="height: 600px;">
        <calendar
          class="event-calendar"
          v-model="value"
          :disabled-days-of-week="disabled"
          :format="format"
          :clear-button="clear"
          :placeholder="placeholder"
          :pane="2"
          :has-input="false"
          :on-day-click="onDayClick3"
          :change-pane="changePane"
        >
          <div
            class="event"
            v-for="(evt, index) in events"
            :key="index"
            :slot="evt.date"
          >
            ${{ evt.content }} <i :class="{ low: evt.low }" v-if="evt.low">↓</i>
          </div>
        </calendar>
        <p>{{ date3 }}</p>
      </div>
      <lorem :len="3"></lorem>
      <div style="height: 600px;">
        <calendar
          class="event-calendar"
          v-model="value"
          :disabled-days-of-week="disabled"
          :format="format"
          :clear-button="clear"
          :placeholder="placeholder"
          :pane="2"
          :has-input="false"
          :on-day-click="onDayClick4"
          :change-pane="changePane2"
        >
          <div
            class="event"
            v-for="(evt, index) in lurevents"
            :key="index"
            :slot="evt.date"
          >
            <div style="font-size:12px;" v-html="evt.content"></div>
          </div>
        </calendar>
        <p>{{ date4 }}</p>
      </div>

      <lorem :len="6"></lorem>
      <div class="text-center">
        Happy 1024!
      </div>
      <p class="lorem">consectetur adipisicing elit,</p>
    </div>
  </div>
</template>
<script>
import Vue from "vue";
import Logo from "components/Logo";
import Hello from "components/Hello";
import Lorem from "components/Lorem";
import Calendar from "components/Calendar";
import lunar from "utils/lunar";
export default {
  name: "docs",
  data() {
    return {
      msg: "Component Demo",
      disabled: [],
      value: this.stringify(new Date()),
      value2: "",
      date1: "",
      date2: "",
      date3: "",
      date4: "",
      events: [],
      lurevents: [],
      format: "yyyy-MM-dd",
      clear: true,
      isHoliday: true,
      placeholder: "placeholder is displayed",
      DATENAME: {
        today: "今天",
        yuandan: "元旦",
        chuxi: "除夕",
        chunjie: "春节",
        yuanxiao: "元宵",
        qingming: "清明",
        wuyi: "劳动",
        duanwu: "端午",
        zhongqiu: "中秋",
        guoqing: "国庆"
      },
      HOLIDAYS: {
        yuandan: [
          "2012-01-01",
          "2013-01-01",
          "2014-01-01",
          "2015-01-01",
          "2016-01-01",
          "2017-01-01",
          "2018-01-01",
          "2019-01-01",
          "2020-01-01"
        ],
        chuxi: [
          "2012-01-22",
          "2013-02-09",
          "2014-01-30",
          "2015-02-18",
          "2016-02-07",
          "2017-01-27",
          "2018-02-15",
          "2019-02-04",
          "2020-01-24"
        ],
        chunjie: [
          "2012-01-23",
          "2013-02-10",
          "2014-01-31",
          "2015-02-19",
          "2016-02-08",
          "2017-01-28",
          "2018-02-16",
          "2019-02-05",
          "2020-01-25"
        ],
        yuanxiao: [
          "2012-02-06",
          "2013-02-24",
          "2014-02-14",
          "2015-03-05",
          "2016-02-22",
          "2017-02-11",
          "2018-03-02",
          "2019-02-19",
          "2020-02-08"
        ],
        qingming: [
          "2012-04-04",
          "2013-04-04",
          "2014-04-05",
          "2015-04-05",
          "2016-04-04",
          "2017-04-04",
          "2018-04-05",
          "2019-04-05",
          "2020-04-04"
        ],
        wuyi: [
          "2012-05-01",
          "2013-05-01",
          "2014-05-01",
          "2015-05-01",
          "2016-05-01",
          "2017-05-01",
          "2018-05-01",
          "2019-05-01",
          "2020-05-01"
        ],
        duanwu: [
          "2012-06-23",
          "2013-06-12",
          "2014-06-02",
          "2015-06-20",
          "2016-06-09",
          "2017-05-30",
          "2018-06-18",
          "2019-06-07",
          "2020-06-25"
        ],
        zhongqiu: [
          "2012-09-30",
          "2013-09-19",
          "2014-09-08",
          "2015-09-27",
          "2016-09-15",
          "2017-10-04",
          "2018-09-24",
          "2019-09-13",
          "2020-10-01"
        ],
        guoqing: [
          "2012-10-01",
          "2013-10-01",
          "2014-10-01",
          "2015-10-01",
          "2016-10-01",
          "2017-10-01",
          "2018-10-01",
          "2019-10-01",
          "2020-10-01"
        ]
      }
    };
  },
  components: {
    Logo,
    Hello,
    Lorem,
    Calendar
  },
  created() {
    this.bus = new Vue();
  },
  mounted() {},
  computed: {
    _dateMap() {
      return this._createDateMap();
    }
  },
  methods: {
    getBus() {
      return this.bus;
    },
    onDrawDate(e) {
      let date = e.date;
      if (new Date().getTime() > date.getTime()) {
        e.allowSelect = false;
      }
    },
    onDrawDate2(e) {
      // console.info(e)
      // let date = e.date
      // if (new Date().getTime() > date.getTime()) {
      //   e.allowSelect = false
      // }
    },
    getDateInfo(v) {
      var iDiff = -1;
      var sNowDate = this.stringify(new Date());
      var sDateName = ["今天", "明天", "后天"];
      switch (true) {
        case v === sNowDate:
          iDiff = 0;
          break;
        case v === this.siblings(sNowDate, 1):
          iDiff = 1;
          break;
        case v === this.siblings(sNowDate, 2):
          iDiff = 2;
          break;
      }
      !this._dateMap &&
        this.isHoliday &&
        (this._dateMap = this._createDateMap());
      return (this._dateMap && this._dateMap[v]) || sDateName[iDiff];
    },
    _createDateMap() {
      var oTmp = {};
      for (var propety in this.HOLIDAYS) {
        var curHoliday = this.HOLIDAYS[propety];
        for (var i = 0; i < curHoliday.length; i++) {
          var sDate = curHoliday[i];
          var sName = this.DATENAME[propety];
          oTmp[sDate] = sName;
        }
      }
      return oTmp;
    },
    isDate(v) {
      if (v instanceof Date) {
        return true;
      }
      return false;
    },
    stringify(v) {
      if (!this.isDate(v)) return null;
      return (
        v.getFullYear() +
        "-" +
        this.filled(v.getMonth() * 1 + 1) +
        "-" +
        this.filled(v.getDate())
      );
    },
    siblings(v, n) {
      var REG = /\d+/g;
      v = v.match(REG);
      return this.stringify(new Date(v[0], v[1] - 1, v[2] * 1 + n * 1));
    },
    filled(v) {
      return String(v).replace(/^(\d)$/, "0$1");
    },
    onDayClick1(date, str) {
      this.date1 = str;
    },
    onDayClick2(date, str) {
      this.date2 = this.getDateInfo(str) || str;
    },
    onDayClick3(date, str) {
      this.date3 = str;
    },
    changePane(year, month, pane) {
      this.events = [];
      // ajax data or ...
      setTimeout(() => {
        this.events = this.getEventContent(year, month, pane);
      }, 200);
    },
    onDayClick4(date, str) {
      this.date4 = str;
    },
    changePane2(year, month, pane) {
      var Today = new Date();
      var ty = parseInt(Today.getFullYear());
      var tm = parseInt(Today.getMonth());
      var td = parseInt(Today.getDate());
      var days = [];
      for (var i = 0; i < pane; i++) {
        var date = new Date(year, month + i);
        var r = new lunar.Calendar(
          date.getFullYear(),
          date.getMonth(),
          ty,
          tm,
          td
        );
        days = days.concat([].slice.call(r, 0));
      }
      for (var j = 0; j < days.length; j++) {
        days[j].date = this.stringify(
          new Date(days[j].sYear, days[j].sMonth - 1, days[j].sDay)
        );
        days[j].content = this.foramtDay(days[j]);
      }
      this.lurevents = days;
    },
    foramtDay(el) {
      /* eslint-disable */
      var S = "",
        J,
        I;
      if (el.lDay == 1) {
        S =
          "<b>" +
          (el.isLeap ? "\u95f0" : "") +
          el.lMonth +
          "\u6708" +
          (lunar.monthDays(el.lYear, el.lMonth) == 29 ? "\u5c0f" : "\u5927") +
          "</b>";
      } else {
        S = lunar.cDay(el.lDay);
      }
      I = el.lunarFestival;
      if (el.lMonth == "4" && I.indexOf("\u7aef\u5348\u8282") != -1) {
        I = "";
        el.lunarFestival = "";
      }
      if (I.length > 0) {
        if (I.length > 7) {
          // I = I.substr(0, 5) + "\u2026"
          I = I.split(" ")[0];
        }
        I = I.fontcolor("#909090");
      } else {
        // I = el.solarFestival;
        // if (I.length > 0) {
        //     J = (I.charCodeAt(0) > 0 && I.charCodeAt(0) < 128) ? 9 : 5;
        //     if (I.length > J + 1) {
        //         I = I.substr(0, J - 1) + "\u2026"
        //     }
        //     I = I.fontcolor("#909090");
        // } else {
        //     I = el.solarTerms;
        //     if (I.length > 0) {
        //         I = I.fontcolor("#ff7200") // 节日
        //     }
        // }
      }
      if (I.length > 0) {
        S = I;
      }
      return S;
    },
    getDayCount(year, month) {
      const dict = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];
      if (month === 1) {
        if (year % 400 === 0 || (year % 4 === 0 && year % 100 !== 0)) {
          return 29;
        }
      }
      return dict[month];
    },
    random(min, max) {
      if (max == null) {
        max = min;
        min = 0;
      }
      return min + Math.floor(Math.random() * (max - min + 1));
    },
    getEventContent(year, month, pane) {
      const data = [];
      for (let p = 0; p < pane; p++) {
        let date = new Date(year, month + p);
        let monthCounts = this.getDayCount(date.getFullYear(), date.getMonth());
        for (let i = 1; i <= monthCounts; i++) {
          data.push({
            date: this.stringify(new Date(year, month + p, i)),
            content: this.random(100, 1000),
            low: this.random(1)
          });
        }
      }
      return data;
    }
  }
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.lorem {
  visibility: hidden;
}
.event-calendar {
  .datepicker-inner {
    width: 440px;
  }
  .datepicker-body {
    span {
      width: 60px;
      height: 60px;
      vertical-align: top;
    }
    .event {
      color: #e56700;
    }
    .low {
      color: red;
      font-weight: bold;
    }
    .datepicker-monthRange span {
      width: 100px;
      height: 100px;
      vertical-align: middle;
      line-height: 100px;
      // font-weight: 600;
    }
  }
}
</style>

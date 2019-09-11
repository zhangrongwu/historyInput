<template>
  <div class="historyInput">
    <van-field :label="label" :placeholder="placeholder" autosize v-model="cMessage" @focus="inputFocus()" @blur="inputBlur()" @input="inputChange" />
    <div v-show="bzfirstResponder" class="bz-first-responder">
      <div class="bz-first-item" @click="bzEvent(item)" v-for="(item, index) in bzInputHistory" :key="index">{{item}}</div>
    </div>
  </div>
</template>

<script>
export default {
  computed: {
    cMessage: {
      get() {
        return this.value;
      },
      set(val) {
        this.newValue = val;
      }
    }
  },
  props: {
    label: {
      type: String,
      default: "标题"
    },
    placeholder: {
      type: String,
      default: "请输入"
    },
    value: {
      type: String,
      default: ""
    },
    historyKey: {
      type: String,
      default: "bzHistoryInfo"
    },
    maxHistory: {
      type: Number,
      default: 10
    }
  },
  model: {
    prop: "value",
    event: "change"
  },

  data() {
    return {
      bzfirstResponder: false,
      bzInputHistory: JSON.parse(window.localStorage.getItem(this.historyKey)),
      newValue: ""
    }
  },
  created() {
    ;
  },
  mounted() {
    ;
  },
  methods: {
    inputFocus() {
      console.log("inputFocus")
      this.bzfirstResponder = true;
    },
    inputBlur() {
      setTimeout(() => {

        console.log("inputBlur", this.value)
        var list = JSON.parse(window.localStorage.getItem(this.historyKey));
        if (this.value != null) {
          if (list != null) {
            if (list.indexOf(this.value) == -1) {//不存在

              if (list.length >= this.maxHistory) {
                list.splice(this.maxHistory - 1, list.length - this.maxHistory + 1)
              }
            } else {
              for (var i = 0; i < list.length; i++) {
                if (list[i] == this.value) {
                  list.splice(i, 1);
                  break;
                }
              }
            }
            list.splice(0, 0, this.value)
          } else {
            list = [];
            list.push(this.value)
          }
          window.localStorage.setItem(this.historyKey, JSON.stringify(list))
          this.bzInputHistory = JSON.parse(window.localStorage.getItem(this.historyKey));
        }

        this.bzfirstResponder = false;
      }, 100);
    },
    bzEvent(val) {
      console.log("bzEvent", val)
      this.newValue = val;
      this.inputChange();
      this.bzfirstResponder = false;
    },
    inputChange() {
      console.log("inputChange", this.newValue)
      this.$emit("change", this.newValue)
    }
  },
}
</script>

<style >
.bz-first-responder {
  position: absolute;
  z-index: 1;
  margin-left: 28%;
  padding-top: 10px;
  background: white;
  border: 1px solid lightgray;
  border-radius: 3px 3px 3px 3px;
}

.bz-first-item {
  height: 22px;
  font-size: 12px;
  color: #b0b1b3;
  padding-left: 10px;
  padding-right: 10px;
}
.historyInput {
}
</style>

<template>
  <div>
    <loading :active.sync="isLoading"></loading>
    <div class="TaiwanTitle justify-between font-bold py-4 bg-gray-200 px-4">
      <div
        class="header flex flex-col items-center md:flex-row md:justify-between"
      >
        <h4 class="md:inline-block ml-3 mb-2 ld:mb-6">
          數據來源來自於衛福部疾管署，與1922所公佈數據可能有些不一樣，還是以1922公佈數據為主
        </h4>
      </div>
      <div
        class="
          info
          text-center
          md:text-left md:flex
          justify-between
          px-4
          md:px-2
        "
      >
        <h2 class="mb-6 md:mb-0">
          <span class="pb-1 font-bold text-3xl border-b-4 border-yellow-500"
            >台灣各地區總確診人數</span
          >
        </h2>
      </div>
    </div>

    <div
      class="
        section
        grid grid-cols-1
        sm:grid-cols-2
        ld:grid-cols-3
        xl:grid-cols-4
        gap-x-20 gap-y-10
        bg-gray-200
        px-8
        pb-8
      "
    >
      <div
        class="
          cityCard
          mt-10
          border-2 border-black
          rounded-3xl
          text-center
          opacity-80
        "
        v-for="(item, i) in data"
        :key="i"
      >
        <h2
          class="
            countryTitle
            text-xl
            border-b-2 border-black
            bg-green-300
            font-bold
            rounded-full
            py-2
          "
          :class="{ plusColor: comfirmedCaseData[i] }"
        >
          {{ item.city }}
        </h2>
        <!-- 有增加確診人數 title就會變顏色 -->

        <div class="comfirmedCasePlus justify-center">
          <h2 class="text-5xl text-blue-700 my-12 font-bold opacity-100">
            {{ item.confirmed.total }}
          </h2>
          <h3
            class="
              plusNum
              font-bold
              text-3xl
              ld:ml-0 ld:text-4xl
              text-red-700
              font-mono
              mt-6
              pt-2
            "
            v-if="comfirmedCaseData[i]"
          >
            +{{ comfirmedCaseData[i] }}
          </h3>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { confirmedCity } from '../data/data'
export default {
  data() {
    return {
      data: [],
      comfirmedCaseData: [], // 新增確診人數的Data (做title變色 跟 新增人數數量的提示)
      isLoading: false
    }
  },
  methods: {
    async getCovidCity() {
      this.isLoading = true
      try {
        const self = this
        self.data = confirmedCity
        self.getConfirmCases()
      } catch (error) {
        console.log("error: ", error)
      } finally {
        this.isLoading = false
      }
    },
    getConfirmCases() {
      const self = this
      self.data.map(element => {
        if (element.confirmed.new > 0) {
          self.comfirmedCaseData.push(element.confirmed.new)
        } else {
          self.comfirmedCaseData.push(0)
        }
      })
    },
  },
  created() {
    this.getCovidCity()
  },
}
</script>

<style lang="scss">
button {
  border: 3px solid black;
}
.comfirmedCasePlus {
  position: relative;
}
.plusNum {
  // 增加人數 數字的位置
  position: absolute;
  right: 7%;
  top: 45%;
}
.plusColor {
  // 有增加確診人數  就會變顏色
  background: #ff2d2d;
}
.cityCard {
  background-image: url(../../assets/Person-city.jpg);
  background-size: cover;
  background-position: center center;
}
</style>

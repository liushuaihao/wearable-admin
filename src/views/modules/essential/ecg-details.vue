<template>
  <el-card shadow="never" class="aui-card--fill">
    <p>
      <el-button type="success">导出</el-button>
    </p>
    <div class="details-box" id="ecg-details-dome">
      <div class="details-title">
        基本信息
      </div>
      <div class="details-main">
        <details-info />
      </div>
      <div class="details-title">
        心电情况
      </div>
      <div class="details-main">
        <el-form :inline="true" :model="dataForm">
          <el-form-item>
            <el-date-picker
              v-model="daterange"
              type="datetimerange"
              value-format="yyyy-MM-dd HH:mm:ss"
              :range-separator="$t('datePicker.range')"
              start-placeholder="起止时间"
              end-placeholder="结束时间"
            ></el-date-picker>
          </el-form-item>
          <el-form-item>
            <el-button @click="getDataList()">{{ $t("query") }}</el-button>
          </el-form-item>

          <div class="tjx">心电图</div>
          <ecg />
          <ecgcan grids="grids" ifStarts="ifStarts" myCanvas="myCanvas" />
          <!--<huxi /> -->
          <el-form-item label="分析时段：">
            <el-radio-group v-model="type" @change="getHeartRate">
              <el-radio :label="0">24小时</el-radio>
              <el-radio :label="1">1周</el-radio>
              <el-radio :label="2">1个月</el-radio>
            </el-radio-group>
          </el-form-item>
          <!-- <div class="human">
            <el-form-item label="心率：">
              <div>60~100次/分</div>
            </el-form-item>
            <el-form-item label="时段：">
              <div>2019-04-05 14:00</div>
            </el-form-item>
          </div> -->
          <div class="tjx">心率展示</div>
          <chartEcg :dataList="heartRateData" />
          <el-form-item label="心电变异性分析：">
            <div>T波改变 异常建议进一步进行检查</div>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </el-card>
</template>
<script>
export default {
  components: {
    "details-info": () => import("@/components/details-info"),
    ecg: () => import("@/components/ecg"),
    ecgcan: () => import("@/components/ecg/ecg"),
    huxi: () => import("@/components/huxi"),
    chartEcg: () => import("@/components/chartEcg"),
  },
  data() {
    return {
      ifStarts: true,
      daterange: "",
      type: 0,
      userId: "",
      dataForm: {},
      heartRateData: [],
      arr: [
        1927,
        1953,
        1951,
        1969,
        2001,
        2375,
        3263,
        2660,
        1600,
        1796,
        1915,
        1926,
        1935,
        1991,
        1981,
        2000,
        1994,
        2021,
        2038,
        2080,
        2131,
        2184,
        2225,
        2289,
        2351,
        2401,
        2418,
        2409,
        2317,
        2271,
        2126,
        2016,
        1911,
        1874,
        1819,
        1832,
        1814,
        1820,
        1836,
        1876,
        1859,
        1866,
        1866,
        1863,
        1881,
        1936,
        1940,
        1948,
        1973,
        2053,
        2021,
        2018,
        2059,
        2050,
        1989,
        2017,
        2105,
        2169,
        2125,
        2115,
        2077,
        2065,
        2071,
        2045,
        2041,
        2031,
        2042,
        2019,
        2073,
        2745,
        3422,
        2049,
        1538,
        1821,
        1935,
        1931,
        1937,
        1966,
        1967,
        2011,
        1983,
        1980,
        2004,
        2040,
        2053,
        2096,
        2157,
        2243,
        2290,
        2353,
        2363,
        2319,
        2219,
        2208,
        2077,
        1971,
        1852,
        1783,
        1759,
        1774,
        1757,
        1782,
        1801,
        1807,
        1854,
        1879,
        1871,
        1885,
        1882,
        1908,
        1863,
        1831,
        1849,
        1841,
        1817,
        1862,
        1869,
        1858,
        1879,
        1937,
        1924,
        1920,
        1882,
        1903,
        1911,
        1903,
        1893,
        1922,
        1906,
        1909,
        1911,
        2274,
        3245,
        2817,
        1583,
        1733,
        1870,
        1900,
        1907,
        1939,
        1946,
        1998,
        2033,
        2064,
        2051,
        2095,
        2150,
        2195,
        2235,
        2293,
        2368,
        2413,
        2436,
        2413,
        2356,
        2270,
        2119,
        2031,
        1937,
        1915,
        1865,
        1854,
        1844,
        1849,
        1865,
        1900,
        1885,
        1913,
        1900,
        1927,
        1930,
        1953,
        1942,
        1966,
        1978,
        1992,
        1951,
        1980,
        1962,
        1937,
        1919,
        1912,
        1990,
        2024,
        2005,
        1926,
        1899,
        1945,
        1923,
        1931,
        1922,
        1926,
        1921,
        1916,
        2018,
        2719,
        3370,
        2002,
        1613,
        1833,
        1874,
        1889,
        1889,
        1926,
        1927,
        1925,
        1949,
        1963,
        1998,
        2046,
        2064,
        2109,
        2154,
        2226,
        2290,
        2342,
        2350,
        2313,
        2237,
        2166,
        2060,
        1965,
        1893,
        1836,
        1815,
        1833,
        1834,
        1841,
        1846,
        1899,
        1895,
        1932,
        1940,
        1936,
        1912,
        1941,
        1908,
        1914,
        1886,
        1905,
        1914,
        1907,
        1895,
        1923,
        1889,
        1902,
        1944,
        1910,
        1967,
        2039,
        1999,
        1988,
        1950,
        1943,
        1920,
        1935,
        1920,
        1952,
        1960,
        1923,
        1943,
        2506,
        3345,
        2274,
        1546,
        1763,
        1835,
        1856,
        1871,
        1896,
        1891,
        1937,
        1981,
        1996,
        1993,
        2041,
        2097,
        2139,
        2182,
        2287,
        2336,
        2410,
        2428,
        2397,
        2324,
        2259,
        2128,
        2015,
        1889,
        1828,
        1792,
        1797,
        1765,
        1776,
        1760,
        1795,
        1802,
        1804,
        1859,
        1866,
        1858,
        1866,
        1877,
        1913,
        1912,
        1925,
        1913,
        1914,
        1924,
        1929,
        1963,
        1954,
        1931,
        1952,
        1928,
        1975,
        2053,
        2077,
        2026,
        1959,
        1947,
        1938,
        1948,
        1943,
        1988,
        2032,
        1986,
        2053,
        2638,
        3417,
        2188,
        1618,
        1827,
        1938,
        1932,
        1941,
        1953,
        2009,
        1982,
        2010,
        2018,
        2066,
        2091,
        2105,
        2143,
        2204,
        2302,
        2391,
        2398,
        2437,
        2387,
        2353,
        2292,
        2155,
        2030,
        1940,
        1854,
        1871,
        1858,
        1845,
        1833,
        1830,
        1861,
        1884,
        1874,
        1895,
        1893,
        1942,
        1914,
        1934,
        1931,
        1943,
        1909,
        1906,
        1905,
        1925,
        1906,
        1916,
        1917,
        1936,
        1980,
        2042,
        2037,
        2055,
        2026,
        2017,
        2016,
        2002,
        1982,
        1985,
        1960,
        1954,
        2006,
        2483,
        3405,
        2511,
        1541,
        1750,
        1891,
        1923,
        1939,
        1972,
        1997,
        2027,
        2074,
        2107,
        2127,
        2132,
        2149,
        2201,
        2230,
        2292,
        2376,
        2441,
        2421,
        2381,
        2356,
        2285,
        2165,
        2069,
        1967,
        1885,
        1831,
        1822,
        1802,
        1816,
        1844,
        1883,
        1896,
        1915,
        1908,
        1923,
        1931,
        1941,
        1949,
        2016,
        2010,
        1991,
        1957,
        1971,
        1965,
        1977,
        1966,
        1947,
        1925,
        2016,
        2060,
        2045,
        1988,
        1961,
        1963,
        1983,
        1971,
        2011,
        2021,
        2050,
        2035,
        2182,
        2875,
        3394,
        1969,
        1695,
        1895,
        1948,
        1937,
        1939,
        1988,
        2031,
        2010,
        2009,
        2029,
        2061,
        2096,
        2134,
        2135,
        2219,
        2294,
        2359,
        2393,
        2419,
        2349,
        2293,
        2180,
        2059,
        1963,
        1884,
        1811,
        1779,
        1752,
        1748,
        1769,
        1791,
        1803,
        1832,
        1819,
        1834,
        1851,
        1887,
        1907,
        1925,
      ],
      arr_v11: [],
    };
  },
  watch: {
    daterange(val) {
      this.dataForm.startDate = val[0];
      this.dataForm.endDate = val[1];
    },
  },
  created() {
    // //*************************************使用示例****************************
    let luo2_zhuan = this.jinzhi(this.arr);
    this.arr_v11 = luo2_zhuan.map(Number); //数字是字符串的必须转number类型，要不然canvas不认。
    console.log(this.arr_v11);
    this.userId = this.$route.params.userId;
    console.log(this.$route.params);
    if (this.userId) {
      this.getHeartRate();
    }
  },
  methods: {
    getHeartRate() {
      this.$http
        .get("/ecg/getHeartRate", {
          params: {
            userId: this.userId,
            type: this.type,
          },
        })
        .then(({ data: res }) => {
          if (res.code !== 0) {
            return this.$message.error(res.msg);
          }
          this.heartRateData = res.data;
        })
        .catch(() => {});
    },
    handleDown() {
      this.$htmlToPdf.downloadPDF(
        document.querySelector("#ecg-details-dome"),
        this.$route.name
      );
    },
    jinzhi(list) {
      // let list = lsit.split(",");
      console.log(list);
      let yu1 = ["7fff"];
      let yu2 = ["8000"];
      let yu1_v = parseInt(yu1, 16);
      let yu2_v = parseInt(yu2, 16);
      let list_xin_2 = [];
      list.forEach((yuan) => {
        // item = item.replace("0X", ""); //16进制 比如 0x0006   拿的其实就是后四位。这是这一步的来历
        // let yuan = parseInt(item, 16);
        let ins = yuan & yu2_v;
        if (ins == 0) {
          // console.log(String(yuan))
          list_xin_2.push(String(yuan));
        } else {
          let intsss = yuan & yu1_v;
          let fu_item = "-" + intsss;
          list_xin_2.push(fu_item);
        }
      });
      console.log(list_xin_2);
      return list_xin_2;
    },
  },
};
</script>
<style scoped>
.details > h3 {
  text-align: center;
  line-height: 80px;
  margin: 0;
}
.personal {
  display: flex;
  width: 70%;
}
.personal > p {
  flex: 1;
}
.tjx {
  height: 300px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
  background-color: #ccc;
}
.human {
  width: 36%;
  display: flex;
}
.human > div {
  flex: 1;
}
</style>

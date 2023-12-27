<script setup>
import api from '@/api';
import G6 from '@antv/g6/dist/g6.min';

import * as echarts from 'echarts';

const apiHeader = 'http://113.57.92.254:8002/api/v1';
const apiHost = 'http://113.57.92.254:8002';

const redFlagImg =
  'data:image/png;base64,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';
const redFlagBg =
  'data:image/png;base64,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';

const activePage = ref(0);

const pageInfo = reactive({
  current: 1,
  size: 10,
  pages: 10
});

let activeProvinceInfo = reactive({ id: 1, name: 'China' });

let activeCaseInfo = ref(null);

let caseList = reactive([]);

const provinceList = [
  {
    id: 2,
    name: '北京市',
    parentId: 1,
    lat: 40.072363,
    lng: 116.452562,
    pinYin: 'Beijing'
  },
  {
    id: 3,
    name: '天津市',
    parentId: 1,
    lat: 39.133066,
    lng: 117.262043,
    pinYin: 'Tianjin'
  },
  {
    id: 4,
    name: '河北省',
    parentId: 1,
    lat: 38.644184,
    lng: 115.643081,
    pinYin: 'Hebei'
  },
  {
    id: 5,
    name: '山西省',
    parentId: 1,
    lat: 37.857014,
    lng: 112.549248,
    pinYin: 'Shanxi'
  },
  {
    id: 6,
    name: '内蒙古自治区',
    parentId: 1,
    lat: 43.163929,
    lng: 112.846691,
    pinYin: 'NeiMengGu'
  },
  {
    id: 7,
    name: '辽宁省',
    parentId: 1,
    lat: 41.470982,
    lng: 123.075589,
    pinYin: 'Liaoning'
  },
  {
    id: 8,
    name: '吉林省',
    parentId: 1,
    lat: 43.593362,
    lng: 126.460691,
    pinYin: null
  },
  {
    id: 9,
    name: '黑龙江省',
    parentId: 1,
    lat: 47.220176,
    lng: 128.153242,
    pinYin: null
  },
  {
    id: 10,
    name: '上海市',
    parentId: 1,
    lat: 31.262235,
    lng: 121.456626,
    pinYin: null
  },
  {
    id: 11,
    name: '江苏省',
    parentId: 1,
    lat: 33.015387,
    lng: 119.837664,
    pinYin: null
  },
  {
    id: 12,
    name: '浙江省',
    parentId: 1,
    lat: 29.217657,
    lng: 120.426378,
    pinYin: null
  },
  {
    id: 13,
    name: '安徽省',
    parentId: 1,
    lat: 32.174367,
    lng: 117.041275,
    pinYin: null
  },
  {
    id: 14,
    name: '福建省',
    parentId: 1,
    lat: 26.371211,
    lng: 118.071524,
    pinYin: null
  },
  {
    id: 15,
    name: '江西省',
    parentId: 1,
    lat: 27.787491,
    lng: 115.863848,
    pinYin: null
  },
  {
    id: 16,
    name: '山东省',
    parentId: 1,
    lat: 36.119406,
    lng: 117.777167,
    pinYin: null
  },
  {
    id: 17,
    name: '河南省',
    parentId: 1,
    lat: 34.307663,
    lng: 113.87694,
    pinYin: null
  },
  {
    id: 18,
    name: '湖北省',
    parentId: 1,
    lat: 31.262235,
    lng: 112.03721,
    pinYin: null
  },
  {
    id: 19,
    name: '湖南省',
    parentId: 1,
    lat: 27.656511,
    lng: 112.03721,
    pinYin: null
  },
  {
    id: 20,
    name: '广东省',
    parentId: 1,
    lat: 23.994144,
    lng: 113.656173,
    pinYin: null
  },
  {
    id: 21,
    name: '广西壮族自治区',
    parentId: 1,
    lat: 24.129297,
    lng: 109.020054,
    pinYin: null
  },
  {
    id: 22,
    name: '海南省',
    parentId: 1,
    lat: 19.317629,
    lng: 109.903124,
    pinYin: null
  },
  {
    id: 23,
    name: '重庆市',
    parentId: 1,
    lat: 30.053442,
    lng: 107.842627,
    pinYin: null
  },
  {
    id: 24,
    name: '四川省',
    parentId: 1,
    lat: 31.135715,
    lng: 102.544205,
    pinYin: 'SiChuan'
  },
  {
    id: 25,
    name: '贵州省',
    parentId: 1,
    lat: 27.196833,
    lng: 107.106735,
    pinYin: null
  },
  {
    id: 26,
    name: '云南省',
    parentId: 1,
    lat: 24.39917,
    lng: 101.513957,
    pinYin: null
  },
  {
    id: 27,
    name: '西藏',
    parentId: 1,
    lat: 29.660361,
    lng: 91.132212,
    pinYin: null
  },
  {
    id: 28,
    name: '陕西省',
    parentId: 1,
    lat: 34.855464,
    lng: 109.020054,
    pinYin: null
  },
  {
    id: 29,
    name: '甘肃省',
    parentId: 1,
    lat: 38.122853,
    lng: 102.249849,
    pinYin: null
  },
  {
    id: 30,
    name: '青海省',
    parentId: 1,
    lat: 36.238732,
    lng: 95.038108,
    pinYin: null
  },
  {
    id: 31,
    name: '宁夏回族自治区',
    parentId: 1,
    lat: 37.421868,
    lng: 106.150075,
    pinYin: null
  },
  {
    id: 32,
    name: '新疆',
    parentId: 1,
    lat: 41.526309,
    lng: 85.324335,
    pinYin: null
  }
];

const cityList = [];

const provinceToMap = {
  China: '/asset/get/s/data-1527045631990-r1dZ0IM1X.json',
  上海市: '/asset/get/s/data-1482909900836-H1BC_1WHg.json',
  河北省: '/asset/get/s/data-1482909799572-Hkgu_yWSg.json',
  山西省: '/asset/get/s/data-1482909909703-SyCA_JbSg.json',
  内蒙古自治区: '/asset/get/s/data-1482909841923-rkqqdyZSe.json',
  辽宁省: '/asset/get/s/data-1482909836074-rJV9O1-Hg.json',
  吉林省: '/asset/get/s/data-1482909832739-rJ-cdy-Hx.json',
  黑龙江省: '/asset/get/s/data-1482909803892-Hy4__J-Sx.json',
  江苏省: '/asset/get/s/data-1482909823260-HkDtOJZBx.json',
  浙江省: '/asset/get/s/data-1482909960637-rkZMYkZBx.json',
  安徽省: '/asset/get/s/data-1482909768458-HJlU_yWBe.json',
  福建省: '/asset/get/s/data-1478782908884-B1H6yezWe.json',
  江西省: '/asset/get/s/data-1482909827542-r12YOJWHe.json',
  山东省: '/asset/get/s/data-1482909892121-BJ3auk-Se.json',
  河南省: '/asset/get/s/data-1482909807135-SJPudkWre.json',
  湖北省: '/asset/get/s/data-1482909813213-Hy6u_kbrl.json',
  湖南省: '/asset/get/s/data-1482909818685-H17FOkZSl.json',
  广东省: '/asset/get/s/data-1482909784051-BJgwuy-Sl.json',
  广西壮族自治区: '/asset/get/s/data-1482909787648-SyEPuJbSg.json',
  海南省: '/asset/get/s/data-1482909796480-H12P_J-Bg.json',
  四川省: '/asset/get/s/data-1482909931094-H17eKk-rg.json',
  贵州省: '/asset/get/s/data-1482909791334-Bkwvd1bBe.json',
  云南省: '/asset/get/s/data-1482909957601-HkA-FyWSx.json',
  西藏: '/asset/get/s/data-1482927407942-SkOV6Qbrl.json',
  陕西省: '/asset/get/s/data-1482909918961-BJw1FyZHg.json',
  甘肃省: '/asset/get/s/data-1482909780863-r1aIdyWHl.json',
  青海省: '/asset/get/s/data-1482909853618-B1IiOyZSl.json',
  宁夏回族自治区: '/asset/get/s/data-1482909848690-HJWiuy-Bg.json',
  新疆: '/asset/get/s/data-1482909952731-B1YZKkbBx.json',
  北京市: '/asset/get/s/data-1482818963027-Hko9SKJrg.json',
  天津市: '/asset/get/s/data-1482909944620-r1-WKyWHg.json',
  重庆市: '/asset/get/s/data-1482909775470-HJDIdk-Se.json',
  香港: '/asset/get/s/data-1461584707906-r1hSmtsx.json',
  澳门: '/asset/get/s/data-1482909771696-ByVIdJWBx.json'
};

const loadProvinceMapJson = async (mapName, url) => {
  //加载时的文字提示
  if (!echarts.getMap(mapName)) {
    await api.axios.get(`http://api.ppmark.cn/chart-assets/${url}`).then(geoJson => {
      echarts.registerMap(mapName, geoJson);
    });
  }
};

provinceList.map(async item => {
  console.log(item.name, provinceToMap[item.name]);
  await loadProvinceMapJson(item.name, provinceToMap[item.name]);
});

let chinaChart = null;

let provinceChart = null;

const flagList = [];

const initTree = () => {
  const container = document.getElementById('container');
  const width = container.scrollWidth || 800;
  const height = container.scrollHeight || 800;

  const registerNode = () => {
    G6.registerEdge('dice-er-edge', {
      afterDraw(cfg, group) {
        {
          const shape = group.get('children')[0];
          let index = 0;
          // Define the animation
          const lineDash = [4, 2, 1, 2];
          shape.animate(
            () => {
              index++;
              if (index > 9) {
                index = 0;
              }
              const res = {
                lineDash,
                lineDashOffset: -index
              };
              // returns the modified configurations here, lineDash and lineDashOffset here
              return res;
            },
            {
              repeat: true, // whether executes the animation repeatly
              duration: 3000 // the duration for executing once
            }
          );
        }
      }
    });
    G6.registerNode(
      'lineNode',
      {
        drawText(cfg, group) {
          group.addShape('text', {
            attrs: {
              // text:cfg.label,
              // fill:'white',
              // ...cfg.labelCfg.style
            }
          });
        },
        draw(cfg, group) {
          //1.
          const attrs = cfg;
          let pointCount = 0;

          cfg.details.forEach(detail => {
            console.log({ pointCount });
            const { color, values } = detail;
            values.forEach((value, index) => {
              pointCount++;

              {
                const itemHeight = 40;
                const yArr = [
                  itemHeight * -8,
                  itemHeight * -7,
                  itemHeight * -6,
                  itemHeight * -5,
                  itemHeight * -4,
                  itemHeight * -3,
                  itemHeight * -2,
                  -1 * itemHeight,
                  0,
                  itemHeight,
                  itemHeight * 2,
                  itemHeight * 3,
                  itemHeight * 4,
                  itemHeight * 5,
                  itemHeight * 6,
                  itemHeight * 7,
                  itemHeight * -7,
                  itemHeight * -6,
                  itemHeight * -5,
                  itemHeight * -4,
                  itemHeight * -3,
                  itemHeight * -2,
                  -1 * itemHeight,
                  0,
                  itemHeight,
                  itemHeight * 2,
                  itemHeight * 3,
                  itemHeight * 4,
                  itemHeight * 5,
                  itemHeight * 6,
                  itemHeight * 7
                  // itemHeight*7,itemHeight*6,itemHeight*5,itemHeight*4,itemHeight*3,itemHeight*2,36,0,-36 ,itemHeight*(-2) ,itemHeight*(-3) ,itemHeight*(-4) ,itemHeight*(-5) ,itemHeight*(-6) ,itemHeight*(-7) ,
                ];
                const rMin = 900,
                  rMax = rMin;
                const rArr = [
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin,
                  rMax,
                  rMin
                ];
                const boxHeight = 28;
                const y = yArr[pointCount % yArr.length];
                const r = rArr[pointCount % rArr.length];
                let x = pointCount % 2 ? Math.sqrt(r * r - y * y) : -1 * Math.sqrt(r * r - y * y);
                // x = x > 0 ? x : x - 14;
                group.addShape('image', {
                  attrs: {
                    width: 24,
                    height: 24,
                    x: x > 0 ? x - 24 - 200 : x + 200,
                    y: y + 21 - boxHeight / 2,
                    img: 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAAwCAYAAABXAvmHAAAAAXNSR0IArs4c6QAADctJREFUaEPtmX2M5VV5xz/n5fd2Z+beednZ9wVBbBEQaNS2CbZFmr5gsCYQ2tg2goKKAalAsVqsQCNpLUsXEVl3wdA2NRig2FCqUROBEGqoRVatxBTBwi77MrM7M/fOvff3es5pzm/2bgZ2Zl9m+cekk2zu3Xt/O/v9nOd7nuc5zxH8gv+IX3D9/D/AYRF0TnAtIRBRoervNQbIuYsCIdwbGfU3JALOiwZO+SDR/pim6RMsJVI1KFdldH5+P7n/XrwBMCcEMBB+662Iu19hqFfRJEQIi9UtsjCg9EKLkqBqEzuJpMANaTrXnETv5pupo3EiICsG8OK9cC/gzjmaeZdhDEI36G+aZL6OwBkH4/D8wuvOaUaqPg0ULhqm+4lROv5zD7JSiBUBHCa+zYgLEMkY7VZCXjURazoI1h0E2AP7mjjdwbVTonSWlihxUYv5E4U4boDF4rfso5UbRpDIaJT2yRMUuUWaBGH7CBstREjmONnAqRQXSezLBwjzOVpYbKSYv24N7ZVG4rgABuJ/cgbi24/TrMVb1KqNtIcSyjxExSlqDmScMDSccpYX1k347yylp8D2EsyaAtNLCfbvooXEeIjffTedM5/HHa+djhngMPEFTRLkxAidqEVVhKjSoI1Fr6+4NIC/do6m36ZC0iktn31V8LASVGFC2SswVRtdztMkxUYhnZVAHBPAcuLHVzEfevEGXSiCwBBO5lytLTfW2QWe9pXAWc7zf69g83TM3aWh1IYySyiNh9jPyEohjgpwNPFpRRgogiInWu/4mLT8uZAY6/jQcy/zgBd+7kn8sYKvOIcykjv2BGzVBUUlKHJJYXPUSiGOCLCk+BA1voaOjjCBJkglkSyIJixXKccNAoyBy3ZUPMQBBPMI3gTnaC5VcD+gjGPLNGxzCZnJybKS0oEs99GkwByPnZYFOJp4lRBKQ5gLoon0oHiBMY7LduzjYTSSHoImgv5CNjp7PZfoQSQcd+7XbLOCzCiyvE+xEoglAY5VvDPEIxVXKcv1h1b+Z/wLIbIGUEhyRP3qfwz2bRu4WCvuxdsJvjCl2O4RVwpxGMBhef5gtvEbdmAbJFGuiSd6XKUM1wuFMZbLdgzEj6IoUJSo95/LXzzwLJtrgABHjnvrWt4XB2xzoKzkC1MR2ynpW0t6yE6LNvaR6sRrAJYsUt7zB7ONquq1jfzKNw0fU4brGNjGe76NIkAzX/efCo2+/FfZk1f8/QM/ZAslzhcvn/vfuo73xoptdSQUd00bvkxC3++JokHeaKNnPITfE0codksC3HWA4X7OqC9SE5N0gtWYgXgE0XDGx5XhWuHFaz6w41ke5kwkXRQddL3WeuH1snfyipSQZtz1tR9zZ40gMeSY0zdwUaLY6rOTgy9NK+52ltSIBYihKdSBaZr++UbE3LUTdF9f6A4BDFZ/+27i2ZIJF6FWraUTxhitCfzK24q4WXKdgKu9o43jiv0HeHBXC8du/wSaPpphAiwBKcHl5/GSECAF9HLu/tr3+SKayotKJOZNq+pIfNFnJyfYuj9gi3OkTpHFFWWRofbvpSlyzFjAgY+sJ1sMcRjA7VOsNhWhSsg3rqcvDGGaEFIRT+R8Wgo+7Kjz/DV5xYNTfcr9C7aQTKEI0YT18yGG8JK3c20z5mofBb+TS8c9//AMd3uIyGFbLZgMeW+o2OztBNz3quJvbEy6KqVwimLXbhomJVKa4sbVTC0L4A8k+wSTvrM8dT0z/RbaW8cFxGMZH8bwGZ9trOTTRcUjRtLvdCl6EWZuzqtD0SOgQVADeJQKffGvcPXYEB/1qzUUQxTx5Tu/wz1hjBsC4oSgFfMeDbd621rNbVMJ25OSzGiKRpvqpd2M+w52jWPaH4gG7fdrIjB2Ha2sT3Px6vcUUZSzZlTxtINECP6ugkergnkvtxBkc32qvsN2SwQZ+uIz6lZCPfIc2wgIMOiLz+GjEyNc2YhgwyoQmns//xjbI9BRgI5i4lhwoYDrhSPdb3iXTdg7ZMgXRyFu0JndQvswgFtucfKOacbKkqFggt6adZR0iVxEPGy4MCjZ5iQvKLipqpi3jvkSulbQzyvyuRSblrjfP40bkojrvOe7OVv/dQf3HdzU6pJzuGLjGB/auArWT9R2uv+zD/LVWBOGEOuQhlDcLA1vKTRX9QO+IXIyhsn37SEoDzAUBPRumGT2lluEPdhvLRw6DgFkDI+P0osnKV1M7BzJWM6F0nCPELwo4LbS0pWOeSPoloKusaSpoexk2PM38YlGxJ95gH7O1kd+wFfqKCjUxWdy5aZxLt8wCR6iX/LPt32dh0JJpCWJ1sRK8kkMb640V3diviEEqcjIsmmCmTmGgpjusgD+aJiVtIZbFMOTpEIQ2ZJGYFkzWvG4g1gK/tHAk9bStYp5IejmBWkFWWop53u4C07h4z7xfP3HbPfig5LgorfxkVUjfLARggcoDA/c+xQPKYhrAE2sBedJx59KyKYlFxCwVwb0nSPvTpN024RxQNuf4paMwH1tov0FqyOBbG1gzvc7HsApGhMlV8mKawArJF8Viu9JD+DoG0sfSEVFUQiqfglljshBlxX6N07hirEGH/BZqBGDUzz87//FYyoi8PYRklha3gH8kXBIE/ClfSFbI0PfA5iUov0qo7nDrgqZurJFviSAP2k99l3WhoJIJZStkyi8hayhoWFoouQz1vIHHkJpHg4D/tOXf+3bNkmWWzJnqCqHqQzYAnX6Ov5kJOT93lK1rSyP7tjFt5QkFI5IKyLnONcZ3ucfkYp/2yv5XKXoxoq+t1D7FUKTEhSO/KIL2OtPbssCPPEEybxjdZCgRxLSZBxsRcMKhqRmaHXJp8qK9wiH1TGPBoodUpAJQ1opn94oLFSlw7kccdYa/smnOl8HOgXffGGax6UkdBB5CGc5yxRcKARSaL45FfJ5k9MVkm6i6aczMJ+SlCnViGDq/PNJlwTwlfgPH0K+NIv8nx/RFIpxrQh1i7TZQFhNwzqGLAxNVtzoSn4PgQ1DvhVIfiIUmU+izta9fWVcXezE6eNs8Ss/n/Hd/23zlGBBPI7IOn7ZGi6og6P59l7JHfgEoegKRy/v46o2SWUonGHml86mc+oY9sFLsUvWAT/neQLkrnFUeydjmWXUtxFDE2RBiPRWco4hJWlMFHzSlvw2Eqs1T4YRP7W2HmR5gNL56ixwp49zU7vk6Z2zPCNAO0Hk06o1nGYN7/I1XGke3xdyu4GeBxAhPVlgeweIq4oylsy1NjG7cQZzPtgjthJ+H/go7Ouhs72M2ZIxmxA0hsl0jNKQOEujlDTWljXEu4XEKcn3hOZFXN3nVMYDONjY5B072zznHFr6KakgcJaTjeHX/LFZKp7cF7KZip5f+cKRViWV6RLLlFIGzMZrmV0zROVX//WTi8N6IQ8w/TzCR6EVoX7+IuMIWtISNcbIggBpFQ3noyFIxis+JSp+E3BC8QMkO53DCHDIuvcU3krC9zmi/mSTs5yLQ1jFU9MBt/s+z//B0K8sppoltv4o5Gif8mZm2jnGr/7kGbhlAXwxW3wS81E4sAfVHELtmmPcVYwqR9QYJ9Uh0rcVHqKSNDZY/lI7zvOWKQzPF5a9DpysBxL+U+qGNFCsDmQ9cBSV4z9elfztIfGQVgZTzZAYQS40cxtHmen0MBPrMEut/msq8WIA/97vhfl1iAMhqtFH793N2AAiaZENICpIcDRODfmrhubX/RrPzvNiWjBjfRj8XMjn/4SJsYRTvfi04JkXSz5Xn5Z9DfG2MRjTJh6IX7ue2X6DaqLAjOzBee8vNb074pHSQ0yB7J+M9BB7XmEcaPlIJJNk2k8SFDGGBoLk9IRbxxq800nc7hlenWkvjAzHmrQ2jLFBOMRsyvd/mnGLK8iEpucMWeWPpNML4oH2upOY8eIbL2NXg11O/GERODiKfY2V/H7wEJ0mqhmhagjFqG+zfSSCBFH4LtUSA8nZLW5b1+Ttfm7ezcj87xxOiP2G3jvLsz/qcpOzCykXRb9McfXKa394ZM6L7+SYZgfjxS/l+4HOZQFevx+OBOH3RKAQhSI2ltj3T2ePcONb1vI72k8jfFGw2Bf28p0fttns+xqtSTFkpcHVnl+h+CMCHBPEQTs1Run7SyUvXkBkBMGQZPW6Br/lf8+ePk/2LFPW1TUi1342UUB/jsZi2xzPyg+icFyjxUEkBnvi0Ma2BOEY/aiBE5bQVARKoqxPpL6NEFhjMUpTOkmR9xHFLA0jKX22GWzYgeePZptjstDihxan16UgqGhaRTA8Wudzqx3aqHqctbBAFqd8ihRUfvrWnWNIGko0nRMRf1QLHQtEcz9q1zRjGJpOo1RA0RwmdbouYIciICpcp0tiSkJR1VOjzsZJZjurMCtZ+WO20JEg6jqxBzWxDvnKz2j63imwC1erapxC1hP1OoFrM1NfvVJKjO9tTjqNzoE9WF+kfJ4/Htsct4WWglhc7No5MvMd0E6SeUXL+pmQQdSdj/+p6oGJk5JyxNBmE2mssa0Ie6Qitfj/Xe79UTfxUv9w8Qhy0Dv5aHiQagbhL/KCjNhGC9GQOaaMyfwFoB7HDYQPVt0/c7xXSyuy0HKR8BD+O7/BPYh/n+5BLL5mTdYt3AkP7OLf+8bsRMQf1yZeLhL+88F9sX8/gFku5APRA+G1iBO4sV+RhV4vbnBjP/h8MdDiZwc384fCfwLCB7/j/wBvPw+pDFw7rwAAAABJRU5ErkJggg==',
                    cursor: 'pointer'
                  },
                  draggable: true
                });

                group.addShape('text', {
                  attrs: {
                    text: `${value.name}`,
                    fill: '#ffffff',
                    height: itemHeight,
                    x: x > 0 ? x - 200 : x + 200,
                    y: y + 20,
                    textAlign: x > 0 ? 'start' : 'end',
                    textBaseline: 'middle',
                    fontSize: 12,
                    fontWeight: 500
                  },
                  name: ''
                });
              }
            });
          });

          // group.addShape('text', {
          //   attrs: {
          //     text: '云启智慧全国业务分布',
          //     fill: 'white',
          //     width: 400,
          //     // height: 40,
          //     x: 0,
          //     y: height / -2 + 150,
          //     textAlign: 'center',
          //     textBaseline: 'middle',
          //     fontSize: 25,
          //     fontWeight: 900
          //   }
          // });

          const shape = group;
          this.drawText(cfg, group);
          return shape;
        }
      },
      'extendNodeName'
    );
  };

  registerNode();
  const data = {
    nodes: [
      {
        id: 'node1',
        label: 'Node 1',
        x: width / 2,
        y: height / 2,
        centerColor: '#bae7ff',
        details: [
          {
            cat: 'pv',
            values: provinceList,
            color: '#5B8FF9'
          }
        ]
      }
      // {
      //   id:'node2',
      //   label:'Node 2',
      //   x:400,
      //   y:150,
      //   centerColor:"#bae7ff",
      //   details:[
      //     {cat:'pv',values :[
      //         10086,30,40,30,30,20,30,40,30,30,
      //         20,30,40,30,30,20,30,40,30,30,
      //         20,30,40,30,30,20,30,40,30,30,
      //       ] , color:'#5B8FF9'},
      //   ]
      // }
    ],
    edges: [
      {
        source: 'node1',
        sourceKey: 'source-10086',
        target: 'node2',
        targetKey: 'target-10086'
      }
    ]
  };
  const graph = new G6.Graph({
    // 控件
    container: container,
    // 宽度
    width: width,
    // 高度
    height: height,
    // 布局
    layout: {
      type: 'dagre'
    },
    // 节点
    defaultNode: {
      type: 'lineNode',
      style: {
        width: 180,
        height: 60,
        radius: 10
      },
      labelCfg: {
        style: {
          fontSize: 20
        }
      },
      anchorPoints: [
        [0, 0],
        [1, 0],
        [0, 1],
        [1, 1]
      ]
    },
    fitCenter: true,
    // 边
    defaultEdge: {
      type: 'dice-er-edge',
      style: {
        stroke: '#e11d1d',
        lineWidth: 4,
        endArrow: false
      }
    },
    // 适应视图的大小
    fitView: true,
    // 添加渲染视图 padding
    fitViewPadding: true,
    modes: {
      default: []
    },
    nodeStateStyles: {
      hover: {
        fill: 'lightsteelblue'
      },
      click: {
        stroke: 'steelblue',
        lineWidth: 10
      }
    }
  });

  graph.read(data);

  graph.on('canvas:click', event => {
    console.log(event);
  });

  graph.on('node:click', event => {
    console.log(event);
    graph.setItemState(event.item, 'click', true);
  });

  graph.on('node:mouseenter', event => {
    console.log(event);
  });

  graph.on('item-content:mouseenter', e => {
    const { target } = e;
    target.animate(
      {
        shadowColor: 'rgba(44,104,255,0.3500)',
        shadowBlur: 10
      },
      {
        duration: 0,
        repeat: false
      }
    );
  });

  graph.on('item-content:mouseleave', e => {
    const { target } = e;
    target.animate(
      {
        shadowColor: 'rgba(44,104,255,0.0500)',
        shadowBlur: 10
      },
      {
        duration: 0,
        repeat: false
      }
    );
  });
};

const initProvinceChart = async () => {
  const mapName = activeProvinceInfo.name;

  /*
  喜欢作品的可以关注csdn https://blog.csdn.net/qq_47410017
  */

  if (!provinceChart) {
    provinceChart = echarts.init(document.getElementById('provinceChart'));
  } else {
    provinceChart.dispose();
    provinceChart = echarts.init(document.getElementById('provinceChart'));
  }

  //调用中国地图（同步）

  const dataTemp = flagList.map(item => {
    return {
      name: item.name,
      lng: item.lng,
      lat: item.lat
    };
  });

  const effectScatterList = cityList
    .filter(cityItem => !flagList.find(flagItem => flagItem.name === cityItem.name))
    .map(item => {
      return {
        name: item.name,
        value: [item.lng, item.lat, null]
      };
    });

  const option = {
    grid: {
      height: '100%',
      width: '100%'
    },
    geo: [
      {
        map: mapName,
        label: {
          normal: {
            show: false
          },
          emphasis: {
            show: false
          }
        },
        top: 'center',
        left: 'center',
        roam: false,
        zoom: 1.01,

        itemStyle: {
          normal: {
            areaColor: '#0066FF00',
            borderColor: '#ffffff00',
            borderWidth: 1.5
          },
          emphasis: {
            areaColor: '#0066FF',
            borderColor: '#ffffff00',
            borderWidth: 1.5
          }
        }
      }
    ],

    series: [
      {
        type: 'map',
        mapType: mapName,
        top: 'center',
        left: 'center',
        label: {
          normal: {
            show: false,
            position: 'inner',
            textStyle: {
              color: '#fff',
              fontSize: 14,
              fontWeight: 'bold',
              align: 'left'
            }
          },
          emphasis: {
            show: false,
            textStyle: {
              color: '#fff'
            }
          }
        },
        select: {
          itemStyle: {
            areaColor: '#0066FF',
            borderColor: '#fff',
            borderWidth: 2
          },
          label: {
            show: false
          }
        },
        itemStyle: {
          normal: {
            areaColor: {
              type: 'linear',
              x: 0,
              y: 0,
              x2: 0,
              y2: 1,
              colorStops: [
                {
                  offset: 0,
                  color: '#0066FF70' // 0% 处的颜色
                },
                {
                  offset: 1,
                  color: '#006690' // 100% 处的颜色
                }
              ],
              global: false // 缺省为 false
            },

            borderColor: '#ffffff30',
            borderWidth: 1,
            shadowColor: '#ffffff50',
            shadowBlur: 10
          },
          emphasis: {
            areaColor: {
              type: 'linear',
              x: 0,
              y: 0,
              x2: 0,
              y2: 1,
              colorStops: [
                {
                  offset: 0,
                  color: '#0066FF80' // 0% 处的颜色
                },
                {
                  offset: 1,
                  color: '#006699' // 100% 处的颜色
                }
              ],
              global: false // 缺省为 false
            },
            borderColor: '#fff',
            borderWidth: 3
          }
        }
      },

      {
        type: 'effectScatter',
        coordinateSystem: 'geo',
        data: effectScatterList,
        symbolSize: function (val) {
          return 12;
        },
        showEffectOn: 'render',
        rippleEffect: {
          brushType: 'stroke'
        },
        hoverAnimation: true,
        label: {
          normal: {
            formatter: '  {b}',
            position: 'right',
            show: true,
            color: '#fff',
            fontSize: 14,
            align: 'left',
            margin: 10
          }
        },
        itemStyle: {
          normal: {
            color: '#fff',
            shadowBlur: 10,
            shadowColor: '#fff'
          }
        }
      },

      {
        type: 'custom',
        coordinateSystem: 'geo',
        renderItem: function (params, api) {
          //自定义图标
          const item = dataTemp[params.dataIndex];
          return {
            type: 'group',
            children: [
              {
                type: 'rect',
                name: item.name,
                style: {
                  fill: '#00000000',
                  stroke: '#00000000',
                  lineWidth: 0
                },
                shape: {
                  x: api.coord([item.lng, item.lat])[0] - 54,
                  y: api.coord([item.lng, item.lat])[1] - 88,
                  width: 80,
                  height: 80,
                  r: 5
                },
                textContent: {
                  style: {
                    text: item.name,
                    color: '#fff',
                    fontSize: 14,
                    fontWeight: 'bold',
                    align: 'left'
                  }
                },
                textConfig: {
                  position: 'inside'
                }
              },
              {
                type: 'image',
                name: item.name,
                style: {
                  image: redFlagImg,
                  x: api.coord([item.lng, item.lat])[0] - 24,
                  y: api.coord([item.lng, item.lat])[1] - 40,
                  width: 48,
                  height: 48
                }
              },

              {
                type: 'image',
                name: item.name,
                style: {
                  image: redFlagBg,
                  x: api.coord([item.lng, item.lat])[0] - 20,
                  y: api.coord([item.lng, item.lat])[1] - 60,
                  width: 54,
                  height: 24
                }
              }
            ]
          };
        },
        label: {
          normal: {
            show: false,
            position: 'inside',
            color: '#000'
          }
        },
        z: 90,
        data: dataTemp
      }
    ]
  };

  provinceChart.setOption(option);

  //点击事件
  provinceChart.on('click', function (e) {
    //高亮鼠标移入的位置
    provinceChart.dispatchAction({
      type: 'select ',
      seriesIndex: 0,
      dataIndex: e.dataIndex
    });
  });
};

const loadProvinceData = async () => {
  const result = await api.axios.get(
    `/api/v1/screen/business/list-by-province-id?current=${pageInfo.current}&pageSize=${pageInfo.size}&provinceId=${activeProvinceInfo.id}`
  );
  caseList.length = 0;
  caseList.push(...result.data.records);
  pageInfo.pages = result.data.pages;
};

const loadCityList = async () => {
  const result = await api.axios.get(`/api/v1/screen/regions/city/list?provinceId=${activeProvinceInfo.id}`);
  cityList.length = 0;
  cityList.push(...result.data);
};

const loadFlagData = async () => {
  const result = await api.axios.get(`/api/v1/screen/business/city/flag?provinceId=${activeProvinceInfo.id}`);
  flagList.length = 0;
  flagList.push(...result.data);
};

const initProvince = async () => {
  if (!provinceChart) {
    provinceChart = echarts.init(document.getElementById('provinceChart'));
  } else {
    provinceChart.dispose();
    provinceChart = echarts.init(document.getElementById('provinceChart'));
  }
  caseList.length = 0;

  const mapName = activeProvinceInfo.name;
  await Promise.all([loadProvinceMapJson(mapName, provinceToMap[mapName]), loadFlagData(), loadCityList(), loadProvinceData()]);
  initProvinceChart();
};

const initChinaChart = async () => {
  /*
  喜欢作品的可以关注csdn https://blog.csdn.net/qq_47410017
  */

  if (!chinaChart) {
    chinaChart = echarts.init(document.getElementById('main'));
  }

  const mapName = 'China';

  //加载时的文字提示
  chinaChart.showLoading({ text: '正在加载数据' }); //增加等待提示
  await loadProvinceMapJson(mapName, provinceToMap[mapName]);
  chinaChart.hideLoading();

  const dataTemp = provinceList.map(item => {
    return {
      name: item.name,
      lng: item.lng,
      lat: item.lat
    };
  });

  const option = {
    // backgroundColor: '#00094A',
    // tooltip: {
    //   trigger: 'item',
    //   axisPointer: {
    //     type: 'shadow'
    //   }
    // },

    grid: {
      height: '100%',
      width: '100%'
    },
    geo: [
      {
        map: 'China',
        label: {
          normal: {
            show: false
          },
          emphasis: {
            show: false
          }
        },
        top: 'center',
        left: 'center',
        roam: false,
        zoom: 1.01,

        itemStyle: {
          normal: {
            areaColor: '#0066FF00',
            borderColor: '#ffffff00',
            borderWidth: 1.5
          },
          emphasis: {
            areaColor: '#0066FF',
            borderColor: '#ffffff00',
            borderWidth: 1.5
          }
        }
      }
    ],

    series: [
      {
        type: 'map',
        mapType: 'China',
        top: 'center',
        left: 'center',
        label: {
          normal: {
            show: false,
            position: 'inner',
            textStyle: {}
          },
          emphasis: {
            show: false
          }
        },
        select: {
          itemStyle: {
            areaColor: '#0066FF',
            borderColor: '#fff',
            borderWidth: 2
          },
          label: {
            show: false
          }
        },
        itemStyle: {
          normal: {
            areaColor: {
              type: 'linear',
              x: 0,
              y: 0,
              x2: 0,
              y2: 1,
              colorStops: [
                {
                  offset: 0,
                  color: '#0066FF70' // 0% 处的颜色
                },
                {
                  offset: 1,
                  color: '#006690' // 100% 处的颜色
                }
              ],
              global: false // 缺省为 false
            },

            borderColor: '#ffffff30',
            borderWidth: 1,
            shadowColor: '#ffffff50',
            shadowBlur: 10
          },
          emphasis: {
            areaColor: '#0066FF',
            borderColor: '#fff',
            borderWidth: 3
          }
        }
      },
      {
        type: 'custom',
        coordinateSystem: 'geo',
        renderItem: function (params, api) {
          //自定义图标
          const item = dataTemp[params.dataIndex];
          return {
            type: 'group',
            children: [
              {
                type: 'rect',
                name: item.name,
                style: {
                  fill: '#00000000',
                  stroke: '#00000000',
                  lineWidth: 0
                },
                shape: {
                  x: api.coord([item.lng, item.lat])[0] - 54,
                  y: api.coord([item.lng, item.lat])[1] - 88,
                  width: 80,
                  height: 80,
                  r: 5
                },
                textContent: {
                  style: {
                    text: item.name,
                    color: '#fff',
                    fontSize: 14,
                    fontWeight: 'bold',
                    align: 'left'
                  }
                },
                textConfig: {
                  position: 'inside'
                }
              },
              {
                type: 'image',
                name: item.name,
                style: {
                  image: redFlagImg,
                  x: api.coord([item.lng, item.lat])[0] - 24,
                  y: api.coord([item.lng, item.lat])[1] - 40,
                  width: 48,
                  height: 48
                }
              },

              {
                type: 'image',
                name: item.name,
                style: {
                  image: redFlagBg,
                  x: api.coord([item.lng, item.lat])[0] - 20,
                  y: api.coord([item.lng, item.lat])[1] - 60,
                  width: 54,
                  height: 24
                }
              }
            ]
          };
        },
        label: {
          normal: {
            show: false,
            position: 'inside',
            color: '#000'
          }
        },
        z: 90,
        data: dataTemp
      }
    ]
  };

  chinaChart.setOption(option);

  //点击事件
  chinaChart.on('click', function (e) {
    //高亮鼠标移入的位置
    chinaChart.dispatchAction({
      type: 'select ',
      seriesIndex: 0,
      dataIndex: e.dataIndex
    });

    const chooseInfo = provinceList.find(item => {
      //我们根据名字来判断是否选择一种
      return item.name == e.name;
    });
    if (chooseInfo) {
      activeProvinceInfo = chooseInfo;
      activePage.value = 1;
    }
  });
};

const initChina = async () => {
  await initChinaChart();
};

watch(
  activePage,
  (newvalue, oldvalue) => {
    if (newvalue === 0) {
      //
    } else if (newvalue === 1 && oldvalue === 0) {
      initProvince();
    } else if (newvalue === 1 && oldvalue === 2) {
      //
    } else if (newvalue === 2) {
      //
    }
  },
  { immediate: false }
);

// watch(
//   pageInfo,
//   (newvalue, oldvalue) => {
//     console.log(newvalue.current, oldvalue.current);
//     if (newvalue.current !== oldvalue.current) {
//       loadProvinceData();
//     }
//   },
//   {
//     deep: true,
//     immediate: false
//   }
// );

nextTick(async () => {
  initTree();
  initChina();
});
</script>

<template>
  <div v-show="activePage === 0" id="quanguo" class="min-w-full min-h-full w-full h-full">
    <div id="main" class="min-w-full min-h-full w-full h-full z-10"></div>
    <div id="container" class="min-h-full h-full"></div>
    <div class="center-circle-bg"></div>
    <div class="head">云启智慧全国业务分布</div>
    <div class="footer"></div>
  </div>
  <div v-show="activePage === 1" v-if="activeProvinceInfo" id="shengfen" class="min-w-full min-h-full w-full h-full">
    <div id="provinceChart" class="min-w-full min-h-full w-full h-full z-10"></div>
    <div id="case-list" class="z-10">
      <div class="case-title mb-8">
        <div class="case-title-bg"></div>
        <div class="zhongwen">{{ activeProvinceInfo.name }}</div>
        <div class="pinyin">{{ activeProvinceInfo.pinYin }}</div>
      </div>
      <div class="case-divider"></div>
      <div
        v-for="item in caseList"
        :key="item"
        class="flex items-center case-item mt-8px"
        @click="
          activeCaseInfo = item;
          activePage = 2;
        "
      >
        <div class="ml-35px">{{ item.projectName }}</div>
      </div>
      <div v-if="caseList && caseList.length" class="case-page flex items-center mx-auto mt-4 justify-around">
        <div
          class="pre"
          @click="
            pageInfo.current = pageInfo.current - 1 > 1 ? pageInfo.current - 1 : 1;
            loadProvinceData();
          "
        ></div>
        <div>{{ pageInfo.current }}/{{ pageInfo.pages }}</div>
        <div
          class="next"
          @click="
            pageInfo.current = pageInfo.current + 1 > pageInfo.pages ? pageInfo.pages : pageInfo.current + 1;
            loadProvinceData();
          "
        ></div>
      </div>
    </div>
    <div class="center-circle-bg"></div>
    <div class="head">云启智慧全国业务分布</div>
    <div class="footer"></div>
    <div class="back-btn z-99" @click="activePage = 0"></div>
  </div>
  <div v-show="activePage === 2" v-if="activeCaseInfo" id="intro" class="min-w-full min-h-full w-full h-full flex flex-col items-center">
    <div class="head">云启智慧全国业务分布</div>
    <div class="title-bg flex justify-between mt-29 items-center">
      <div class="title ml-8">{{ activeCaseInfo.projectName }}</div>
    </div>
    <div class="content-box flex justify-between">
      <div class="center-circle-bg"></div>
      <div class="w-4/7 z-10 flex flex-col h-full">
        <div class="detailed-intro mt-6 ml-20"></div>
        <div class="product-intro mt-18">
          <div class="ml-24 title-bg"></div>
          <div class="content ml-32 mt-8">
            {{ activeCaseInfo.introduction }}
          </div>
        </div>
        <div v-if="activeCaseInfo.advantages" class="product-advantage mt-24">
          <div class="ml-24 title-bg"></div>
          <div class="advantages-item ml-40 mt-8 flex" v-for="(item, index) in activeCaseInfo.advantages.split(';;')" :key="index">
            <div></div>
            <span class="ml-4">{{ item }}</span>
          </div>
        </div>
      </div>
      <div v-if="activeCaseInfo.imageUrl" class="w-3/7 z-10 flex min-h-full h-full">
        <div class="product-img mt-7"></div>
        <div class="flex flex-col justify-evenly ml-3 py-3">
          <img v-for="(item, index) in activeCaseInfo.imageUrl.split(';;')" :key="index" class="intro-img" :src="`${apiHost}${item}`" />
        </div>
      </div>
    </div>
    <div class="footer"></div>
    <div class="back-btn z-99" @click="activePage = 1"></div>
  </div>
</template>

<style lang="scss">
body {
  overflow: hidden;
}

#quanguo {
  position: absolute;
  width: 100%;
  height: 100%;
  background-image: url('@/assets/images/zt/map/web/quanguo/quanguo.png');
  background-size: 100% 100%;

  .center-circle-bg {
    margin-top: 5px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    width: 1000px;
    height: 1000px;
    background-image: url('@/assets/images/zt/map/web/center-circle-bg.png');
    background-size: 100% 100%;
    z-index: 1;
  }

  .head {
    width: 460px;
    height: 48px;
    font-size: 40px;
    font-family: AlimamaShuHeiTi;
    color: #ffffff;
    line-height: 56px;
    letter-spacing: 6px;
    text-shadow: 0px 1px 2px rgba(2, 49, 100, 0.6);
    text-align: center;

    position: absolute;
    left: 50%;
    top: 30px;
    transform: translate(-50%, -50%);
  }

  .footer {
    width: 1920px;
    height: 56px;
    font-size: 40px;
    font-family: AlimamaShuHeiTi;
    color: #ffffff;
    line-height: 56px;
    letter-spacing: 6px;
    text-shadow: 0px 1px 2px rgba(2, 49, 100, 0.6);
    text-align: center;

    position: absolute;
    left: 50%;
    bottom: 0px;
    transform: translate(-50%, -50%);

    background-image: url('@/assets/images/zt/map/web/footer-bg.png');
    background-size: 100% 100%;
  }

  #main {
    text-align: center;
    border-radius: 20px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    // background-image: url('@/assets/images/zt/map/bg.png');
    // background-size: 100% 100%;
  }

  #container {
    margin-left: 0px;
    text-align: center;
    border-radius: 20px;
    width: 92%;
    height: 350px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }
}

#shengfen {
  position: absolute;
  width: 100%;
  height: 100%;
  background-image: url('@/assets/images/zt/map/web/province/province.png');
  background-size: 100% 100%;

  .center-circle-bg {
    position: absolute;
    left: 42%;
    top: 50%;
    transform: translate(-50%, -50%);
    width: 1000px;
    height: 1000px;
    background-image: url('@/assets/images/zt/map/web/center-circle-bg.png');
    background-size: 100% 100%;
    z-index: 1;
  }

  .head {
    width: 460px;
    height: 48px;
    font-size: 40px;
    font-family: AlimamaShuHeiTi;
    color: #ffffff;
    line-height: 56px;
    letter-spacing: 6px;
    text-shadow: 0px 1px 2px rgba(2, 49, 100, 0.6);
    text-align: center;

    position: absolute;
    left: 50%;
    top: 30px;
    transform: translate(-50%, -50%);
  }

  .footer {
    width: 1920px;
    height: 56px;

    position: absolute;
    left: 50%;
    bottom: 0px;
    transform: translate(-50%, -50%);

    background-image: url('@/assets/images/zt/map/web/footer-bg.png');
    background-size: 100% 100%;
  }

  .back-btn {
    position: absolute;
    left: 50%;
    bottom: 0px;
    transform: translate(-50%, -50%);

    width: 116px;
    height: 36px;
    background-image: url('@/assets/images/zt/map/web/back-btn.png');
    background-size: 100% 100%;
  }

  #provinceChart {
    text-align: center;
    border-radius: 20px;
    position: absolute;
    left: 42%;
    top: 50%;
    transform: translate(-50%, -50%);
    // background-image: url('@/assets/images/zt/map/bg.png');
    // background-size: 100% 100%;
  }

  #case-list {
    text-align: left;
    width: 262px;
    height: 350px;
    position: absolute;
    right: -3%;
    top: 30%;
    transform: translate(-50%, -50%);

    .case-title {
      width: 229px;
      height: 67px;
      .case-title-bg {
        margin-top: 24px;

        position: absolute;
        width: 229px;
        height: 67px;
        background-image: url('@/assets/images/zt/map/web/intro/case-title-bg.png');
        background-size: 100% 100%;
      }
      .zhongwen {
        margin-top: -10px;
        margin-left: 36px;

        position: absolute;
        height: 63px;
        font-size: 48px;
        font-family: jiangxizhuokai-Regular, jiangxizhuokai;
        font-weight: 400;
        color: #ffffff;
        line-height: 63px;
        letter-spacing: 2px;
      }
      .pinyin {
        margin-top: 55px;
        margin-left: 150px;

        position: absolute;
        height: 22px;
        font-size: 16px;
        font-family: PingFangSC-Regular, PingFang SC;
        font-weight: 400;
        color: #ffc97a;
        line-height: 22px;
      }
    }

    .case-divider {
      width: 248px;
      height: 7px;
      background-image: url('@/assets/images/zt/map/web/province/case-divider.png');
      background-size: 100% 100%;
    }

    .case-item {
      width: 262px;
      height: 60px;
      background-image: url('@/assets/images/zt/map/web/province/case-item-bg.png');
      background-size: 100% 100%;
      div {
        width: 220px;
        height: 44px;
        font-size: 14px;
        font-family: PingFangSC-Regular, PingFang SC;
        font-weight: 400;
        color: #ffffff;
        line-height: 22px;
      }
    }

    .case-page {
      width: 160px;
      height: 22px;
      font-size: 14px;
      font-family: PingFangSC-Medium, PingFang SC;
      font-weight: 500;
      color: #ffffff;
      line-height: 22px;
      .pre {
        width: 34px;
        height: 34px;
        background-image: url('@/assets/images/zt/map/web/province/page-pre.png');
        background-size: 100% 100%;
      }
      .next {
        width: 34px;
        height: 34px;
        background-image: url('@/assets/images/zt/map/web/province/page-next.png');
        background-size: 100% 100%;
      }
    }
  }
}

#intro {
  position: absolute;
  width: 100%;
  height: 100%;
  background-image: url('@/assets/images/zt/map/web/intro/intro.png');
  background-size: 100% 100%;
  color: white;

  .title-bg {
    width: 1840px;
    height: 62px;
    background-image: url('@/assets/images/zt/map/web/intro/title.png');
    background-size: 100% 100%;
  }

  .head {
    width: 460px;
    height: 48px;
    font-size: 40px;
    font-family: AlimamaShuHeiTi;
    color: #ffffff;
    line-height: 56px;
    letter-spacing: 6px;
    text-shadow: 0px 1px 2px rgba(2, 49, 100, 0.6);
    text-align: center;

    position: absolute;
    left: 50%;
    top: 30px;
    transform: translate(-50%, -50%);
  }

  .title {
    width: 704px;
    height: 45px;
    font-size: 32px;
    font-family: PingFangSC-Semibold, PingFang SC;
    font-weight: 600;
    color: #f0faff;
    line-height: 24px;
    text-shadow: 0px 0px 12px rgba(51, 133, 255, 0.7);
  }

  .content-box {
    width: 1840px;
    height: 820px;
    background-image: url('@/assets/images/zt/map/web/intro/content-bg.png');
    background-size: 100% 100%;
    .center-circle-bg {
      margin-top: 5px;
      position: absolute;
      left: 50%;
      top: 54%;
      transform: translate(-50%, -50%);
      width: 880px;
      height: 880px;
      background-image: url('@/assets/images/zt/map/web/center-circle-bg.png');
      background-size: 100% 100%;
      z-index: 1;
    }
  }

  .detailed-intro {
    width: 456px;
    height: 90px;
    background-image: url('@/assets/images/zt/map/web/intro/detailed-intro.png');
    background-size: 100% 100%;
  }

  .product-intro {
    .title-bg {
      width: 620px;
      height: 55px;
      background-image: url('@/assets/images/zt/map/web/intro/product-intro.png');
      background-size: 100% 100%;
    }
    .content {
      width: 760px;
      height: 104px;
      font-size: 18px;
      font-family: PingFangSC-Regular, PingFang SC;
      font-weight: 400;
      color: #cce0ff;
      line-height: 26px;
    }
  }

  .product-advantage {
    .title-bg {
      width: 620px;
      height: 55px;
      background-image: url('@/assets/images/zt/map/web/intro/product-advantage.png');
      background-size: 100% 100%;
    }
    .advantages-item {
      width: 740px;
      height: 26px;
      font-size: 18px;
      font-family: PingFangSC-Regular, PingFang SC;
      font-weight: 400;
      color: #cce0ff;
      line-height: 26px;
      div {
        width: 26px;
        height: 26px;
        background-image: url('@/assets/images/zt/map/web/intro/advantages-item-bg.png');
        background-size: 100% 100%;
      }
    }
  }

  .product-img {
    width: 47px;
    height: 263px;
    background-image: url('@/assets/images/zt/map/web/intro/product-img.png');
    background-size: 100% 100%;
  }

  .intro-img {
    width: 654px;
    height: 368px;
    border: 2px solid #3385ff;
  }

  .footer {
    width: 1920px;
    height: 56px;

    position: absolute;
    left: 50%;
    bottom: 0px;
    transform: translate(-50%, -50%);

    background-image: url('@/assets/images/zt/map/web/footer-bg.png');
    background-size: 100% 100%;
  }

  .back-btn {
    position: absolute;
    left: 50%;
    bottom: 0px;
    transform: translate(-50%, -50%);

    width: 116px;
    height: 36px;
    background-image: url('@/assets/images/zt/map/web/back-btn.png');
    background-size: 100% 100%;
  }
}
</style>

<route lang="yml">
meta:
  layout: blank
</route>

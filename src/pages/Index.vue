<template>
  <q-page class="q-pa-md">
    <div v-if="beznal">
      <p>Банк покупает у меня Евро за Рубли : {{ euro.buyValue }}</p>
      <p>Покупка предыдущая : {{ euro.buyValuePrev }}</p>
      <p>Банк продает мне Евро : {{ euro.sellValue }}</p>
      <p>Продажа предыдущая : {{ euro.sellValuePrev }}</p>

      -------------------------------

      <p>Я купил {{ my_euro }} евро у банка<br/> по курсу {{ rate }} <br/> за {{ my_rubles }} рублей</p>
      <p>Сейчас мои дела обстоят так:</p>
      <p>Если я их буду продавать банку то
        <span v-if="vigodno_prodavat" style="color: green">выйграю {{ raznica_v_prodage_now_rub }} рублей</span>
        <span v-else style="color: red;">проиграю {{ raznica_v_prodage_now_rub * (-1) }} рублей</span>
      </p>
      <p>Если бы я купил евро сейчас, то получил бы {{ my_rubles_buy_now_euro }} евро</p>
      <p>
        <span v-if="moy_rate_vigonee_now" style="color: #00d300">Молодец что купил, я выигрываю {{(my_euro - my_rubles_buy_now_euro).toFixed(2) }} евро</span>
        <span v-else style="color: red">проигрываю {{(my_rubles_buy_now_euro - my_euro).toFixed(2) }} евро</span>
      </p>
    </div>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data(){
    return {
      my_euro: 114.92,
      rate: 86.69,

      beznal: {}
    }
  },
  created() {
    this.$axios.get("https://www.sberbank.ru/portalserver/proxy/?pipe=shortCachePipe&url=http://localhost/rates-web/rateService/rate/current%3FregionId%3D77%26currencyCode%3D840%26currencyCode%3D978%26rateCategory%3Dbeznal")
    .then(res=>{
      this.beznal = res.data.beznal;
    })
  },
  computed:{
    euro(){
      if ( this.beznal.hasOwnProperty("978") ){
        return this.beznal['978'][0];
      }
    },
    my_rubles(){
      return this.my_euro * this.rate;
    },
    moy_rate_vigonee_now(){
      return this.rate < this.euro.sellValue;
    },
    vigodno_prodavat(){
      return this.euro.buyValue > this.rate;
    },
    my_euro_now_buy_for_rub(){
      // Мои евро сейчас стоят столько рублей
      return this.my_euro * this.euro.buyValue;
    },
    raznica_v_prodage_now_rub(){
      // Разница между теми рублями за которые можно купить мои евро и теми за которые я их и приобрёл
      const rub = this.my_euro_now_buy_for_rub - this.my_rubles;
      return rub.toFixed(2);
    },
    my_rubles_buy_now_euro(){
      // Если бы за те рубли сейчас купить в евро
      return (this.my_rubles / this.euro.sellValue).toFixed(2);
    }
  }
}
</script>

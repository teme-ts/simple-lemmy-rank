<template>
  <div class="add-rank">
    <div class="title">
      ランキング登録
    </div>
    <div class="rank-name-container">
      <div class="rank-name">
        <label class="" for="rank-name">ランキング名</label>
        <input class="input" name="rank-name" v-model="rank.name">
      </div>
      <div class="evaluation-container">
        <div class="evaluation-content">
          <label class="evaluation-label" for="rank-evaluation-point1">評価ポイント1:</label><br>
          <input class="input" name="rank-evaluation-point1" v-model="rank.evaluationPoints[0]">
        </div>
        <div class="evaluation-content">
          <label class="evaluation-label" for="rank-evaluation-point2">評価ポイント2:</label><br>
          <input class="input" name="rank-evaluation-point2" v-model="rank.evaluationPoints[1]">
        </div>
        <div class="evaluation-content">
          <label class="evaluation-label" for="rank-evaluation-point3">評価ポイント3:</label><br>
          <input class="input" name="rank-evaluation-point3" v-model="rank.evaluationPoints[2]">
        </div>
        <div class="evaluation-content">
          <label class="evaluation-label" for="rank-evaluation-point4">評価ポイント4:</label><br>
          <input class="input" name="rank-evaluation-point4" v-model="rank.evaluationPoints[3]">
        </div>
        <div class="evaluation-content">
          <label class="evaluation-label" for="rank-evaluation-point5">評価ポイント5:</label><br>
          <input class="input" name="rank-evaluation-point5" v-model="rank.evaluationPoints[4]">
        </div>
      </div>
      <button class="add-button" @click="addRank">登録</button>
    </div>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import {firestore} from '../firebase'

interface RankTemplate{
  name: string;
  evaluationPoints: string[];
}

@Options({
  props: {
    msg: String
  }
})
export default class HelloWorld extends Vue {
  msg!: string
  private ranks: any[] = []
  private rank: RankTemplate = {
    name:'',
    evaluationPoints:['','','','',''],
  }
  created(){
    firestore.collection('ranks').get().then((snapshot)=>{
      snapshot.forEach((doc)=>{
          this.ranks.push(doc.data())
      })
      console.log(snapshot.docs.toString())
    })
  }

  addRank(){
    firestore.collection('ranks').doc(this.rank.name).set({
      name:this.rank.name,
      evaluationPoints:this.rank.evaluationPoints
    }).then(()=>{
      alert('登録完了！')
    }).catch((error)=>{
      alert(`登録失敗...。`)
    })
    /*firestore.collection('ranks').doc(this.rank.name).set({
      name:this.rank.name,
      evaluationPoints:this.rank.evaluationPoints
    }).then(()=>{
      alert('登録完了！')
    }).catch((error)=>{
      alert(`登録失敗...。`)
    })*/
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.input{
  border:1px solid black;
}

.add-rank{
  display: flex;
  justify-content: center;

  flex-wrap: wrap;
}

.rank-name-container{
  border:1px solid gray;
  border-radius: 15px;

  width: 90%;
}
.rank-name{
  border-bottom: 1px solid gray;

  padding: 0.5em 1em;
}

.evaluation-container{
  text-align: left;
}

 .evaluation-content{
   margin: 0.5em 1em;
 }

.evaluation-label{
  width: 30%;
  margin-left: 0;
  margin-right: auto;
}

.add-button{
  color:white;
  border: 1px solid #0a6bbb;
  background-color: #0a6bbb;

  padding:0.1em 1.5em;
}
</style>

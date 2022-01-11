<template>
   <div class="test" style="display:flex">
     <div class="list_row" style="margin-left:">
       <div class="col-md-6">
        <h4>Trắc nhiệm Vật lí 1 chương 3</h4>
        <ul class="list-group">
          <li class="list-group-item" :class="{ active: index == currentIndex }"
          v-for="(exambank, index) in exambanks"
          v-bind:key="exambank.key">

            <div class="number_question"
            >Câu {{index + 1}}: {{ exambank.title }}</div>
          
            <div class="question_content"></div>

            <div :class="exambank.selected === exambank.choices[0] ? 'active': ''" 
            @click="[exambank.selected = exambank.choices[0]]"><span class="label">A.</span> {{exambank.choices[0]}}</div>

            <div :class="exambank.selected === exambank.choices[1] ? 'active': ''"  
            @click="[exambank.selected = exambank.choices[1]]"><span class="label">B.</span> {{exambank.choices[1]}}</div>

            <div :class="exambank.selected === exambank.choices[2] ? 'active': ''"  
            @click="[exambank.selected = exambank.choices[2]]"><span class="label">C.</span> {{exambank.choices[2]}}</div>

            <div :class="exambank.selected === exambank.choices[3] ? 'active': ''"  
            @click="[exambank.selected = exambank.choices[3]]"><span class="label">D.</span> {{exambank.choices[3]}}</div>

            </li>
        
        </ul>
      </div>
     </div>
    <div class="right">
                <div class="rightcontainer">
                        <div class="timechoice">Chọn thời gian</div>
                        <div class="choice" style="display: flex;">
                        <div class="time1" v-if="on1"><button @click="countDownTimer1" >60 phút</button></div>
                        <div v-else></div>
                        <div class="time2" v-if="on2"><button @click="countDownTimer2" >45 phút</button></div>
                        <div v-else></div>
                        <div class="time3" v-if="on3"><button @click="countDownTimer3" >30 phút</button></div>
                        <div v-else></div>
                        </div>
                        <div class="timepost">Thời gian còn lại</div>
                        <div class="clock" >
                            <div id="base-timer-label" class="base-timer__label">{{minutes}}:{{seconds}}</div>
                        </div>
                        <div class="checkanswer">
                            <div class="answeritem" id="01" @click="scroll1" >
                              <span>1</span>
                              <span v-if="turn_on" class="space">C</span>
                            </div>
                            
                            <div class="answeritem" id="02" @click="scroll2" >
                              <span>2</span>
                              <span v-if="turn_on" class="space">D</span>
                            </div>
                            
                            <div class="answeritem" id="03" @click="scroll3" >
                              <span>3</span>
                              <span v-if="turn_on" class="space">D</span>
                            </div>
                            
                            <div class="answeritem" id="04" @click="scroll4" >
                              <span>4</span>
                              <span v-if="turn_on" class="space">C</span>
                            </div>
                            
                            <div class="answeritem" id="05" @click="scroll5" >
                              <span>5</span>
                              <span v-if="turn_on" class="space">B</span>
                            </div>
                            
                            <div class="answeritem" id="06" @click="scroll6" >
                              <span>6</span>
                              <span v-if="turn_on" class="space">D</span>
                            </div>
                            
                            <div class="answeritem" id="07" @click="scroll7" >
                              <span>7</span>
                              <span v-if="turn_on" class="space">C</span>
                            </div>
                            
                            <div class="answeritem" id="08" @click="scroll8" >
                              <span>8</span>
                              <span v-if="turn_on" class="space">D</span>
                            </div>
                            
                            <div class="answeritem" id="09" @click="scroll9" >
                              <span>9</span>
                              <span v-if="turn_on" class="space">D</span>
                            </div>
                            
                            <div class="answeritem" id="010" @click="scroll10" >
                              <span>10</span>
                              <span v-if="turn_on" class="space">B</span>
                            </div>
                            
                        </div>

                        <button class="m-3 btn btn-sm btn-danger" @click="submit()"  style="width: 75%">Submit</button>

                        <div v-if="turn_on" class="result-wrap" >You scored {{marks()}} Points</div>
                        <div v-else></div>

                        <div class="timechoice">Quy định làm bài</div>

                        <div class="rule">
                          <div class="rule_container">
                            <div>- Mỗi câu đúng sẽ được 10 points</div>
                            <div>- Có thể chọn thời gian làm bài nếu muốn</div>
                            <div>- Ấn Submit để nộp bài và kết quả sẽ hiện ra</div>
                            <div>- Đáp án sẽ hiện ra khi ấn Submit</div>
                          </div>
                        </div>
                        
                         
                </div>
      </div> 
</div>

  
</template>
<script>
import ExamBankDataService from "../services/ExamBankDataService";
import ResultDataService from "../services/ResultDataService";

export default {
  
  data() {
    return {
      exambanks: [
       {
        title: "Tấm kim loại phẳng, đồng chất, khối lượng phân bố đều, hình bán nguyệt, đường kính AB = 24cm. Khối tâm G của tấm kim loại nằm trên trục đối xứng của nó và cách tâm O một đoạn",
        choices: [
            "6cm",
            "4cm",
            "5,1cm",
            "0 cm"
        ],
        answer: "5,1cm",
        selected: null,
       },
       {
        title: "Quả cầu đặc, tâm O, bán kính R = 14 cm, đồng chất, khối lượng phân bố đều, bị khoét một lỗ hổng cũng có dạng hình cầu, bán kính r = 7cm. Tâm O’ của lỗ cách tâm O của quả cầu một đoạn d = 7cm. Khối tâm G của phần còn lại nằm trên đường thẳng nối O với O’ và",
        choices: [
            "nằm trong đoạn OO’, cách O 0,5 cm",
            "nằm trong đoạn OO’, cách O 1 cm",
            "nằm ngoài đoạn OO’, cách O 0,5 cm",
            "nằm ngoài đoạn OO’, cách O 1 cm"
        ],
        answer: "nằm ngoài đoạn OO’, cách O 1 cm",
        selected: null,
       },
       {
        title: "Một đĩa tròn mỏng đồng chất bán kính R, khối lượng phân bồ đều, bị khóet một lỗ cũng có dạng hình tròn bán kính R/2. Tâm O’ của lỗ cách tâm O của đĩa một đoạn R/2. Khối tâm G của phần còn lại nằm trên đường thẳng nối O với O’, ngoài đoạn OO’ và cách tâm O một khoảng",
        choices: [
            "x = R/8",
            "x = R/3",
            "x = R/4",
            "x = R/6"
        ],
        answer: "x = R/6",
        selected: null,
       },
       {
        title: "Vật thể có dạng khối hình nón đồng chất, khối lượng phân bố đều, đường cao 12cm thì khối tâm của vật nằm trên trục của hình nón và cách đáy một khoảng",
        choices: [
            "6cm",
            "4cm",
            "3cm",
            "2cm"
        ],
        answer: "3cm",
        selected: null,
       },
       {
        title: "Một bánh mài đang quay với vận tốc 300 vòng/phút thì bị ngắt điện và nó quay chậm dần đều. Sau đó một phút, vận tốc còn 180vòng/phút. Tính số vòng nó đã quay trong thời gian đó.",
        choices: [
            "120 vòng",
            "240 vòng",
            "60 vòng",
            "180 vòng"
        ],
        answer: "240 vòng",
        selected: null,
       },
       {
        title: "Có 4 chất điểm khối lượng bằng nhau và bằng m, đặt tại 4 đỉnh của hình vuông ABCD, cạnh a. Mômen quán tính của hệ này đối với trục quay đi qua tâm hình vuông và vuông góc với mặt phẳng hình vuông là ",
        choices: [
            "4 ma2",
            "3 ma2",
            "2 ma2",
            "ma2"
        ],
        answer: "ma2",
        selected: null,
       },
       {
        title: "Một vô lăng hình đĩa tròn đồng chất, có khối lượng m = 10 kg, bán kính R = 20 cm, đang quay với vận tốc 240 vòng/phút thì bị hãm đều và dừng lại sau 20 giây. Độ lớn của mômen hãm là ",
        choices: [
            "0,13 Nm",
            "0,50 Nm",
            "0,25 Nm",
            "1 Nm"
        ],
        answer: "0,25 Nm",
        selected: null,
       },
       {
        title: "Bánh mài hình đĩa đồng chất, khối lượng m = 500g, bán kính R = 20cm đang quay với vận tốc 480vòng/phút thì bị hãm đều lại. Tính mômen của lực hãm để bánh mài quay thêm 100 vòng nữa thì dừng.",
        choices: [
            "1Nm",
            "0.1Nm",
            "10Nm",
            "0.02Nm"
        ],
        answer: "0.02Nm",
        selected: null,
       },
       {
        title: "Một cái đĩa đồng chất, bán kính R = 24cm, dao động trong mặt phẳng thẳng đứng, quanh một trục nằm ngang đi qua một điểm trên mép đĩa. Tính chu kì dao động nhỏ của đĩa (lấy g = 9,8 m/s2, 2 = 9,8).",
        choices: [
            "1,20s",
            "0,69",
            "7,53s",
            "3,74s"
        ],
        answer: "3,74s",
        selected: null,
       },
       {
        title: "Hai đĩa tròn giống hệt nhau. Một cái giữ cố định, còn cái thứ II tiếp xúc ngoài và lăn không trượt xung quanh chu vi của đĩa I. Hỏi khi đĩa II trở về đúng điểm xuất phát ban đầu thì nó đã quay xung quanh tâm của nó được mấy vòng?",
        choices: [
            "1 vòng",
            "2 vòng",
            "3 vòng",
            "4 vòng"
        ],
        answer: "2 vòng",
        selected: null,
       },
       
      //  {
      //   title: "212",
      //   choices: [
      //       "11",
      //       "21",
      //       "31",
      //       "41"
      //   ],
      //   answer: "41",
      //   selected: null,
      //  },
      ],
      result: {
        marks: "",
        username:"",
        subject:"Vật lí 1 Chương 3",
      },
      // submitted: false,
      // exambanks: [],
      currentQuestion: null,
      currentIndex: -1,
      title: "",
      turn_on: null,
      countDown1 : 3600,
      countDown2 : 2700,
      countDown3 : 1800,
      seconds:null,
      minutes:null,
      on1 : true,
      on2 : true,
      on3 : true,
      number : 1,
      isActive: false
    };
  },
  methods: {
    // activeout(){
    //   if((this.exambanks[0].selected == this.exambanks[0].choices[0]) || (this.exambanks[0].selected == this.exambanks[0].choices[1])){
    //     var el = this.$el.getElementsByClassName("answeritem")[0];
    //   el.style.color = "blue";
    //   }
    // },
    saveResult() {
      var data = {
        marks: this.result.marks,
        username: this.result.username,
        subject: this.result.subject,
      };
      ResultDataService.create(data)
        .then(response => {
          this.result.id = response.data.id;
          console.log(response.data);
          // this.submitted = true;
        })
        .catch(e => {
          console.log(e);
        });
    },
    retrieveQuestion() {
      ExamBankDataService.getAll()
        .then(response => {
          this.exambanks = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },
    // setActiveQuestion(exambank, index) {
    //   this.currentQuestion = exambank;
    //   this.currentIndex = index;
    // },
     marks() {
      var correct = this.exambanks.filter(exambank => exambank.selected === exambank.answer);
      this.result.marks = correct.length * 10;
      this.result.username = this.currentUser.username;
      this.saveResult();
      return correct.length * 10;
    },
    submit(){
      this.turn_on = true ;
      
    },
    countDownTimer1() {  
                this.on2 = false;
                this.on3 = false;
                if(this.countDown1 > 0) {
                    setTimeout(() => {
                        this.countDown1 -= 1
                        this.minutes = Math.floor(this.countDown1 / 60)
                        this.seconds = this.countDown1 % 60
                        if(this.seconds < 10){
                          this.seconds = "0"+this.seconds
                        }
                        this.countDownTimer1()
                    }, 1000)
                }
    },
    countDownTimer2() {
                this.on1 = false;
                this.on3 = false;
                if(this.countDown2 > 0) {
                    setTimeout(() => {
                        this.countDown2 -= 1
                        this.minutes = Math.floor(this.countDown2 / 60)
                        this.seconds = this.countDown2 % 60
                        if(this.seconds < 10){
                          this.seconds = "0"+this.seconds
                        }
                        this.countDownTimer2()
                    }, 1000)
                }
    },
    countDownTimer3() {
                this.on2 = false;
                this.on1 = false;
                if(this.countDown3 > 0) {
                    setTimeout(() => {
                        this.countDown3 -= 1
                        this.minutes = Math.floor(this.countDown3 / 60)
                        this.seconds = this.countDown3 % 60
                        if(this.seconds < 10){
                          this.seconds = "0"+this.seconds
                        }
                        this.countDownTimer3()
                    }, 1000)
                }
    },
    plus(){
      this.number = this.number + 1
    },
    scroll1(){
      var el = this.$el.getElementsByClassName("number_question")[0];
      el.scrollIntoView();
      
    },
    scroll2(){
      var el = this.$el.getElementsByClassName("number_question")[1];
      el.scrollIntoView();
    },
    scroll3(){
      var el = this.$el.getElementsByClassName("number_question")[2];
      el.scrollIntoView();
    },
    scroll4(){
      var el = this.$el.getElementsByClassName("number_question")[3];
      el.scrollIntoView();
    },
    scroll5(){
      var el = this.$el.getElementsByClassName("number_question")[4];
      el.scrollIntoView();
    },
    scroll6(){
      var el = this.$el.getElementsByClassName("number_question")[5];
      el.scrollIntoView();
    },
    scroll7(){
      var el = this.$el.getElementsByClassName("number_question")[6];
      el.scrollIntoView();
    },
    scroll8(){
      var el = this.$el.getElementsByClassName("number_question")[7];
      el.scrollIntoView();
    },
    scroll9(){
      var el = this.$el.getElementsByClassName("number_question")[8];
      el.scrollIntoView();
    },
    scroll10(){
      var el = this.$el.getElementsByClassName("number_question")[9];
      el.scrollIntoView();
    },
  },
  computed: {
    currentUser() {
      return this.$store.state.auth.user;
    },
  },
  created() {
           this.plus();
  },
  mounted() {
    // this.retrieveQuestion();
  }
};
</script>

<style>
.space{
  margin-left: 1px;
}
.rule_container{
  margin: 15px 10px 10px 20px;
}
.list {
  text-align: left;
  max-width: 750px;
  margin: auto;
}
.test{
  width: 1200px;
  padding: 0 20px;
}
.list_row{
  width: 65%;
}
.col-md-6{
  max-width: 90%;
}
.right{
    width: 300px;
    margin: 0;
    height: 100vh;
    overflow-y: visible;
    position: relative;
    box-sizing: border-box;
    position: fixed;
    top: 30px;
    left: 68%;
}
.rightcontainer{
    width: 100%;
    height: 100%;
    margin: 10px 0;
    border-radius: 4px;
    padding-bottom: 10px;
    overflow: hidden;
    overflow-x: hidden;
    overflow-y: visible;
    position: relative;
}
.timepost{
    width: calc(100% - 40px);
    margin: 20px 20px 10px;
    font-family: Roboto,sans-serif;
    font-size: 20px;
    font-weight: 900;
    color: #444;
}
.timetext{
    width: calc(100% - 40px);
    margin: 20px 20px 10px;
    font-family: Roboto,sans-serif;
    font-size: 20px;
    font-weight: 900;
    color: #444;
    box-sizing: border-box;
}
.checkanswer{
    width: calc(100% - 50px);
    margin: 15px 10px 10px 20px;
    box-sizing: border-box;
    display: flex;
    flex-wrap: wrap;
}
.answeritem{
    width: 30px;
    height: 30px;
    border-radius: 4px;
    margin: 0 2.8px 2.8px 0;
    background-color: #f5f5f5;
    line-height: 30px;
    text-align: center;
    font-family: Roboto,sans-serif;
    font-weight: 400;
    font-size: 14px;
    color: #444;
    cursor: pointer;
    user-select: none;
}
.submit{
    width: calc(100% - 40px);
    margin: 10px 20px;
}
.btn{
    font-size: 14px;
    font-weight: 700;
    font-family: Roboto,sans-serif;
    color: #fff;
    background: #2392ec;
    padding: 1rem 0;
    width: 100%;
    letter-spacing: .02rem;
    border-radius: 4px;
    border: 4px #2392ec;
    transition: background .15s ease-in-out,transform .1s ease,-webkit-transform .1s ease;
    cursor: pointer;
    position: relative;
}
.btn:hover{
    background: #1a83d8;
}

.active, .active2, .active3, .active4, .active5, .active6, .active7, .active8, .active9, .active10, .active11, .active12, .active13, .active14, .active15, .active16, .active17, .active18, .active19, .active20, .active21, .active22, .active23, .active24, .active25, .active26, .active27, .active28, .active29, .active30 {
    background: #d9edfe;
    color: #2392ec;
}
.timechoice{
    width: calc(100% - 40px);
    margin: 20px 20px 10px;
    font-family: Roboto,sans-serif;
    font-size: 20px;
    font-weight: 900;
    color: #444;
}
.choice{
    margin: 20px 20px 10px;
    align-items: center;
    justify-content: center;
}
.choice div{
    padding-right: 10px;
    
}
.choice button{
    background: #2392ec;
    color: #fff;
    border: 4px #2392ec;
    border-radius: .25rem;
    cursor: pointer;
}
.clock{
    margin: 20px 20px 10px;
}
.clock div{
    width: 220.5px;
    height: 72.5px;
    text-align: center;
    line-height: 70px;
    border-radius: 4px;
    color: #2392ec;
    background-color: #d9edfe;
    font-family: Roboto,sans-serif;
    font-weight: 900;
    font-size: 35px;
    margin: 0;
}
.list-group-item{
  border: none;
  width: 100%;
  padding: 0 0 20px;
  margin: 10px 0 0;
  background: #fff;
  border-bottom: 2px solid #f0f0f0;
}
.number_question{
  width: 100%;
  line-height: 25px;
  font-family: Noto Serif JP,serif;
  font-weight: 700;
  font-size: 15px;
  color: #2392ec;
  margin: 15px 0 0;
}
.question_content{
  width: 100%;
  padding-top: 10px;
  padding-bottom: 10px;
  font-family: Noto Serif JP,serif;
  font-weight: 400;
  font-size: 15px;
  line-height: 1.5;
  color: #444;
}
.label{
  font-family: Noto Serif JP,serif;
  font-weight: 700;
  color: #2392ec;
  width: 20px;
  margin: 0 0 0 10px;
  text-align: left;
  padding-right: 5px;
}
.result-wrap{
  font-weight: 700;
  margin-left: 50px;
}
</style>
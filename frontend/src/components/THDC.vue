<template>
   <div class="test" style="display:flex">
     <div class="list_row" style="margin-left:">
       <div class="col-md-6">
        <h4>Trắc nhiệm Tin học đại cương</h4>
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
                              <span v-if="turn_on" class="space">C</span>
                            </div>
                            
                            <div class="answeritem" id="03" @click="scroll3" >
                              <span>3</span>
                              <span v-if="turn_on" class="space">D</span>
                            </div>
                            
                            <div class="answeritem" id="04" @click="scroll4" >
                              <span>4</span>
                              <span v-if="turn_on" class="space">D</span>
                            </div>
                            
                            <div class="answeritem" id="05" @click="scroll5" >
                              <span>5</span>
                              <span v-if="turn_on" class="space">D</span>
                            </div>
                            
                            <div class="answeritem" id="06" @click="scroll6" >
                              <span>6</span>
                              <span v-if="turn_on" class="space">A</span>
                            </div>
                            
                            <div class="answeritem" id="07" @click="scroll7" >
                              <span>7</span>
                              <span v-if="turn_on" class="space">B</span>
                            </div>
                            
                            <div class="answeritem" id="08" @click="scroll8" >
                              <span>8</span>
                              <span v-if="turn_on" class="space">A</span>
                            </div>
                            
                            <div class="answeritem" id="09" @click="scroll9" >
                              <span>9</span>
                              <span v-if="turn_on" class="space">A</span>
                            </div>
                            
                            <div class="answeritem" id="010" @click="scroll10" >
                              <span>10</span>
                              <span v-if="turn_on" class="space">C</span>
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
        title: "Bộ nhớ RAM và ROM là bộ nhớ gì?",
        choices: [
            "Secondary memory",
            "Receive memory",
            "Primary memory",
            "Random access memory"
        ],
        answer: "Primary memory",
        selected: null
       },
       {
        title: "Phát biểu nào sau đây là sai",
        choices: [
            " Đơn vị điều khiển (Control Unit) chứa CPU, điều khiển tất cả các hoạt động của máy",
            "CPU là bộ nhớ xử lý trung ương, thực hiện việc xử lý thông tin lưu trữ trong bộ nhớ",
            "ALU là đơn vị số học và luận lý và các thanh ghi cũ ng nằm trong CPU",
            "Memory Cell là tập hợp các ô nhớ"
        ],
        answer: "ALU là đơn vị số học và luận lý và các thanh ghi cũ ng nằm trong CPU",
        selected: null,
       },
       {
        title: "Dữ liệu là gì?",
        choices: [
            "Là các số liệu hoặc là tà i liệu cho trước chưa được xử lý",
            "Là khái niệm có thể được phát sinh, lưu trữ , tìm kiếm, sao chép, biến đổi…",
            "Là các thông tin được thể hiện dưới nhiều dạng khác nhau",
            "Tất cả đều đúng"
        ],
        answer: "Tất cả đều đúng",
        selected: null,
       },
       {
        title: "Bit là gì?",
        choices: [
            "Là đơn vị nhỏ nhất của thông tin được sử dụng trong máy tính",
            "Là một phần tử nhỏ mang một trong 2 giá trị 0 và 1",
            "Là một đơn vị đo thông tin",
            "Tất cả đều đúng"
        ],
        answer: "Tất cả đều đúng",
        selected: null,
       },
       {
        title: "Hex là hệ đếm",
        choices: [
            "Hệ nhị phân",
            "Hệ bát phân",
            "Hệ thập phân",
            "Hệ thập lục phân"
        ],
        answer: "Hệ thập lục phân",
        selected: null,
       },
       {
        title: "Các thà nh phần: bộ nhớ chính, bộ xử lý trung ương, bộ phận nhập xuất, các loại hệ điều hà nh là",
        choices: [
            "Phần cứng",
            "Phần mềm",
            "Thiết bị lưu trữ",
            "Tất cả đều sai"
        ],
        answer: "Phần cứng",
        selected: null,
       },
       {
        title: "Hệ thống các chương trình đảm nhận chức năng là m môi trườ ng trung gian giữa người sử dụng và phần cứng của máy tính được gọi là",
        choices: [
            "Phần mềm",
            "hệ điều hành",
            "Các loại trình dịch trung gian",
            "Tất cả đều đúng"
        ],
        answer: "hệ điều hành",
        selected: null,
       },
       {
        title: "Các loại hệ điều hà nh Windows đều có khả năng tự động nhận biết các thiết bị phần cứng và tự động cà i đặt cấu hình của các thiết bị dây là chức năng",
        choices: [
            "Plug and Play",
            "Windows Explorer",
            "Desktop",
            "Multimedia"
        ],
        answer: "Plug and Play",
        selected: null,
       },
       {
        title: "Danh sách các mục chọn trong thực đơn gọi là",
        choices: [
            "Menu bar ",
            "Menu options",
            "Menu pad",
            "Tất cả đều sai"
        ],
        answer: "Menu bar ",
        selected: null,
       },
       {
        title: "Windows Explorer có các thành phần: Explorer bar, Explorer view, Tool bar, menu bar. Còn lại là gì?",
        choices: [
            "Status bar",
            "Menu bar",
            "Task bar",
            "tất cả đều sai"
        ],
        answer: "Status bar",
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
        subject:"Tin đại cương",
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
        subject: this.result.subject
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
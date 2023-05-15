<template>
  <div id="app">
    <Header :numCorrect="index" :numTotal="numTotal" />

    <b-container class="bv-example-row">
      <b-row>
        <b-col v-if="!finished" sm="6" offset="3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
        <b-col v-else class="px-5 my-4">
          <h2>Професии кои ви ги препорачуваме</h2>
          <dl>
            <div
              v-for="(job, i) in jobsOrder"
              :key="i"
              class="bg-light shadow mb-4 p-2"
            >
              <dt>{{ i + 1 }}. {{ job.text }}</dt>
              <dd>{{ job.description }}</dd>
            </div>
          </dl>
          <b-btn variant="primary" href="/"> Пробај повторно</b-btn>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "app",
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      answers: [],
      finished: false,
      jobs: [
        {
          name: "frontend",
          text: "Frontend програмер",
          description:
            "Frontend програмер е професија која побараува познавања од HTML, CSS, Javascript, GitHub, PWA, итн. Оваа професија во главно се занимава со визуелни елементи и кориснички интерфејс. За оваа професија не е потребно високо образование, но се смета за предност.",
        },
        {
          name: "backend",
          text: "Backend програмер",
          description:
            "Backend програмер е професија која побараува познавања од Основи на Frontend, OS, OOP, SQL & NoSQL, APIs, CI/CD, Web Servers, оваа работа во главно се занимава со бизнис логика на апликациите, и поврзување со базите на податоци. За оваа позиција е пожелно да се има високо образование, но не е задолжително.",
        },
        {
          name: "DB",
          text: "Администратор на бази на податоци",
          description:
            "Администратор на бази на податоци  е професија која побараува познавања од Структури на податоци, SQL, NoSQL, оваа работа во главно се занимава со креирање, одржување и администраци на бази на податоци. За оваа позиција е пожелно да се има најмалку средно образовани, но високо образование се смета за предност.",
        },
        {
          name: "Data Science",
          text: "Data Science",
          description:
            "Data Science е професија која побараува познавања од Обработка, аналитика и визуелизација на податоци, оваа работа во главно се занимава со oбработка, аналитика и визуелизација на податоци. За оваа позиција е пожелно да се има најмалку високо образование.",
        },
      ],
    };
  },
  methods: {
    next() {
      this.index++;
    },
    increment(answer) {
      this.answers.push(answer);
      if (this.index < this.questions.length - 1) this.next();
      else {
        this.finished = true;
      }
      // if (isCorrect) {
      //   this.numCorrect++;
      // }
      // this.numTotal++;
    },
  },
  computed: {
    jobsOrder() {
      let jobs = ["frontend", "backend", "DB", "Data Science"];
      let sorted = [];
      jobs.forEach((job) => {
        sorted.push({
          position: job,
          times: this.answers.filter((el) => el.includes(job)).length,
        });
      });
      sorted.sort((a, b) => {
        let comparison = 0;
        let countA = a.times;
        let countB = b.times;
        if (countA > countB) {
          comparison = -1;
        } else if (countA < countB) {
          comparison = 1;
        }
        return comparison;
      });
      let arr = [];
      sorted.forEach((el) => {
        arr.push(this.jobs.find((job) => job.name == el.position));
      });
      return arr;
    },
  },
  mounted: function() {
    let questions = [
      {
        question: "Вашето образование?",
        answers: [
          { text: "Средно образование", jobs: ["frontend", "backend", "DB"] },
          {
            text: "Високо - информатика",
            jobs: ["frontend", "backend", "DB", "Data Science"],
          },
          { text: "Високо - математика", jobs: ["DB", "Data Science"] },
          { text: "Високо - останато", jobs: ["frontend", "backend", "DB"] },
        ],
      },
      {
        question: "Вештини кои ги поседувате?",
        answers: [
          { text: "HTML, CSS, Javascript, GitHub, PWA", jobs: ["frontend"] },
          {
            text:
              "Основи на Frontend, OS, OOP, SQL & NoSQL, APIs, CI/CD, Web Servers",
            jobs: ["frontend", "backend"],
          },
          { text: "Структури на податоци, SQL, NoSQL", jobs: ["DB"] },
          {
            text:
              "Веројатност и статистика, Нумерика, Аналитика, Бази на податоци, Python/R/SAS, ML, Визуелизација на податоци",
            jobs: ["Data Science"],
          },
        ],
      },
      {
        question: "Ваши интереси?",
        answers: [
          {
            text:
              "Креирање на веб дизајн и изглед на веб страни и кориснички интерфејси",
            jobs: ["frontend"],
          },
          {
            text:
              "Бизнис логика на апликација, дизајнирање на бази на податоци и интеграција на апликации",
            jobs: ["backend"],
          },
          {
            text: "Администрација и одржување на бази на податоци",
            jobs: ["DB"],
          },
          {
            text: "Обработка, аналитика и визуелизација на податоци",
            jobs: ["Data Science"],
          },
        ],
      },
      {
        question: "Каков тип на работа?",
        answers: [
          {
            text: "Работа со повеќе визуелни елементи",
            jobs: ["frontend", "DB"],
          },
          {
            text: "Работа која бара логичко размислување",
            jobs: ["backend", "Data Science"],
          },
          { text: "Аналитика, математички пресметки", jobs: ["Data Science"] },
          { text: "Креирање на шеми и организирање на податоци", jobs: ["DB"] },
        ],
      },
    ];
    this.questions = questions;
    this.numTotal = questions.length;
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

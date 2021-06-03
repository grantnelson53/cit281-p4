## Project 4

In this project, we used our knowledge of fastify, arrays, and code modules to sort a list of questions and answers

### Source Code 


    // Question and answer data array
    const data = [
      {
        question: "Q1",
        answer: "A1",
      },
      {
        question: "Q2",
        answer: "A2",
      },
      {
        question: "Q3",
        answer: "A3",
      },
    ];

    // Export statement must be below data declaration - no hoisting with const
    module.exports = {
      data,
    };



    const { data } = require("./p4-data");

    function getQuestions() {
      let questions = [];
      for (let i = 0; i < data.length; i++) {
        questions.push(data[i].question);
      }
      return questions;
    }

    function getAnswers() {
      let answers = [];
      for (let i = 0; i < data.length; i++) {
        answers.push(data[i].answer);
      }
      return answers;
    }

    function getQuestionsAnswers() {
      const dataClone = Object.assign(data);
      return dataClone;
    }

    function getQuestion(number = "") {
      let questionObj = { question: "", number: "", error: "" };

      questionObj.question = data[number - 1].question;
      questionObj.number = number;

      if (number > data.length) {
        questionObj.error = "Error";
      }

      if (number < 1) {
        questionObj.error = "Error";
      }

      return questionObj;
    }
    function getAnswer(number = "") {
      let answerObj = { answer: "", number: "", error: "" };

      answerObj.answer = data[number - 1].answer;
      answerObj.number = number;

      if (number > data.length) {
        answerObj.error = "Error";
      }

      if (number < 1) {
        answerObj.error = "Error";
      }

      return answerObj;
    }

    function getQuestionAnswer(number = "") {
      let questionAnswerObj = { question: "", answer: "", number: "", error: "" };

      questionAnswerObj.question = data[number - 1].question;
      questionAnswerObj.answer = data[number - 1].answer;
      questionAnswerObj.number = number;

      if (number > data.length) {
        questionAnswerObj.error = "Error";
      }

      if (number < 1) {
        questionAnswerObj.error = "Error";
      }

      return questionAnswerObj;
    }

    /*****************************
      Module function testing
    ******************************/
    function testing(category, ...args) {
      console.log(`\n** Testing ${category} **`);
      console.log("-------------------------------");
      for (const o of args) {
        console.log(`-> ${category}${o.d}:`);
        console.log(o.f);
      }
    }

    // Set a constant to true to test the appropriate function
    const testGetQs = false;
    const testGetAs = false;
    const testGetQsAs = false;
    const testGetQ = false;
    const testGetA = false;
    const testGetQA = false;
    const testAdd = false; // Extra credit
    const testUpdate = false; // Extra credit
    const testDelete = false; // Extra credit

    // getQuestions()
    if (testGetQs) {
      testing("getQuestions", { d: "()", f: getQuestions() });
    }

    // getAnswers()
    if (testGetAs) {
      testing("getAnswers", { d: "()", f: getAnswers() });
    }

    // getQuestionsAnswers()
    if (testGetQsAs) {
      testing("getQuestionsAnswers", { d: "()", f: getQuestionsAnswers() });
    }

    // getQuestion()
    if (testGetQ) {
      testing(
        "getQuestion",
        { d: "()", f: getQuestion() }, // Extra credit: +1
        { d: "(0)", f: getQuestion(0) }, // Extra credit: +1
        { d: "(1)", f: getQuestion(1) },
        { d: "(4)", f: getQuestion(4) } // Extra credit: +1
      );
    }

    // getAnswer()
    if (testGetA) {
      testing(
        "getAnswer",
        { d: "()", f: getAnswer() }, // Extra credit: +1
        { d: "(0)", f: getAnswer(0) }, // Extra credit: +1
        { d: "(1)", f: getAnswer(1) },
        { d: "(4)", f: getAnswer(4) } // Extra credit: +1
      );
    }

    // getQuestionAnswer()
    if (testGetQA) {
      testing(
        "getQuestionAnswer",
        { d: "()", f: getQuestionAnswer() }, // Extra credit: +1
        { d: "(0)", f: getQuestionAnswer(0) }, // Extra credit: +1
        { d: "(1)", f: getQuestionAnswer(1) },
        { d: "(4)", f: getQuestionAnswer(4) } // Extra credit: +1
      );
    }

    module.exports = {
        getQuestions,
        getAnswers,
        getQuestionsAnswers,
        getQuestion,
        getAnswer,
        getQuestionAnswer
    };


    // Require the Fastify framework and instantiate it
    const fastify = require("fastify")();

    const { 
        getQuestions,
        getAnswers,
        getQuestionsAnswers,
        getQuestion,
        getAnswer,
        getQuestionAnswer } = require("./p4-module-v2.js.js");


    fastify.get("/cit/question", (request, reply) => {
      reply.code(200).header("Content-Type", "application/json") .send({
        error: "",
        statusCode: 200,
        questions: getQuestions(),
        });
    });

    fastify.get("/cit/answer", (request, reply) => {
      reply.code(200).header("Content-Type", "application/json").send({
        error: "",
        statusCode: 200,
        answers: getAnswers(),
      });
    });

    fastify.get("/cit/questionanswer", (request, reply) => {
      reply.code(200).header("Content-Type", "application/json").send({
        error: "",
        statusCode: 200,
        questionsAnswers: getQuestionsAnswers(),
      });
    });

    fastify.get("/cit/question/:number", (request, reply) => {
        const numberReq = request.params.number;
        const questionObj = getQuestion(parseInt(numberReq));
        const {question, number, error} =questionObj;
      reply.code(200).header("Content-Type", "application/json").send({
        error: "",
        statusCode: 200,
        question: question,
        number: number,
      });
    });

    fastify.get("/cit/answer/:number", (request, reply) => {
      const numberReq = request.params.number;
      const answerObj = getAnswer(parseInt(numberReq));
      const { answer, number, error } = answerObj;
      reply.code(200).header("Content-Type", "application/json").send({
        error: "",
        statusCode: 200,
        answer: answer,
        number: number,
      });
    });

    fastify.get("/cit/questionanswer/:number", (request, reply) => {
      const numberReq = request.params.number;
      const questionAnswerObj = getQuestionAnswer(parseInt(numberReq));
      const { question, answer, number, error } = questionAnswerObj;
      reply.code(200).header("Content-Type", "application/json").send({
        error: "",
        statusCode: 200,
        question: question,
        answer: answer,
        number: number,
      });
    });

    fastify.get("*", (request, reply) => {
      reply.code(200).header("Content-Type", "application/json").send({
        error: "Route Not Found",
        statusCode: 404,
      });
    });
    // Start server and listen to requests using Fastify
    const listenIP = "localhost";
    const listenPort = 8080;
    fastify.listen(listenPort, listenIP, (err, address) => {
      if (err) {
        console.log(err);
        process.exit(1);
      }
      console.log(`Server listening on ${address}`);
    });

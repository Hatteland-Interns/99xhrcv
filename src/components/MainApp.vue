<template>
  <v-row no-gutters justify="space-evenly">

    <v-col cols="8" md="5" class="mt-4">
      <v-card elevation="3" class="pa-2">
        <v-file-input v-model="file" @update:model-value="sendPdf" accept=".pdf" variant="outlined"
          prepend-icon="mdi-file-pdf-box" counter show-size truncate-length="50"
          label="Upload your CV here"></v-file-input></v-card>
    </v-col>

    <v-col cols="8" md="3" class="mt-4">
      <v-card variant="outlined">
        <v-list-item three-line>
          <v-list-item-content>
            <div class="text-overline mb-4">
              {{ universities[0]?.University_Name != null ? universities[0].University_Name.toUpperCase() : "University not found" }} : {{
                universities[0]?.Duration != null ? universities[0].Duration.toUpperCase() : "Duration not found" }}
            </div>
            <v-list-item-title class="text-h5 mb-1">
              {{ name != null ? name.toUpperCase() : "Name not found" }}
            </v-list-item-title>
            <v-list-item-subtitle>{{ age != null ? "Age : " + age : "Age not found" }}</v-list-item-subtitle>
          </v-list-item-content>

          <v-list-item-avatar tile size="80" color="grey"></v-list-item-avatar>
        </v-list-item>
      </v-card>
    </v-col>
  </v-row>


  <v-progress-linear v-if="loading" class="mt-10" indeterminate color="yellow-darken-2"></v-progress-linear>

  <v-row justify="center">
    <v-col cols="12" sm="10">
      <v-table>
        <thead>
          <tr>
            <th class="text-left">
              Programming Language
            </th>
            <th class="text-left">
              Rating
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="programmingLanguage in programmingLanguages" :key="programmingLanguage.Programming_Language_Name">
            <td v-if="programmingLanguage.Programming_Language_Name != null">{{
              programmingLanguage.Programming_Language_Name }}</td>
            <td v-if="programmingLanguage.Programming_Language_Name != null">{{
              programmingLanguage.Programming_Language_Rate }}</td>
          </tr>
        </tbody>
      </v-table>
    </v-col>
  </v-row>
  <hr>
  <v-row justify="center">
    <v-col cols="12" sm="10">
      <v-table>
        <thead>
          <tr>
            <th class="text-left">
              Project Name
            </th>
            <th class="text-left">
              Used Technologies
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="project in projects" :key="project.Project_Name">
            <td v-if="project.Project_Name != null">
              {{ project.Project_Name }}
            </td>
            <td v-if="project.Project_Name != null">
              <ul>
                <li v-for="technology in project.Used_Technologies" :key="technology">{{ technology }}</li>
              </ul>
            </td>
          </tr>
        </tbody>
      </v-table>
    </v-col>
  </v-row>
  <hr>
  <v-row justify="center">
    <v-col cols="12" sm="10">
      <v-table>
        <thead>
          <tr>
            <th class="text-left">
              COMPANY
            </th>
            <th class="text-left">
              POSITION
            </th>
            <th class="text-left">
              DURATION
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="jobExperience in jobExperiences" :key="jobExperience.Company">
            <td v-if="jobExperience.Company != null">{{ jobExperience.Company }}</td>
            <td v-if="jobExperience.Company != null">{{ jobExperience.Position }}</td>
            <td v-if="jobExperience.Company != null">{{ jobExperience.Duration }}</td>

          </tr>
        </tbody>
      </v-table>
    </v-col>
  </v-row>
</template>

<script>

import axios from 'axios';
import ConvertApi from 'convertapi-js'
import { Configuration, OpenAIApi } from "openai";

export default {
  data() {
    return {
      loading: false,
      name: null,
      age: null,
      jobExperiences: [],
      universities: [],
      programmingLanguages: [],
      projects: [],
      file: null
    }
  },
  methods: {
    async sendPdf() {

      this.loading = true

      let A = "sk-7ggKOA"
      let P = "aHuCbu5t"
      let I = "6Pkuh6T"
      let K = "3BlbkFJkj"
      let E = "Gv7GcvuoK"
      let Y = "cfb1T0B4w"

      let convertApi = ConvertApi.auth('tWwpgnscsp3mWzgn')
      let params = convertApi.createParams()
      params.add('File', this.file[0]);
      params.add('IncludeFormatting', 'true');
      let result = await convertApi.convert('pdf', 'txt', params)

      axios.get(result.files[0].Url)
        .then(async (response) => {
          console.log(response.data);

      const configuration = new Configuration({
        apiKey: A + P + I + K + E + Y
      });

      const openai = new OpenAIApi(configuration);

      // const data = "X's Curriculum vitae \n\nCONTACT         \r\n\r\n077-9391860/071-7022495\r\n\r\nyasiru.19@cse.mrt.ac.lk\r\n\r\nNo.270, Wiharamawatha,\r\nWeliamuna, Walasmulla\r\n\r\n/yasiru-lakshan19\r\n\r\n/yasirulakshan.19\r\n\r\n/yasirulakshan\r\n\r\nTECHNICAL     SKILLS     \r\n\r\nYASIRU LAKSHAN\r\n\r\nWICKRAMASINGHA\r\n\r\nPassionate Computer Science& Engineeringundergraduate\r\nwith problem solving, web developing and software\r\nengineering skills. Hope to work with a good organization\r\nto  gain experience in these fields and help the betterment\r\nof the company.\r\n\r\nEDUCATION               \r\n\r\n• BSc Engineering Hons (2020/01/03 - Current)\r\n\r\nGPA - 3.62(Current)\r\n\r\nDepartment - Computer Science & Engineering\r\nFaculty - Faculty of Engineering\r\n\r\nUniversity of Moratuwa\r\n\r\n•  GCE Advanced Level (2018)\r\nWeerakeriya Rajapaksha Central College\r\nPhysical Science Stream\r\n\r\nCombined Maths : A Physics: A  Chemestry: B\r\n\r\nPassed with Merit pass : Distric Rank - 27 Island Rank - 616\r\n\r\n• GCE Ordinary Level (2014)\r\n\r\n\r\nUI/UX\r\n\r\nAlgorithms\r\nFrontend\r\n\r\nOOP Concepts\r\nDatabase\r\nBackend\r\n\r\nWeerakeriya Rajapaksha Central College\r\n\r\nWith 7 ‘A’ Passes including Maths, Science, English and 1 ‘B’\r\npass for Sinhala and ‘S’ pass for English Literature\r\n\r\nPROJECTS                \r\n\r\n\r\nJava\r\n\r\nPython\r\n\r\nReact\r\n\r\n• Fuel Queue Management System (Ongoing)\r\n\r\n\r\nJavaScript\r\n\r\nNodejs\r\n\r\nWeb based system for managing fuel queues and\r\nregulating fuel distribution. Mobile app and Web app are\r\n\r\n\r\nHTML\r\n\r\nCSS\r\n\r\nMySQL\r\n\r\nbeing developed for this project.\r\n\r\n\r\nGraphic Design\r\n\r\nSOCIAL      SKILLS       \r\n\r\nReact, Nodejs, MongoDB\r\n\r\n• E-Mart (Online shopping platform) (Jul 2022)\r\n\r\nAn E-commerce system which a company or seller can\r\n\r\n\r\nProblem Solving  Planning\r\n\r\nsell their products online. Focused on web ui and backend.\r\n\r\nReact, Expressjs, PostgreSQL, MUI\r\n\r\n\r\nLeadership  Adaptability\r\nCollaborative  Negotiation\r\n\r\n• Donateme - Online Donation Application (Jan 2022)\r\nA web application where users can request\r\ndonations and users can donate for requests.\r\n\r\n\r\nHOBBIES         \r\n\r\nI focused on frontend most in this project.\r\n\r\nPHP, Bootstrap, MySQL\r\n\r\n\r\nVolleyball   Photography\r\n\r\n• Student Management System (Jan 2021)\r\n\r\n\r\nGraphic Design   Carrom\r\n\r\nThis is for managing student request with university. In\r\nthis system I focused on gui design and frontend mostly.\r\n\r\nReact, Expressjs\r\n\r\n\r\nCERTIFICATES                       EXTRA ACTIVITIES\r\n\r\n\r\n• Introduction to UI Design\r\n\r\n2020/11/17\r\n\r\nUniversity of Minnesota\r\n\r\nVerify at - coursera.org/verify/A23AJPSL7YXS\r\n\r\n•  Database Creation and Modeling using MYSQL\r\nWorkbench\r\n\r\n2021/02/24\r\n\r\nCoursera Project Network\r\n\r\nVerify at - coursera.org/verify/2YSNKUD LC3XM\r\n\r\n• HTML, CSS, and Javascript for Web Developers\r\n\r\n2021/08/12\r\n\r\nJohns Hopkins University\r\n\r\nVerify at - coursera.org/verify/MYS8N8Q3VRF9\r\n\r\nWORK EXPERIENCE\r\n\r\n• Trainee Staft Member - Peoples Bank\r\n\r\n2019/06/12- 2019/12/20\r\n\r\n6 month training program offered by Peoples Bank\r\n\r\n• Freelancer\r\n\r\n2020/06 - Now\r\n\r\nLevel 2 seller on FIVERR platform. Work as a graphic\r\ndesigner. Worked over 30 foriegn clients in more than 100\r\nprojects.\r\n\r\nACHIEVEMENTS\r\n\r\n• 3rd Place in All Island Mathematics Competition\r\n\r\n2011/07/11\r\n\r\n• Achieve leading places countinously in Provincial\r\nMathematics Competitions\r\n\r\n2009/2010/2011/2012/2013\r\n\r\nREFEREES\r\n\r\n• Radapasa Media Unit\r\n\r\n2012 - 2017\r\n\r\nWorked as Photographer,\r\nDesigner and Event orgnizer in\r\nSchool Media Unit\r\n\r\n• MoraSpirit\r\n\r\n2020/03/14 - 2022/01/05\r\n\r\nCreative Design Pillar\r\n\r\n• Rotract Club\r\n2020/08/12 - 2022/01/01\r\nCreative Design Pillar\r\n\r\n•  IESL Student Chapter\r\n2021/03/22 - 2022/01/01\r\nDepartment Coordinator\r\nof Computer Scince and\r\nEngineering 19 Batch\r\n\r\n•  IEEE CS Chapter\r\n2020/12 - 2022/01/01\r\nDesign Pillar Member\r\n\r\n• IEEE Young Professionals\r\n2021/03 - 2022/01/01\r\nPublicity Team Member\r\n\r\n• School Volleyball\r\n\r\nSince 2010 School Volleyball\r\nteam member of Weeraketiya\r\nRajapaksha Central College.\r\n\r\n•  Won 2010/ 2011/ 2012/ 2013/\r\n\r\n2014/ 2015 Zonal level\r\n\r\n• Achieved 3rd Place in 2013\r\nProvincial Volleyball Matches\r\n\r\n• University Volleyball\r\n\r\n•  Freshers meet Champions and\r\nCaptain of the Team.\r\n\r\n• University Volleyball team\r\nmember.\r\n\r\n\r\n•  Dr. Gayashan Amarasinghe (Senior Lecturer)\r\nPhD (Melb), BSc. Eng (Hons) (Moratuwa),\r\nAMIE(SL))\r\n\r\n071 831 4517\r\n\r\nDept. of Computer Science and\r\nEngineering\r\n\r\nUniversity of Moratuwa.\r\n\r\n• Kaumudi Guntilaka\r\n\r\n(Senior Software Engineer & Team )\r\nLead-RPA)\r\n\r\n077 193 3407\r\n\r\nInnovotechnology\r\nSolutions -Austrailia\r\n\r\n\n\n"

      let promptText = "X's Curriculum vitae \n\n" + response.data + "\n\n" +
        "Q: Convert that X's Curriculum vitae to json format? Json should includes these feilds only. Don't add additional fields. If you can't find a value for a field, replace it with null keyword ?" + "\n\n" +
        "  * Name (Single String value)\n  * Age (Single String value)\n  * Job_Experiences\n    ^ Company (Single String value)\n    ^ Position (Single String value)\n    ^ Duration (Single String value)\n  * Universities\n    ^ University_Name (Single String value)\n    ^ Duration (Single String value)\n  * Programming_Languages\n    ^ Programming_Language_Name (Single String value)\n    ^ Programming_Language_Rate (Rate each Programming_Language from 0 to 10 your creativity. This should be single integer a value between 1 to 10)\n  * Projects ( Projects Done by X )\n    ^ Project_Name (This should be a Sring value)    ^ Used Tachnologies (Used Tachnologies for each Project. This should be inside 'Projects' object. Used Tachnologies This should be an String array)\n\nA: "

      console.log(promptText)

      const outJson = await openai.createCompletion({
        model: "text-davinci-003",
        prompt: promptText,
        temperature: 0.7,
        max_tokens: 2000,
        top_p: 1,
        frequency_penalty: 0,
        presence_penalty: 0,
      });

      let jsonRes = JSON.parse(outJson.data.choices[0].text)
      console.log(jsonRes)

      this.name = jsonRes.Name
      this.age = jsonRes.Age
      this.jobExperiences = jsonRes.Job_Experiences != null ? jsonRes.Job_Experiences : []
      this.universities = jsonRes.Universities != null ? jsonRes.Universities : []
      this.programmingLanguages = jsonRes.Programming_Languages != null ? jsonRes.Programming_Languages : []
      this.projects = jsonRes.Projects != null ? jsonRes.Projects : []

      this.loading = false

      })
      .catch((error) => {
        console.error(error);
        this.loading = false
      });



    },
    mounted() {

    }
  }
}
</script>

inter

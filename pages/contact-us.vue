<template>
  <v-row>
    <v-col cols="12">
      <v-card max-width="500" class="my-4 mx-auto">
        <v-card-title>Please fill in your contact details </v-card-title>
        <v-divider></v-divider>
        <Survey :survey="contactUsModel" />
      </v-card>
    </v-col>
  </v-row>
</template>
<script>
import * as SurveyVue from 'survey-vue'
import 'innsoSurveyTheme'

import ContactUsFormJSON from './contact-us-form.json'

const Survey = SurveyVue.Survey
SurveyVue.StylesManager.Enabled = false
SurveyVue.StylesManager.applyTheme('modern')

export default {
  name: 'ContactUs',
  components: {
    Survey,
  },
  layout: 'innsoTheme',
  data() {
    return {
      reasons: [],
      contactBackData: {
        customerInfo: {
          emailId: '',
          firstName: '',
          lastName: '',
        },
        ticketInfo: {
          contactReason: '',
          issueDescription: '',
        },
      },
      contactUsModel: new SurveyVue.Model(ContactUsFormJSON),
    }
  },
  head() {
    return {
      title: 'Innso Client Service - Contact Us',
    }
  },
  methods: {
    getIndexOfAttr(value) {
      for (let i = 0; i < this.reasons.data.length; i++) {
        if (this.reasons.data[i].ticketTypologyId === value) {
          return i
        }
      }
    },
  },
  created() {
    const that = this
    this.contactUsModel.onComplete.add(async function (result) {
      // console.log('Result: ', result)
      console.log('that.reasons: ', that.reasons)
      const issueDesc =
        result.data.question5 && result.data.question5 !== ''
          ? result.data.question5
          : that.reasons.data[that.getIndexOfAttr(result.data.question1)]
              .ticketTypologyDescription
      const contactBack = await that.$axios.post(
        '/contactback',
        {
          customerInfo: {
            emailId: result.data.question2,
            firstName: result.data.question3,
            lastName: result.data.question4,
          },
          ticketInfo: {
            contactReason:
              that.reasons.data[that.getIndexOfAttr(result.data.question1)],
            issueDescription: issueDesc,
          },
        },
        {
          headers: {
            'Content-Type': 'application/json',
            'client-ref': 'client-webapp',
            'requested-datetime': Math.floor(new Date().getTime() / 1000),
          },
        }
      )

      console.log('Result data: ', contactBack)
    })
  },
  async mounted() {
    this.reasons = await this.$axios.get('/ticket/typology', {
      headers: {
        'Content-Type': 'appliction/json',
        'client-ref': 'client-webapp',
        'requested-datetime': Math.floor(new Date().getTime() / 1000),
      },
    })
    console.log('Response from jsonplaceholder', this.reasons)

    const choices = this.reasons.data.map((reason) => {
      return {
        text: reason.ticketTypologyName,
        value: reason.ticketTypologyId,
      }
    })

    this.contactUsModel.getQuestionByName('question1').choices = choices

    console.log(
      JSON.parse(
        JSON.stringify(this.contactUsModel.getQuestionByName('question1'))
      )
    )

    console.log('Final choices options:', choices)
  },
}
</script>
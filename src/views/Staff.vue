<template>
    <div id="app">
        <h2>Direct Exposure Questionnaire</h2>
        <h4>Complete this if you were exposed to someone who was exposed to a confirmed case of COVID-19</h4>
        <survey :survey="survey"></survey>
    </div>
</template>

<script>
    //In your VueJS App.vue or yourComponent.vue file add these lines to import
    import * as SurveyVue from 'survey-vue'
    import 'bootstrap/dist/css/bootstrap.css';
    var Survey = SurveyVue.Survey
    Survey.cssType = "bootstrap";

    const lessExposed = "<h4>Complete the following steps:</h4> <ol> <li>Continue duties</li> <li>No test is required</li> <li>Active monitoring for 14 days</li> </ol>"
    // eslint-disable-next-line no-unused-vars
    const moreExposed = "<h4>Complete the following steps:</h4> <ol> <li>Stop duties</li> <li>Test is required</li> <li>Isolation for 14 days</li> </ol>"

    export default {
        name: 'app',
        components: {
            Survey
        },
        data () {
            var json = {
                "showNavigationButtons": "none",
                clearInvisibleValues: "onHidden",
                questions: [
                    {   type: "dropdown",
                        name: "firstTest",
                        title: "What were the test results of the person you were exposed to tested?",
                        choices: [
                            {value: 1, text: "Test Positive"},
                            {value: 2, text: "Test Negative"}
                        ]
                    },
                    {   type: "dropdown",
                        name: "negativeSymptoms",
                        title: "Are you experiencing any symptoms?",
                        choices: [
                            {value: 1, text: "Yes, I'm experiencing symptoms"},
                            {value: 2, text: "Yes, I'm not experiencing symptoms"}
                        ],
                        visibleIf: "{firstTest}='2'"
                    },
                    {
                        type: "html",
                        name: "negNoSymptoms",
                        html: moreExposed,
                        visibleIf: "{negativeSymptoms}='1'"
                    },
                    {
                        type: "html",
                        name: "negNoSymptoms",
                        html: lessExposed,
                        visibleIf: "{negativeSymptoms}='2'"
                    },
                    {   type: "dropdown",
                        name: "exposureRisk",
                        title: "Was your exposure low risk?",
                        choices: [
                            {value: 1, text: "My exposure was low risk (e.g. contact < 20 mins, distance > 1.5m, wearing mask)"},
                            {value: 2, text: "My exposure was high risk (e.g. contact > 20 mins, distance < 1.5m, not wearing mask)"}
                        ],
                        visibleIf: "{firstTest}='1'"
                    },
                    {
                        type: "html",
                        name: "negNoSymptoms",
                        html: moreExposed,
                        visibleIf: "{exposureRisk}='2'"
                    },
                    {
                        type: "html",
                        name: "negNoSymptoms",
                        html: lessExposed,
                        visibleIf: "{exposureRisk}='1'"
                    },
                    {   type: "dropdown",
                        name: "meetingPeople",
                        title: "After exposure did you mix with people at work?",
                        choices: [
                            {value: 1, text: "Yes, I mixed with people"},
                            {value: 2, text: "No, I did not mix with people"}
                        ],
                        visibleIf: "{exposureRisk} notempty or {negativeSymptoms} notempty"
                    },
                    {
                        type: "html",
                        name: "didNotMeet",
                        html: "<h3>Good Job. Make sure you follow the instructions above</h3>",
                        visibleIf: "{meetingPeople}='2'"
                    },
                    {   type: "dropdown",
                        name: "youTested",
                        title: "Are you tested for COVID-19??",
                        choices: [
                            {value: 1, text: "Yes, I'm tested"},
                            {value: 2, text: "No, I'm not tested"}
                        ],
                        visibleIf: "{meetingPeople}=1"
                    },
                    {
                        type: "html",
                        name: "didNotTest",
                        html: "<h4>Other people exposed to you should follow these instructions</h4> <ol> <li>Continue duties</li> <li>No test is required</li> <li>Active monitoring for 14 days</li> </ol>",
                        visibleIf: "{youTested}='2'"
                    },
                    {
                        type: "html",
                        name: "didTest",
                        html: "<h4>Other people exposed to you should follow these instructions</h4> <ol> <li>Stop duties</li> <li>Test is required</li> <li>Isolation for 14 days</li> </ol>",
                        visibleIf: "{youTested}='1'"
                    },


                ]
            };

            var model = new SurveyVue.Model(json)

            return {
                survey: model
            }
        }
    }
</script>
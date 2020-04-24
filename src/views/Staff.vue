<template>
    <div id="app">
        <h2>Direct Exposure Questionnaire</h2>
        <h4>Complete this if you were directly exposed to COVID-19 from a relative or a friend</h4>
        <survey :survey="survey"></survey>
    </div>
</template>

<script>
    //In your VueJS App.vue or yourComponent.vue file add these lines to import
    import * as SurveyVue from 'survey-vue'
    import 'bootstrap/dist/css/bootstrap.css';
    var Survey = SurveyVue.Survey
    Survey.cssType = "bootstrap";

    export default {
        name: 'app',
        components: {
            Survey
        },
        data () {
            var json = {
                "showNavigationButtons": "none",
                questions: [
                    {   type: "dropdown",
                        name: "exposureType",
                        title: "How were you exposed?",
                        choices: [
                            "Direct Exposure (Test Positive)",
                            "Indirect Exposure (Symptomatic)",
                            "Indirect Exposure (Asymptomatic)",
                            "In Contact with Indirect Exposure"
                        ]
                    },
                    {
                        type: "html",
                        name: "asympExposure",
                        html: "<h4>Follow all the following steps:</h4> <ol> <li>Continue duties with mask</li> <li>No test is required</li> <li>Active monitoring for 14 days</li> </ol>",
                        visibleIf: "{exposureType}='Indirect Exposure (Asymptomatic)'"
                    },
                    {
                        type: "dropdown",
                        name: "exposureType2",
                        title: "What type of exposure?",
                        choices: [
                            "Test Positive",
                            "Test Negative"
                        ],
                        visibleIf: "{exposureType}='In Contact with Indirect Exposure'"
                    },
                    {
                        type: "html",
                        name: "exposureNegative",
                        html: "<h4>Follow all the following steps:</h4> <ol> <li>Continue duties</li> <li>No test is required</li> <li>Active monitoring for 14 days</li> </ol>",
                        visibleIf: "{exposureType2}='Test Negative'"
                    },
                    {
                        type: "html",
                        name: "testPositive",
                        html: "<h4>Follow all the following steps:</h4> <ol> <li>Stop duties</li> <li>Test is required</li> <li>Isolation for 14 days</li> </ol>",
                        visibleIf: "{exposureType2}='Test Positive' or {exposureType}='Direct Exposure (Test Positive)' or {exposureType}='Indirect Exposure (Symptomatic)'"
                    },
                    {
                        type: "dropdown",
                        name: "testAgain",
                        title: "Test again",
                        choices: [
                            "Test Positive",
                            "Test Negative"
                        ],
                        visibleIf: "{exposureType2}='Test Positive' or {exposureType}='Direct Exposure (Test Positive)' or {exposureType}='Indirect Exposure (Symptomatic)'"
                    },
                    {
                        type: "html",
                        name: "testPositive2",
                        html: "<h4>Return to work after:</h4> <ol> <li>Isolation for 14 days post exposure</li> <li>Repeat test on day 13 post exposure</li> </ol>",
                        visibleIf: "{testAgain}='Test Positive'"
                    },
                    {
                        type: "html",
                        name: "testNegative2",
                        html: "<h4>Return to work after:</h4> <ol> <li>Asymptomatic for 48 hours</li> <li>Isolation for 14 days post exposure</li> <li>At least 2 negative tests for COVID-19</li> </ol>",
                        visibleIf: "{testAgain}='Test Negative'"
                    }
                ]
            };

            var model = new SurveyVue.Model(json)

            return {
                survey: model
            }
        }
    }
</script>
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
                questions: [
                    {   type: "dropdown",
                        name: "exposureType",
                        title: "How were you exposed?",
                        isRequired: true,
                        choices: [
                            "Direct Exposure (Test Positive)",
                            "Indirect Exposure (Symptomatic)",
                            "Indirect Exposure (Asymptomatic)",
                            "In Contact with Indirect Exposure"
                        ]
                    },
                    {
                        type: "dropdown",
                        name: "exposureType2",
                        title: "What type of exposure?",
                        isRequired: true,
                        choices: [
                            "Test Positive",
                            "Test Negative"
                        ],
                        visibleIf: "{exposureType}='In Contact with Indirect Exposure'"
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
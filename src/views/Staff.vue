<template>
    <div id="app">
        <h2>Exposure Questionnaire</h2>
        <h4>Complete this if you had a direct or indirect exposure to COVID-19</h4>
        <survey :survey="survey"></survey>
    </div>
</template>

<script>
    //In your VueJS App.vue or yourComponent.vue file add these lines to import
    import * as SurveyVue from 'survey-vue'
    import 'bootstrap/dist/css/bootstrap.css';
    var Survey = SurveyVue.Survey
    Survey.cssType = "bootstrap";

    const lessExposed = "<h4>You will need to:</h4> <ol> <li>Continue duties</li> <li>No test is required</li> <li>Active monitoring for 14 days</li> </ol>"
    const moreExposed = "<h4>You will be assessed by an EH physician. You will need to:</h4> <ol> <li>Stop duties</li> <li>Test is required</li> <li>Isolation for 14 days</li> </ol>"

    const completedHTML = "<h3>Thank you for completing the questionnaire, {name}</h3>"

    export default {
        name: 'app',
        components: {
            Survey
        },
        data () {
            var json = {
                "showNavigationButtons": "none",
                clearInvisibleValues: "onHidden",
                completedHtml: completedHTML,
                questions: [
                    {
                        name: "name",
                        type: "text",
                        title: "What's your name:",
                        placeHolder: "Please enter your name",
                        isRequired: true
                    },
                    {
                        name: "MRN",
                        type: "text",
                        title: "What's your MRN Number:",
                        placeHolder: "Please enter your MRN",
                        isRequired: true
                    },
                    {   type: "dropdown",
                        name: "Was the person you were exposed to tested",
                        title: "Was the person you were exposed to tested?",
                        description: "Remember that National ID is required by the IPC to retrieve a person's test results",
                        choices: [
                            {value: "yes", text: "Yes, they were tested"},
                            {value: "no", text: "No, they were not tested"}
                        ]
                    },
                    {   type: "dropdown",
                        name: "What was the test result of the person you were exposed",
                        title: "What was the test result of the person you were exposed to?",
                        choices: [
                            {value: "positive", text: "Test positive"},
                            {value: "negative", text: "Test negative"},
                            {value: "pending", text: "Test result is pending"}
                        ],
                        visibleIf: "{Was the person you were exposed to tested}=yes"
                    },
                    {   type: "dropdown",
                        name: "Experiencing symptoms",
                        title: "Are you experiencing any symptoms?",
                        description: "Symptoms may include fever, cough, shortness of breath, nausea, or diarrhea",
                        choices: [
                            {value: "yes", text: "Yes, I'm experiencing symptoms"},
                            {value: "no", text: "No, I'm not experiencing symptoms"}
                        ],
                        visibleIf: "{What was the test result of the person you were exposed}='negative' or " +
                            "{Was the person you were exposed to tested}=no " +
                            "or {What was the test result of the person you were exposed}='pending'"
                    },
                    {
                        type: "html",
                        html: moreExposed,
                        visibleIf: "{Experiencing symptoms}='yes'"
                    },
                    {
                        type: "html",
                        html: lessExposed,
                        visibleIf: "{Experiencing symptoms}='no' and {What was the test result of the person you were exposed}='negative'"
                    },
                    {
                        type: "html",
                        html: "<h3>Please maintain social distancing at work, wear mask, and perform hand hygiene</h3><h4>EH will contact you once the result is available</h4>",
                        visibleIf: "{Experiencing symptoms}='no' and {What was the test result of the person you were exposed}='pending'"
                    },
                    {   type: "dropdown",
                        name: "Exposure risk",
                        title: "Was your exposure low risk or high risk?",
                        choices: [
                            {value: "low risk", text: "My exposure was low risk (e.g. contact < 20 mins, distance > 1.5m, exposed with mask)"},
                            {value: "high risk", text: "My exposure was high risk (e.g. contact > 20 mins, distance < 1.5m, exposed without mask)"}
                        ],
                        visibleIf: "{What was the test result of the person you were exposed}='positive'"
                    },
                    {
                        type: "html",
                        html: moreExposed,
                        visibleIf: "{Exposure risk}='high risk'"
                    },
                    {
                        type: "html",
                        html: lessExposed,
                        visibleIf: "{Exposure risk}='low risk'"
                    },
                    {   type: "dropdown",
                        name: "Did they meet people",
                        title: "After exposure did you come into contact with people at work?",
                        choices: [
                            {value: "yes", text: "Yes, I mixed with people"},
                            {value: "no", text: "No, I did not mix with people"}
                        ],
                        visibleIf: "{Exposure risk} notempty or {Experiencing symptoms} notempty"
                    },
                    {
                        type: "html",
                        html: "<h3>Thank you. Make sure you follow the instructions above</h3>",
                        visibleIf: "{Did they meet people}='no'"
                    },
                    {
                        type: "comment",
                        name: "people who came in contact",
                        title: "Please provide the names of the people you came in contact with",
                        visibleIf: "{Did they meet people}='yes'"
                    },
                ]
            };

            const model = new SurveyVue.Model(json);

            model
                .onComplete
                .add(function (result) {
                    var element = document.createElement('a');

                    const data = result.data
                    element.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(JSON.stringify(result.data, null, 3)));
                    element.setAttribute('download', "Results for Patient {0} - {1}".format(data.name, data.MRN));

                    element.style.display = 'none';
                    document.body.appendChild(element);

                    element.click();

                    document.body.removeChild(element);
                    console.log(result.data);
                });

            return {
                survey: model
            }
        }
    }
</script>
<template>
  <div> 
    <h2>Workato Settings</h2>
    <vue-form-generator :schema="schema" :model="model" :options="formOptions"></vue-form-generator>
  </div>
</template>

<script>
import axios from "axios";
let RecipeJSON = {
    parameters: {
        status: "modified - v2",
        work: "Hybrid"
    },
    parameters_schema: [{
            "label": "Status",
            "name": "status",
            "type": "string",
            "control_type": "text",
            "hint": "add some text"
        },
        {
            "label": "work mode",
            "name": "work",
            "type": "string",
            "control_type": "select",
            "pick_list": [
                [
                    "WFH",
                    "WFH"
                ],
                [
                    "WFO",
                    "WFO"
                ],
                [
                    "Hybrid",
                    "Hybrid"
                ],
                [
                    "WFA",
                    "WFA"
                ]
            ]
        }
    ]
};
let veuJSON = {};
let updateObject = function(object) {
    veuJSON.model = object.parameters;
    let fields = object.parameters_schema;
    transformFields(fields);
    veuJSON.schema = {};
    veuJSON.schema['fields'] = fields;
}
let transformFields = function(fields) {
    for (let i = 0; i < fields.length; ++i) {
        let field = fields[i];
        // get all field values;
        let inputType = field.control_type;
        let placeholder = field.hint;
        let validator = field.type;
        let model = field.name;
        // delete field values
        delete field.control_type;
        delete field.hint;
        delete field.type;
        delete field.name;

        // map values
        if (inputType === 'select') {
            field.type = 'select';
            field.values = [];
            let options = field.pick_list;
            for (let i = 0; i < options.length; ++i) {
                field.values.push(options[i][0]);
            }
        } else {
            field.type = 'input';
            field.inputType = 'text';
        }
        field.placeholder = placeholder;

        field.validator = validator

        field.model = model;
    }
}

updateObject(RecipeJSON);


export default {
    data: () => (veuJSON),
    mounted() {
        axios.get("https://www.workato.com/api/managed_users/604353/recipes/2389626", {
                headers: {
                    'x-user-email': 'workato@chargebee.com',
                    'x-user-token': '410e91ce-9906-4408-b577-e332b0524190'
                }
            })
            .then(response => {
                RecipeJSON = [...response.data]
            })
    }
};
</script>
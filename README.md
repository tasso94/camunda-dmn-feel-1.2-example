## Example: DMN FEEL 1.2 for Camunda BPM Platform

Let's have a look at the following example to get a better understanding of how **FEEL 1.2** integrates 
with Camunda BPM.

In this example, we want to calculate the maximum credit sum that we can grant to a customer based 
on the requested type of the credit and the credit score. 

When the credit score of a customer is…

* ... weaker than 80% of the average credit score:\
no credit is granted

* ... between 80% and 100% of the average credit score:\
the historical average credit sum is granted

* ... better than the average credit score:\
1.5 times of the historical average credit sum is granted

Based on the business context explained above, I created a DMN Decison Table & Literal Expression 
which uses **FEEL 1.2 Expressions** exclusively.

![Decision Table]
![Literal Expression]

### Step-By-Step Guide

To try out the example yourself, follow the step-by-step guide below.

**Step 1: Run Camunda BPM**

Run the following command in your terminal:

```
mvn clean install && java -jar ./target/feel-example-1.0.0-SNAPSHOT.jar
```

**Step 2: Evaluate Decision**

Evaluate the decision by performing the following REST API request:

`POST /decision-definition/key/calc-credit-sum/evaluate`
```javascript
{
   "variables":{
      "credit_history":{
         "value":" [{\"type\":\"personal-loan\", \"score\": 505, \"granted_sum\": 1554.30}, {\"type\":\"mortgage\", \"score\": 931, \"granted_sum\": 600900.43}, {\"type\":\"mortgage\", \"score\": 754, \"granted_sum\": 210000.00}, {\"type\":\"personal-loan\", \"score\": 437, \"granted_sum\": 1900.44}]",
         "type": "Json"
      },
      "credit_score":{
         "value": 800
      },
      "credit_type":{
         "value": "mortgage"
      }
   }
}
```

**Step 3: Review Decision History**

Go to Cockpit and open the decision instance view to see the result:

![Camunda Cockpit "Decision Instance View"]

[Camunda BPM 7.13.0-alpha2]: https://camunda.com/download/
[calc-credit-sum.dmn]: ./src/main/resources/calc-credit-sum.dmn

[Decision Table]: ./assets/decision-table.png
[Literal Expression]: ./assets/literal-expression.png
[Camunda Cockpit "Decision Instance View"]: ./assets/cockpit-decision-instance-view.png
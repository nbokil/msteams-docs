---
title: Sequential workflows
description: Sample for sequential workflow using Universal Bot
ms.topic: conceptual
---
## Sequential workflows

Sequential workflow is supported when the same card is updated on user action and user can progress through a series of sequential cards. This is supported through `Action.Execute` which enables bot developers to return an adaptive card in response of an user action.

Example: Take a scenario where a restaurant wants to take an order. With `Action.Execute` the order for various items such as food, drinks etc can be taken sequentially.

<img src="~/assets/images/bots/sequentialWorkflow.gif" alt="Sequential workflow" width="200" height="300"/>

![Catering bot states](~/assets/images/bots/Cateringbotstates.png)

**Card 1 JSON payload**

```
{
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
  "type": "AdaptiveCard",
  "version": "1.4",
  "body": [
    {
      "type": "TextBlock",
      "text": "Select from food options"
    },
    { 
      "type": "ActionSet",
      "actions": [
        {
          "type": "Action.Execute",
          "title": "Chicken",
          "verb": "food",
          "data": {
              "item": "chicken"
          }
        },
        {
          "type": "Action.Execute",
          "title": "Beef",
          "verb": "food",
          "data": {
              "item": "beef"
          }
        },
        {
          "type": "Action.Execute",
          "title": "Vegan",
          "verb": "food",
          "data": {
              "item": "vegan"
          }
        }
      ]
    }
  ]
}
```

For Action.Execute invoke the bot can return an adaptive card as response which would replace the existing card in Teams. 
Example: 
1. On food selection from Card 1, bot can return a card for selection of drinks (Card 2). 
2. On drink selection from Card 2, bot can return an order confirmation card (Card 3).
3. On order confirmation from Card 3, bot can return an order confirmed card (Card 4).

**Sample invoke request recieved on bot side**

```
{ 
  "type": "invoke",
  "name": "adaptiveCard/action",

  // ... other properties omitted for brevity

  "value": { 
    "action": { 
      "type": "Action.Execute", 
      "id": "", 
      "verb": "food",
      "data": { 
            "item": "vegan"
      } 
    },
    "trigger": "manual" 
  }
}
```

**Sample invoke response to return adaptive card**
```
string cardJson = "<adaptive card json>";
var card = JsonConvert.DeserializeObject(cardJson);

var adaptiveCardResponse = JObject.FromObject(new
 {
                statusCode = 200,
                type = "application/vnd.microsoft.card.adaptive",
                value = card
 });
```


## See also

* [Adaptive card actions in Teams](~/task-modules-and-cards/cards/cards-actions.md#adaptive-cards-actions)
* [How bots work](/azure/bot-service/bot-builder-basics?view=azure-bot-service-4.0&preserve-view=true)
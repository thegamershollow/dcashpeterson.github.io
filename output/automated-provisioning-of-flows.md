---
title: "Automated Provisioning of Flows"
date: "2021-06-09"
categories: 
  - "m365"
  - "power-automate"
coverImage: "modern-glass-architecture.jpg"
---

I have a client that needed to automate their site provisioning. We are using [Orchestry](https://www.orchestry.com/) to handle the site setup but there are some gaps in that provisioning. The biggest gap is that they need to attach workflows in PowerAutomate to do things like approvals and notifications. Out of the box when you create a new Flow in Power Automate and you can create a trigger on a specific list or library on a specific site. These connections and urls get stored in the manifest for the Flow. In this instance, once the site is created I need to add tasks with dates to a task list and then add an approval workflow to those tasks when they are marked for completion.

One word about this architecture. Depending on which triggers and actions you use in Power Automate you might need a premium license. In this instance our site provisioning has the ability to call a web hook at the end of the provisioning process. You could also trigger this based on when a site joins a hub or any other standard trigger. Also, there is a limit of 600 flows per user account so if you are going to be doing loads of sites or flows this might not be the right choice.

Here is an example of the architecture for this solution using Power Automate.

[![](https://spdcp.com/wp-content/uploads/2021/02/copy-flow-workflow.jpg?w=493)](https://spdcp.com/wp-content/uploads/2021/02/copy-flow-workflow.jpg)

The first thing I needed to do was create the Cloud flow that is going to do the work. I got it up and running using the appropriate business logic. For this example I am going to send an email notification but you can create a Cloud flow that matches your business need. Make sure that it is working the way you want it. This will be our "template" that will get copied to all the sites after they are provisioned.

[![](https://spdcp.com/wp-content/uploads/2021/06/flow-template.jpeg?w=814)](https://spdcp.com/wp-content/uploads/2021/06/flow-template.jpeg)

[![](https://spdcp.com/wp-content/uploads/2021/02/unpack-zip.jpg?w=380)](https://spdcp.com/wp-content/uploads/2021/02/unpack-zip.jpg)

When you are done export the flow and save the file to your desktop. This generates a series of JSON files. that get package up as a .zip file. Extract this zip file and find the definition.json file. Open the definition.json file in a text editor. I use VS Code. In the definition.json file you will find lots of different nodes. For this example we are going to focus on the triggers, actions, and connections nodes.

Now that have the template created and exported we can create the flow that copies the template and sets it up on the new site.

Create a new Cloud flow called Copy Flow. I used the When HTTP request is received trigger but you can use the trigger of your choice depending on how you want to do the copying. You will need to pass the URL of the new site and the name of the list or library to attach the flow to. the Request Body JSON Schema looks like this.

```
{
    "type": "object",
    "properties": {
        "siteUrl": {
            "type": "string"
        },
        "libraryName": {
            "type": "string"
        }
    }
}
```

The first step is getting the GUID of the library. I did that by using the Send an HTTP request to SharePoint and using the REST api to get the GUID.

[![](https://spdcp.com/wp-content/uploads/2021/02/get-library-guid.jpg?w=596)](https://spdcp.com/wp-content/uploads/2021/02/get-library-guid.jpg)

You will need to use the Parse JSON action to parse the data from the Get Library GUID call. I slimmed down the payload to only the GUID of the list by using the following sample payload.

```
{
  "type": "object",
  "properties": {
    "d": {
      "type": "object",
      "properties": {
        "Id": {
          "type": "string"
        }
      }
    }
  }
}
```

Look in the definition.json file in the triggers node. This is the code that the flow uses to wire itself up to the correct site and correct list.

```
{
        "manual": {
          "splitOn": "@triggerBody()['rows']",
          "type": "Request",
          "kind": "ApiConnection",
          "inputs": {
            "schema": {
              "type": "object",
              "properties": {
                "rows": {
                  "type": "array",
                  "items": {
                    "type": "object",
                    "properties": {
                      "entity": {
                        "type": "object",
                        "properties": {
                          "itemUrl": {
                            "title": "itemUrl",
                            "type": "string"
                          }
                        },
                        "required": []
                      }
                    },
                    "required": []
                  }
                }
              },
              "required": []
            },
            "host": {
              "connection": {
                "name": "@parameters('$connections')['shared_sharepointonline']['connectionId']"
              },
              "api": {
                "runtimeUrl": "https://unitedstates-002.azure-apim.net/apim/sharepointonline"
              }
            },
            "operationId": "GetItemHybridTriggerSchema",
            "parameters": {
              "dataset": "https://test.sharepoint.com/sites/TestFlowCreation",
              "table": "9c719235-bf7f-4fc3-8369-e01970ddaf96"
            }
          }
        }
      }
```

The highlighted lines are the hooks to the site and library. The dataset is the URL of the site and table is the GUID of the library. We will be replacing these with the data from the new site.

Create a string variable and copy in the content of the triggers node from definition.json. Replace the dataset with the site url of the new site and table with the GUID of the list from the previous step.

[![](https://spdcp.com/wp-content/uploads/2021/02/stringtrigger.jpg?w=602)](https://spdcp.com/wp-content/uploads/2021/02/stringtrigger.jpg)

The next step is to serialize the trigger string variable into JSON using the Parse JSON action.

[![](https://spdcp.com/wp-content/uploads/2021/02/get-flow.jpg?w=600)](https://spdcp.com/wp-content/uploads/2021/02/get-flow.jpg)

Now that we have the new trigger as an object in our Flow we need to create the copy of the flow. First we need to get the template flow we created using the Get Flow Action. We are going to use this action to retrieve the full definition from the template and only replace the trigger. We do this with another Compose action.

[![](https://spdcp.com/wp-content/uploads/2021/02/compose-new-trigger.jpg?w=597)](https://spdcp.com/wp-content/uploads/2021/02/compose-new-trigger.jpg)

```
setProperty(outputs('Get_Flow')?['body/properties/definition'],'triggers',body('Parse_Triggers_JSON'))
```

The input for the Compose New Trigger needs to set the Triggers property of the flow definition. We do that with the setProperty expression and replace it with the body of the parsed JSON of the trigger. If your action names are different you will need to replace Get\_Flow and Parse\_Triggers\_JSON with the names of your actions.

Before we create the flow we need one last piece of information. We need the connections from the definition.json file in order to create the flow and ensure we have the correct connections in the new flow. Copy the connectionReferences node from definition.json.

```
{
      "shared_sharepointonline": {
        "connectionName": "shared-sharepointonl-f5530182-ee60-4ed3-931f-956c98cbb4e5",
        "source": "Invoker",
        "id": "/providers/Microsoft.PowerApps/apis/shared_sharepointonline",
        "tier": "NotSpecified"
      },
      "shared_sendmail": {
        "connectionName": "shared-sendmail-0b82e697-7faa-41ee-b182-ee61-81a9dc4f",
        "source": "Invoker",
        "id": "/providers/Microsoft.PowerApps/apis/shared_sendmail",
        "tier": "NotSpecified"
      }
    }
```

We will need to manipulate this a little bit to create an array to enter in the Create Flow Action so that it looks like the code below. Using a text editor replace the open and close brackets with \[ and \]. Remove the names of the nodes and corresponding : (e.g. "shared\_sharepointonline":).

```
[
  {
    "connectionName": "shared-sharepointonl-f5530182-ee60-4ed3-931f-956c98cbb4e5",
    "source": "Invoker",
    "id": "/providers/Microsoft.PowerApps/apis/shared_sharepointonline",
    "tier": "NotSpecified"
  },
  {
    "connectionName": "shared-sendmail-0b82e697-7faa-41ee-b182-ee61-81a9dc4f",
    "source": "Invoker",
    "id": "/providers/Microsoft.PowerApps/apis/shared_sendmail",
    "tier": "NotSpecified"
  }
]
```

Now add a Create Flow action. Select the correct environment and give the new flow a name. If this is going to be provisioned across multiple sites you might want to include something like the site name in the flow name. In this example I am calling it Copied Flow. Mark the Flow State to Started.

Next to connection References click the T to the right. This changes it to a text box to enter the data into manually.

[![](https://spdcp.com/wp-content/uploads/2021/02/create-flow-connection-closed.jpg?w=594)](https://spdcp.com/wp-content/uploads/2021/02/create-flow-connection-closed.jpg)

[![](https://spdcp.com/wp-content/uploads/2021/02/create-flow-connection-open.jpg?w=595)](https://spdcp.com/wp-content/uploads/2021/02/create-flow-connection-open.jpg)

In the text box paste the connection array from the text editor. Finally in the Flow Definition select the Output from the Compose New Trigger action. The final action should look like this.

[![](https://spdcp.com/wp-content/uploads/2021/02/create-flow.jpg?w=599)](https://spdcp.com/wp-content/uploads/2021/02/create-flow.jpg)

At this point you should have everything you need. Save the Flow and test it. Your copied flow should be provisioned and attached to the new site and list or library.

I hope this helped. Happy coding.

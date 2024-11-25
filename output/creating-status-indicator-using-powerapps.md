---
title: "Creating Status Indicator using PowerApps"
date: "2021-03-01"
categories: 
  - "m365"
  - "o365"
  - "powerapps"
coverImage: "adobestock_37001853.jpeg"
---

I was working on creating a demo for a conference session on user experience and design for Power Apps and part of that included creating a PowerApp to illustrate the point. In the session we talk about the [10 Usability Heuristics for User Interface Design](https://www.nngroup.com/articles/ten-usability-heuristics/) and how to map them to the work we do with the Power Platform. One of those heuristics is **Visibility of system status**.

What this means is:  
**The design should always keep users informed about what is going on, through appropriate feedback within a reasonable amount of time.** When users know the current system status, they learn the outcome of their prior interactions and determine next steps. Predictable interactions create trust in the product as well as the brand.

In this example I created a procurement system that lets users add and track a contract request through the different stages of the process, New, In Review, Pending, Approved, Denied, More Information. I wanted to create a way for users to quickly know where their request was in the system.

First I create a new Canvas App and connected it to my datasource. In this instance it was a SharePoint list. The list contained a Choice field called Request Status and had the following values, New, In Review, Pending, Approved, Denied, More Information. The field has a default value set to New so there will always be a status in the list item.

On the landing screen I used a Gallery Component and connected it to my data source. In the gallery I added a circle and a label.

[![](https://spdcp.com/wp-content/uploads/2021/02/landing-screen-powerapp-circle.jpg?w=355)](https://spdcp.com/wp-content/uploads/2021/02/landing-screen-powerapp-circle.jpg)

[![](https://spdcp.com/wp-content/uploads/2021/02/landing-screen-powerapp-label.jpg?w=257)](https://spdcp.com/wp-content/uploads/2021/02/landing-screen-powerapp-label.jpg)

For the circle I set the Fill property using a switch statement. All the values except for Denied and Requesting More Information are going to be green so I used Green as the default in my Switch and the other conditions used Red. [Check out the documentation for If and Switch statements if you need some help](https://docs.microsoft.com/en-us/powerapps/maker/canvas-apps/functions/function-if).

```
Switch(
    ThisItem.'Request Status'.Value,
    "Denied", Red,
    "Requesting More Information", Red,
    Green
)
```

For the label I wanted to make sure that it showed the text of the status so I set the Text field to

```
ThisItem.'Request Status'.Value
```

Now for each item in the gallery it will change the color of the circle depending on the status and display that status. This ensures that when the user looks at each request there is not only a text indicator but also a visual indicator to ensure that they know where their request is in the status.

[![Power App Gallery Control with status indicator](https://spdcp.com/wp-content/uploads/2021/02/landing-screen-powerapp-2.jpg?w=619)](https://spdcp.com/wp-content/uploads/2021/02/landing-screen-powerapp-2.jpg)

The Details screen was a little more challenging as I wanted to provide a more visual way to see the progression of the task and what the next step was. I decided to create a train stop type of status indicator. This way the user not only knew the current state of their request they also knew what the next steps were.

I chose to create a form and set it as the View form. Using the OnVisible property I used another Switch statement to set a variable called varStatusNum based on the different status levels. You can see in the code below that I set the variable to 1 for new and increment the value based on the different status values. Keep in mind that this is case sensitive so **New** is not the same as **new**. You need to use the values that are in your Choice field.

```
Switch(
    ThisItem.'Request Status'.Value,
    "New",Set(varStatusNum,1),
    "In Review",Set(varStatusNum,2),
    "Pending",Set(varStatusNum,3),
    "Approved",Set(varStatusNum,4),
    "Denied",Set(varStatusNum,5),
    "Requesting More Information",Set(varStatusNum,6)
)
```

This sets up a numeric variable so that you can easily do comparisons and not have to use a bunch of long switch statements in the various circles and repeat code.

The next thing I did was add the UI components. At the top of the form. I created a custom data card and added it to the form. I added a circle for each status I had, a label for each status, and a line to connect them. I grouped these together for ease of moving them around the page and put this at the top of the form. For the final step in the train stop I added some additional logic to change the displayed text depending on the status Approved, Denied, Requesting More Information.

```
If(
    varStatusNum = 4,"Approved",
    varStatusNum = 5,"Denied",
    varStatusNum = 5,"Requesting More Information",
    "Approved"
)
```

[![](https://spdcp.com/wp-content/uploads/2021/02/train-stop-2.jpg?w=612)](https://spdcp.com/wp-content/uploads/2021/02/train-stop-2.jpg)

In the Fill property for each circle I used varStatusNum to set the color of the Fill property like this adjusting for each step in the process. If the status is New or In Review then set the Fill color to Green, if not set it to white.

```
//Set the fill to Green if you are In Review
If(
    varStatusNum >= 2,Green,
    RGBA(255,255,255,1)
)
```

For the final step I wanted to include all three possible status options Approved, Denied, Requesting More Information so I extended the if.

```
If(
    varStatusNum = 4,Green, //Approved
    varStatusNum > 4,Red, //Denied or More Info
    RGBA(255,255,255,1) //White
)
```

[![](https://spdcp.com/wp-content/uploads/2021/02/train-stop2.jpg?w=617)](https://spdcp.com/wp-content/uploads/2021/02/train-stop2.jpg)

That's it. Now when a new request is created the request will be added to the SharePoint list with a status of New. When the user uses the PowerApp to view the details of the request on subsequent visits they can see the status of the request as well as what the next step. This increases the visibility into the system status making the user feel like they have a clear understanding of where things are at and what's next. That leads to a more usable system and happier end users.

I hope you enjoyed this and found it helpful.

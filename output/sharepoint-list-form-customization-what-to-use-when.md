---
title: "SharePoint List Form Customization: What to use when"
date: "2024-02-14"
tags: 
  - "microsoft"
  - "microsoft-365"
  - "power-apps"
  - "sharepoint"
  - "sharepoint-online"
coverImage: "thomas-lefebvre-gp8blyataa0-unsplash.jpg"
---

This post was created in collaboration with [Syskit](https://www.syskit.com/blog/sharepoint-list-formatting/?_gl=1*8x8um5*_up*MQ..&gclid=Cj0KCQiA5rGuBhCnARIsAN11vgS3w3agzqx_BJA5RN59Eb-1S-nRp7H5qVGTQPzA22BzMBdoYuL2Ay0aAh3GEALw_wcB).

Managing documents and data in SharePoint is easy, but when it comes to form data, a challenge can lie in the user interface for data entry. By leveraging different Microsoft 365 (M365) functionality and adhering to a few user experience principles, we can streamline user interaction, decrease data entry errors, and increase our users’ satisfaction. The key is understanding which SharePoint list formatting customization method suits the purpose and when to implement it effectively. 

## User Experience Principles 

The [Law of Proximity](https://lawsofux.com/law-of-proximity/), which states that objects close to each other are perceived as grouped. Similarly, the [Law of Common Regions](https://lawsofux.com/law-of-common-region/) indicates that elements sharing a visual boundary are seen as belonging together. These principles guide our approach to SharePoint list form customizations. 

## **Challenges with Traditional SharePoint Lists** 

Traditional SharePoint lists stack fields without clear visual hierarchy or headers. You can enter the data in the information panel or edit it in the new/edit form. The fields are stacked on top of each other, going from top to bottom. These forms are fine if you have a small number of fields in the form. With longer forms, this can cause confusion, especially for forms with different sections that are filled out by different business groups or forms that have a multi-step process.

This stacked approach can make users feel frustrated that the form is long, resulting in users filling out the wrong fields for this step in the process or filling out fields that don’t belong to their group. This is an example of a project task tracking list item using a SharePoint list:

[![](https://spdcp.com/wp-content/uploads/2024/02/newform.png?w=716)](https://spdcp.com/wp-content/uploads/2024/02/newform.png)

## SharePoint Form **Customization Options** 

There are several customization options, each with its own set of pros and cons. Lets help you choose the right SharePoint form customization option:

### SharePoint **List Formatting** 

SharePoint list formatting allows customizing the appearance and layout of lists through the SharePoint user interface. It permits **showing or hiding columns** based on data in other fields, **simplifying form design** by displaying only relevant fields. It also allows us to use JavaScript Object Notation (JSON), to **organize fields under specific headers**.

We can group fields into sections, applying the Law of Common Regions to group related data logically. We can show or hide fields based on other values by leveraging list formatting options. In this case, we are looking at the due date and showing a field based on if it is overdue.

[![](https://spdcp.com/wp-content/uploads/2024/02/columnformatting.png?w=1024)](https://spdcp.com/wp-content/uploads/2024/02/columnformatting.png)

#### **SharePoint list formatting pros** 

- **Easy implementation:** Editing code snippets facilitates quick customizations.

- **Cross-platform compatibility**: Ensures consistent experiences across devices.

- **Cost-effective**: Built-in within SharePoint, no additional licensing expenses.

#### **SharePoint list formatting cons** 

- **Limited functionality**: Complex logic and interactivity have constraints compared to other options.

- **Technical expertise needed**: Understanding JSON might be challenging for some users.

### **Power App List Form Customization** 

Power Apps allow us to **create tailored forms** with intricate layouts, detailed business logic, and **multi-stage forms**. It offers flexibility in arranging form fields on the canvas, improving user experience by grouping relevant elements. We can use the law of common regions to group like form fields together. We can choose to show or hide fields based on conditional logic.

We can also split the form into different pages or sections depending on the stage of the request. In this example of the same project task tracking, we are using a Power App to provide a two-column layout for the form as well as conditional formatting of the header if the task is overdue.

[![](https://spdcp.com/wp-content/uploads/2024/02/powerapp1.png?w=398)](https://spdcp.com/wp-content/uploads/2024/02/powerapp1.png)

[![](https://spdcp.com/wp-content/uploads/2024/02/powerapp2.png?w=397)](https://spdcp.com/wp-content/uploads/2024/02/powerapp2.png)

[![](https://spdcp.com/wp-content/uploads/2024/02/powerapp3.png?w=399)](https://spdcp.com/wp-content/uploads/2024/02/powerapp3.png)

#### **Power App list form customization pros**

- **High customizability**: Offers advanced controls, integrations, and conditional formatting.

- **Integration capabilities**: Seamlessly connects with Microsoft and third-party services.

- **Enhanced user experience:** Form builders can create a more intuitive user experience by grouping form fields together or by providing visual cues like a shared border or background to increase usability.

#### **Power App list form customization cons** 

- **Complexity and learning curve**: Building complex forms or business logic may require an in-depth understanding of Power Apps.

- **Licensing costs**: Advanced functionalities may incur additional expenses.

- **Limited portability**: Forms aren’t easily transferable between lists.

### **SPFx Form Customizers** 

SharePoint Framework grants developers full control to create custom solutions using standard web technologies. SPFx Form Customizers empowers developers to modify SharePoint list form layouts, applying conditional logic and visual cues. We can create different screens for the new, edit, and display forms. We can **completely customize the user interface** and the business **logic around the form**.

This can include making fields required based on the value of another field (e.g., if you select Other than Description, it is required). Here is an example of the same project task tracking for using an SPFx form customizer. As you can see, we have defined a custom user experience as well as used the Law of Common Regions to group related fields together with a gray background.

[![](https://spdcp.com/wp-content/uploads/2024/02/spfx1.png?w=729)](https://spdcp.com/wp-content/uploads/2024/02/spfx1.png)

You can also see how we have similar conditional logic to indicate if the task is overdue and also displayed additional fields that need to be filled out based on the current overdue status.

[![](https://spdcp.com/wp-content/uploads/2024/02/spfx2.png?w=717)](https://spdcp.com/wp-content/uploads/2024/02/spfx2.png)

#### **SPFx form customizer pros** 

- **Extensive customization**: Offers completely custom form solution.

- **Reusability:** Forms are tied to content types so once it is deployed it is available globally any time that content type is used.

- **Full control**: Enables precise customization of form elements and behaviors.

#### **SPFx form customizer cons** 

- **Development expertise required**: Requires proficiency in SPFx and web development. 

- **Maintenance overhead:** Custom solutions may need ongoing updates.

## Which is the best SharePoint list formatting customization method?

While SharePoint List formatting offers quick tweaks, Power App list form customization, and SPFx form customizers cater to diverse requirements.

Understanding their strengths and limitations helps organizations make informed decisions, tailoring SharePoint lists and forms to specific needs. With any of these solutions, it’s always good to keep user experience in mind.

--- 
title: the edit Form (classes with the stereotype editform) 
sidebar: flexberry-winforms_sidebar 
keywords: Flexberry Designer, Flexberry Winforms 
summary: Describes how to create edit form in the environment Flexberry Designer and specified that will be generated in the form's code based on the set in the designer properties 
toc: true 
permalink: en/fw_editform.html 
lang: en 
autotranslated: true 
hash: 7f00db96a052d79bf1fa5053faaa8eff17939b6dfe9a0a4d46243dba284c4151 
--- 

Description of the environment `Flexberry` regarding the user interface described in: [Tutorial programmer Flexberry Platform](gbt_flexberry-platform-guide.html) the "User interface "(paragraph 45-65). 

The edit form provides a user interface for entering/editing the instance of the data object in one or multiple views. 

To describe a form of editing must be created in the CASE of a UML class with the stereotype "editform". For example: 

!An example of an edit form on the graph](/images/pages/products/flexberry-winforms/forms/editform.png) 

## generated 

* Class UI-independent edit form that are inherited from ICSSoft.STORMNET.UI.BaseIndpdEdit. 
* .Net-dependent interface for editing form. 
* UI-related form editable (if necessary). 
* Attributes of the form are generated in the following way: 

* If you have enabled the generation of UI-dependent forms, is generated .Net-UI interface-dependent forms editing definition properties, but only if this attribute is public. 
* Code UI-specific form is generated which implements the above interface definition of virtual properties, as well as private member and both accessors code for setting/getting the value of a private member, followed by parentheses programmer. 
* Code UI-independent forms generated definition of virtual property with the specified modifier. If the generation of the UI-dependent form is included in generated code properties the same properties of the UI-dependent forms through the interface. 
* Methods of the form are generated in the following way: 

* If you have enabled the generation of UI-dependent forms, is generated .Net-UI interface-dependent form editing method definition, but only if this method is public. 
* Code UI-specific form is generated that implements the above interface definition of the virtual method, and in this parenthesis the programmer (similar to as described in [Methods of classes and method parameters](fd_methods-parameters.html)). 
* Code UI-independent form is generated by defining a virtual method with the specified modifier. If the generation of the UI-dependent forms included in the code generated method call the same method the UI-dependent forms through the interface. 

## Additional editable properties and how that is generated 

The most important thing in the advanced edit properties - select the class and view in which you want to edit an object of that class. This is on the "Composite view": 

![Submission for the edit form](/images/pages/products/flexberry-winforms/forms/editformviews.jpg) 

In `Flexberry Plugins` you can specify only one class and one view in which to edit the object in this form. If you specify multiple classes and/or performances, all but the first are ignored. 

So, in order to select a class and performance, you need to click on the " ... " button: 

![View](/images/pages/products/flexberry-winforms/forms/view-sel.jpg) 

### Tab "Form" 

![Property editor](/images/pages/products/flexberry-winforms/forms/editformprops.jpg) 

| Property | Description / Generation .Net language | 
|--|--| 
| `Name` - the name of the UML class | Name .Net class independent of the form and its derivative for the dependent (e.g., ```winform[Name]```). 
| `Description` - some description of class.| `DocComment` before the class definition an independent 
| `Caption` - a user name (shown in UI)| Title on the form 
| `GenerateDependedForm`| If the option is set, - generated UI-dependent (WinForm) form the user interface in the source code that allows you to "manually" place the form controls. If the option is not installed, use the generic UI-independent edit form where control elements corresponding to the attributes in the view are automatically. 
| `FixDependedForm`| If the option is set, - UI-dependent (WinForm) form not pregenerated. This is done to make sure that are made in the form of changes in the composition and arrangement of the controls will not be lost, which sometimes occurs due to the presence of (unfortunately) error in the environment `Visual Studio .Net`. <br / > If the option is not set generation is as follows: <br> 1. If the view (the name and message attributes) are not changed, then pregenerated nonvisual part of the form (methods, properties, etc.) ; <br> 2. If you completely changed the view (its name, i.e. after the last generation otherwise the view), the visual part pregenerated completely, all controls are removed and placed new ones in accordance with the new view. <br / >3. If changed message attributes view, on the form, added controls, added to the corresponding view attributes. El-you the control, respectively. remote attributes are not destroyed on the form. 
| `Packet, NamespacePostfix` - allow to set the Assembly and namespace | see [the Location of assemblies after code generation](fo_location-assembly.html).
| `PBCustomAttributes` - allows you to specify whether to brace the programmer immediately before the class definition, for "manual" of any attributes | If the option is specified - generated bracket of the programmer for manual application .Net attributes to class-independent form. 
| `PBMembers`| If the option is specified - generated bracket programmer for "manual" introduction of the members of the class of independent form. 
| `PropertyLookup`| Allows for a separate form to configure which list form to open this form to select the appropriate related objects (masters). 
| `EditFormOperations`| options available in the form of a transaction. 
| `PrintContainer` is the class name with the stereotype `printform`, which is a printed form that is used when printing from the edit form.| In code independent form is generated overload GetPrinter method, which:<br>* If `PrintContainer` is not specified, an error is returned `NoSuchContainerException`; <br> * If `PrintContainer` specified, the return type is контейнера; <br>In this method, before the return value is unmuted bracket of the programmer, which can be coded "manually" return type in the case of logic, which, for example, returns printed form depending on the properties of the object. 
| `ScriptName` - the script name to use this form. Matches the name of the EBSD-maps used to describe the scenario.| PstrfGetScript` method in class independent is overloaded so that it returns from the provider scenarios scenario with the specified name. 
| `PublishToEBSD`| If the option is specified before the class-independent form is generated to specify a `PublishToEBSDAttribute` attribute that specifies the availability of this class to use the chart editor scripts.| 

#### PropertyLookup 

`PropertyLookup` is generated in the method code `GetEditor` UI-independent edit form which returns the list type of the form based on behalf of artisan properties. 

![PropertyLookup](/images/pages/products/flexberry-winforms/forms/propertylookup.jpg) 

At the top of the list are the artisans of the properties in the lower drop-down list, select the form that needs to open for selection by the user respectively. the linked object. 

#### EditFormOperations 

Allows you to specify which operations are available on this form (respectively, which buttons appear on the control panel): 

* `Save` - сохранение; 
* `Save and Close` - preservation, followed by the closure формы; 
* `Print` - печать; 
* `Print preview` - preview. 

![EditFormOperations](/images/pages/products/flexberry-winforms/forms/editformoperations.jpg) 

Generated: 

* If you use a generic form (tick `GenerateDependedForm` not set), the generated parameter in the constructor of the dependent form in the method `GetDpdForm()` independent формы; 
* If you are using the generated form is generated directly in dependent form by setting the visibility of the toolbar buttons. 

### attribute Properties 

The properties of the attributes are the same as referred to in article [Attributes data classes](fo_attributes-class-data.html), given above (in paragraph 4 "What is generated") comments. 

### Properties methods 

Properties methods similar to those described in the article [class Methods and method parameters](fd_methods-parameters.html), given above (in item 5"What is generated") comments. 

## Related articles 

[E-view](fd_e-view.html) 



{% include callout.html content="Переведено сервисом «Яндекс.Переводчик» <http://translate.yandex.ru>" type="info" %}
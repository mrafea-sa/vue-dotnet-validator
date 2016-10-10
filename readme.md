#Vue dotnet validator
A vuejs validator for .NET forms.

## Summary
This package makes it possible to use .NET model validation using vue.js instead of the default jQuery validator way that microsoft dictates.
The idea is that you use this on your server rendered HTML forms which include the data-attributes that are generated by C#'s razor template engine.

## Requirements
This package (from version 0.3.0 and up) requires vue 2.x.

##Installation
`npm install vue-dotnet-validator`


##Usage

Using this library requires changes on two places in your application, JavaScript and your razor cshtml templates.

###JavaScript
This registers the vue components so that vue.js knows what to activate.
Base usage:
```JavaScript
import dotnetValidator from './dotnet-validator/dotnet-validator';
Vue.component('validator-group', dotnetValidator.validatorGroup);
Vue.component('validator', dotnetValidator.validator());

```


###Cshtml
The following code should be added to your cshtml forms. This makes sure that the validator logic is activated and adds the required references to DOM-nodes.
```HTML
<validator-group inline-template>
    <form asp-controller="Account" asp-action="Register" method="post" v-on:submit="validate">
        <validator value="@Model.LastName" inline-template>
           <span asp-validation-for="LastName" ref="message"></span>
           <input type="text" asp-for="LastName" ref="field" v-model="val" />
        </validator>
        <button type="submit">Register</button>
    </form>
</validator-group>
```


##Creating custom validators
It is possible to create your own validators, below is an example of a very simple custom validator.
###JavaScript
```JavaScript

class MyCustomValidator extends vueDotnetValidator.BaseValidator {
    isValid(value) {
        return !value || value == 'Hello';
    }
}
let validators = {
  Mycustomvalidator: MyCustomValidator
};

Vue.component('validator', dotnetValidator.validator(validators));

```

###Cshtml
To use this custom validator in your own form, make sure your custom .NET data annotation outputs a `data-val-mycustom="MESSAGE"` attribute on your `<input>` DOM node.

##Custom validators with additional parameters
You can extend the features of your custom validators using additional data-attributes on your `<input>` tag. This is a feature supported in .NET.
For an example on the usage of this feature, see `regexvalidator.js`.

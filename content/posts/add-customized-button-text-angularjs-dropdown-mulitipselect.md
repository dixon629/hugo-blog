+++
title = "Customize button text for angularjs-dropdown-mulitipselect"
date =  2016-04-03T17:33:35+08:00
draft = true
tags = ["angularjs"]
categories = ["javascript"]
+++
Recently I was using [angularjs-dropdown-multiselect](https://github.com/dotansimha/angularjs-dropdown-multiselect) for multiple select. It's a good library, but it doesn't support customizing button text very well, so I forked it and added the customized button text feature. 

The forked repository is https://github.com/dixon629/angularjs-dropdown-multiselect.
It's easy to use it, just specify **buttonTextFunction** in the **extraSettings**.
It passes three parameters: *options*, *selectedModel*, *settings*  

1. **options**: the data provider of dropdown list
2. **selectedModel**: the selected model of dropdown list
3. **settings**: the settings of dropdown list

This is a demo: how to display "All" when you check the all items for the dropdown
```javascript
function dropdownButtonTextFunction(options,selectedModel,settings){
    if(selectedModel && selectedModel.length == options.length){
        return 'All';
    }else{
        return selectedModel.length+' checked';
    }
}
$scope.myExtraSettings = {buttonTextFunction: dropdownButtonTextFunction};
```
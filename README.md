formidable
============

A jQuery based form manager. It handles application of labels to inputs as well as verifies inputs on change or before submission. Additionally, it facilitates followup actions after form submission (if the submission is asynchronous via AJAX)

It requires the following javascripts :

1. [jQuery 1.10 or above](http://jquery.com/download/)
2. [jQuery UI 1.10 or above](http://jqueryui.com/download/)
3. [jQuery qTip v2](http://qtip2.com/)
4. [jQuery History](https://github.com/balupton/jquery-history)

The associated css for jQuery UI will also be needed.

A demo of formidable in action can be found here : [Swinku sign-up page](http://swinku.com/signup.php)

To create a new form handler:

```javascript
FormHandler(form_node, validation_rules, formatting_rules, submission_options)
```

The parameter form_node can be a jQuery node or a jQuery selector string (such as '#form_id' or '.form_class')

The parameter validation_rules is an object which has the following structure:

```javascript
{'input_name':{
     null_value_allowed: true/false, //optional argument default true
     max_length: integer, //optional argument
     min_length: integer, //optional argument
     min_value: integer, //optional in case of numeric values
     max_value: integer, //optional in case of numeric values
     regexp: 'regular expression', //optional regular expression to match against
     onchange: function ( jquery_input_node, input_node_current_value, InputValidator,
              InputValidator_callback_function){
                    return 1;//for continued onchange event propagation
                    return 0;//to stop further onchange event propagation
                    return -1; // upon this return value InputValidator assumes that the function will issue a callback after it's done processing
              }, //the onchange parameter is also optional the FormHandler validates the input values onchange by default
     validate_func: function(node, value, InputValidator, InputValidator_callback_function){
          return 0;//InputValidator validation is discontinued after this return value
          return 1;//the InputValidator object continues validation
          return -1;// upon this return InputValidator assumes that the function will issue a callback after it's done processing
     },
     enable_validate: integer //possible values 0 => disable, 1 => on change, 2 => on submit, default: 1|2
 }}
 ```
 The parameter formatting_rules is an object which has the following structure:
 
 ```javascript
 {'input_name':{
      class: class name, //optional
      hovered_class: class_name, //optional
      focused_class: class_name, //optional
      label: label_text or { text:label_text, alignment: left/center/right}, //optional
      tooltip: tooltip_text, //optional
      icon:{pos: 'Apx Bpx', show:left/right}
  }}
 ```
 The parameter submission_options is an object which has the following structure:
 
 ```javascript
 {
   'submit_element': submit element node or jQuery selector, //required field
   'submission_url': url to submit to after validation, //required field
   'submit_method' : possible values 'GET', 'POST', {method:'GET'/'POST',
                                                       asynchronous: true/false, //optional parameter defaults to false
                                                       async_callback: function(FormHandler, ResponseData, FormHandler_Callback){},//optional and can only be specified if asynchronous is true,
                                                                                                       //FormHandler_Callback takes the ResponseData as argument
                                                       
                                                       }//optional parameter, default 'GET'
  }
  ```
The FormHandler object expects responses from the backend server in JSON format with the following structure:

```javascript
{
      'code':0, //0=>success, non-zero number indicates error
      'action': 0,//0=> do nothing, 1 => show info text, 2 => Hide form, 4 => Redirect to URL, 8 => Go back in history
      'redirect_url': url,
      'info_text': 'text message',
      'inputs_errors':
        {
            'input_name':{'message':'Any error messages', 'showMessage':true/false}
        }
}
```

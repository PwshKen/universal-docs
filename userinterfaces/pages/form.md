---
description: Forms that can be invoked to call scripts and APIs.
---

# Form

## Invoke a Script with a Form

Forms can be used to invoke scripts. The form component ties directly into the automation features to allow for the display of output, progress and respond to feedback. 

To invoke a script, select the target of Script and the script you'd like to invoke. 

![](../../.gitbook/assets/image%20%28252%29.png)

By default, no output or progress will be shown. The form will simply consist of a Save button. 

![](../../.gitbook/assets/image%20%28234%29.png)

If the script succeeds, you'll see a success result. If it fails, it will return a failure result. 

![](../../.gitbook/assets/image%20%28251%29.png)

### Viewing Output

You can view the output as the script runs by using the Show Output check box. 

![](../../.gitbook/assets/image%20%28248%29.png)

### Viewing Progress

To view the output of cmdlets like `Write-Progress`, you can use the Show Progress check box. While the script executes, you'll see the progress messages. 

![](../../.gitbook/assets/image%20%28254%29.png)

The script in this example simply shows progress. 

```text
1..10 | % { 
    Write-Progress -Activity "Processing..." -PercentComplete ($_ * 10)
    Start-Sleep 1
}
```

### Viewing Output after the script has run

To view the output of the script after it has run, you can use the Result Type drop down. The Text result type, will display output as text. 

![](../../.gitbook/assets/image%20%28246%29.png)

To view output as a table, select the result type of Table.

![](../../.gitbook/assets/image%20%28239%29.png)

### Responding to Feedback

Scripts that request feedback will prompt the user to enter more information. 

![](../../.gitbook/assets/image%20%28235%29.png)

An example of a script that may request feedback is one that uses `Read-Host`

```text
Read-Host "Enter some feedback"
```



## Invoke an API with a Form

Invoking an API with a form can be done by selecting API from the Target Type and then selecting the API endpoint you wish to call. 

![](../../.gitbook/assets/image%20%28259%29.png)

API endpoints do not support progress, feedback, or output while running the API. They do support returning results as text or tables. APIs that return 200 will show a Success result and other status codes will return Failure. 

Fields provided to an API endpoint will be sent as a single JSON object with properties for each field. 

## Fields 

You can specify fields for each form. Currently forms support text boxes, checkboxes and hidden values. Fields can have a tooltip description and marked as required. 

![](../../.gitbook/assets/image%20%28273%29.png)

### Fields in Scripts

Fields are provided to scripts as parameters. You can use a `param` block to capture these field values. 

For example, if you had a FirstName and LastName field, you would define the following param block in your script. 

```text
param(
    $FirstName,
    $LastName
)
```

### Fields in APIs

Fields are provided to APIs as a JSON body to the POST endpoint. Each field will be a property of the JSON object passed to the API. 

```text
$Fields = $Body | ConvertFrom-Json
$Fields.FirstName
$Fields.LastName
```

### Field Types

#### Checkbox

The checkbox field provides a checkbox the user can select and `$true` or `$false` will be sent to the target. 

#### Hidden

Hidden fields provide a value to the target that is not shown to the user. The value will be a string.

#### Textbox

The textbox field allows the user to enter any text into the field and it will be submitted to the target. The value will be a string. 

## Text

You can customize the text of numerous features of a field including the success and failure text, waiting on feedback text and button text. 

![](../../.gitbook/assets/image%20%28265%29.png)


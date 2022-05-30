# Time Sheet in Appscript #

## Algorithm ##

## Step 1:
* Open the New google sheet
## Step 2:
* Go to Extensions and Open Appscript
## Step 3:
* In Appscript We have HTML and Script
## Step 4:
* We can rename the project in Appscript
## Step 5:
* In HTML , create the time sheet or form by using Bootstrap
## Step 6:
* In google sheet , we have to enter the data what we want from the form
## Step 7:
* In script, we have to write code to store data in excel sheet from form which is created in HTML
## Step 8:
* Go to deploy option and for what app(web app,API Executable) we have to choose and copy the link
## Step 9:
* Also in HTML we have to insert that link in the script tag
## Step 10:
* Then we have to check by filling the form and we can store data in the google sheet

## Appscript
## Program for form in HTML
```Javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Time Sheet</title>
    <link href="https://maxcdn.bootstrapcdn.com/bootstrap/4.1.1/css/bootstrap.min.css" rel="stylesheet" id="bootstrap-css"> 
</head>
<body>
    
<div class="container register">
            <div class="row">
                <div class="col-md-12">
                        <div class="tab-pane fade show active text-align form-new" id="home" role="tabpanel" aria-labelledby="home-tab">
                            <h2 class="register-heading" align="center">Yavar Time Sheet</h2>
                            <br>
                            <div class="row register-form">
                                <div class="col-md-12">
                                    <form method="post" autocomplete="off" name="google-sheet">
                                        <div class="form-group">Name:
                                            <input type="text" name="Name" class="form-control" value="" width="50px" required=""/>
                                        </div>
                                        <div class="form-group">Project:
                                            <input type="text" name="Project" class="form-control" value="" required=""/>
                                        </div>
                                        <div class="form-group">Date:
                                            <input type="date" name="Date" class="form-control" value="" required=""/>
                                        </div>
                                        <div class="form-group">Time Duration:
                                            <input type="time" name="Time Duration" class="form-control" value="" required=""/>
                                        </div>
                                        <div class="form-group">
                                            <input type="submit" name="submit" class="btnSubmit btn-block" value="Submit" />
                                        </div>
                                    </form>
                                </div>
                            </div>
                        </div>
                </div>
            </div>
        </div>  
          <script>
            const scriptURL = 'https://script.google.com/macros/s/AKfycbzaZsWAaW6Fch8dVnUdOT_B_fFVt6rFOUgv5UztQ0NtWw_SFJpL9kXCxaP8LUYpJDrK/exec'
            const form = document.forms['google-sheet']
          
            form.addEventListener('submit', e => {
              e.preventDefault()
              fetch(scriptURL, { method: 'POST', body: new FormData(form)})
                .then(response => alert("Your data is submitted"))
                .catch(error => console.error('Error!', error.message))
            })
          </script>

        <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.1.1/js/bootstrap.min.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
</body>
</html>
```

## code.gs
```Javascript
var sheetName = 'Sheet1'
		var scriptProp = PropertiesService.getScriptProperties()

		function intialSetup () {
		  var activeSpreadsheet = SpreadsheetApp.getActiveSpreadsheet()
		  scriptProp.setProperty('key', activeSpreadsheet.getId())
		}

		function doPost (e) {
		  var lock = LockService.getScriptLock()
		  lock.tryLock(10000)

		  try {
			var doc = SpreadsheetApp.openById(scriptProp.getProperty('key'))
			var sheet = doc.getSheetByName(sheetName)

			var headers = sheet.getRange(1, 1, 1, sheet.getLastColumn()).getValues()[0]
			var nextRow = sheet.getLastRow() + 1

			var newRow = headers.map(function(header) {
			  return header === 'timestamp' ? new Date() : e.parameter[header]
			})

			sheet.getRange(nextRow, 1, 1, newRow.length).setValues([newRow])

			return ContentService
			  .createTextOutput(JSON.stringify({ 'result': 'success', 'row': nextRow }))
			  .setMimeType(ContentService.MimeType.JSON)
		  }

		  catch (e) {
			return ContentService
			  .createTextOutput(JSON.stringify({ 'result': 'error', 'error': e }))
			  .setMimeType(ContentService.MimeType.JSON)
		  }

		  finally {
			lock.releaseLock()
		  }
		}
```



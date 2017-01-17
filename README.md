# angular-upload-file
Angular directive to upload files for cross browser support 

###Step 1 : Add required plugins to Project
```sh
sudo cordova plugin add org.apache.cordova.file
sudo cordova plugin add org.apache.cordova.file-transfer
```
###Step 2 : Add Javascript file your code

```script
<script src="angular.min.js"></script>
<!-- shim is needed to support non-HTML5 FormData browsers (IE8-9)-->
<script src="ng-file-upload-shim.min.js"></script>
<script src="ng-file-upload.min.js"></script>
```
###Step 3 : Create a field inside html file
```html
<div class="app">
    <h1>File Upload</h1>
    <div id="main">
        <h3>Uploading file from Mobile App to PHP backend</h3>
        <div class="upload-body">
            <button type="file" class="upload" ngf-select="uploadFiles($file)">
            </button>
        </div>
        <div class="uploadprogress" style="display:none">
            <div id="picture_msg">
               Upload progress bar 
            </div>
        </div>
    </div>
</div>
```

```css
.uploadprogress{
    display: inline-block;
    border: 1px solid #000; 
    border-radius: 2px;
}
.uploadprogress div {
    font-size: smaller;
    background: red;
    width: 0%;
    color: #fff;
    padding: 1%;
}
```
###Step 4 : add service

```js
var app = angular.module('myApp', ['ngFileUpload']);
```

###step 5 :  Create Method for File Upload (File API Implementation)
```js
//upload on file select

$scope.uploadFiles = function(file) {
      var url = "https://your-domain.com/upload"; //upload url including folder name
      $scope.f = file;
      if (file) {
          file.upload = Upload.upload({
              url: url,
              data: {file: file}
          });

          file.upload.then(function (response) {
              console.log('Success ' + response.config.data.file.name + 'uploaded. Response: ' + response.data);
          }, function (response) {
              if (response.status > 0)
                  errorMsg = response.status + ': ' + response.data;
                  console.log(errorMsg);
          }, function (evt) {
              percent = Math.min(100, parseInt(100.0 * evt.loaded / evt.total)); //Progress bar
              document.getElementById('progressBarStatus').style.display = "block";
              document.getElementById('progress_msg').style.width = percent + "%";
              document.getElementById('progress_msg').innerHTML = percent + "%";
              //console.log(Math.min(100, parseInt(100.0 * evt.loaded / evt.total)));
          });
      }   
  };
  ```
  ###step 6 : Create PHP Script on server side to save file (Server Side Implementation)
```php
 <?php 
     // Directory where uploaded files are saved
     $dirname = "uploads/"; 
     // If uploading file
     if ($_FILES) {
        //print_r($_FILES);
        mkdir ($dirname, 0777, true);
        move_uploaded_file($_FILES["file"]["tmp_name"],$dirname."/".$_FILES["file"]["name"]);
     }
 ?>
  ```

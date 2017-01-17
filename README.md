# angular-upload-file
Angular directive to upload files for cross browser support 

###Step 1 : Add required plugins to Project
sudo cordova plugin add org.apache.cordova.file
sudo cordova plugin add org.apache.cordova.file-transfer

###Step 2 : Add Javascript code

//inject directives and services.
var app = angular.module('myApp', ['ngFileUpload']);

app.directive('fileInput', ['$parse', function($parse){
    return {
        restrict: 'A',
        link: function(scope, elm, attrs){
            if(typeof(scope.test) == undefined){
              scope.test = { "files": []}
            }
            if(typeof(scope.test.files) !== undefined){
              scope.test["files"] =[];
            }
            elm.bind('change', function(){
                $parse(attrs.fileInput)
                .assign(scope,elm[0].files)
                scope.$apply()
            })
        }
    }
}]);

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
              document.getElementById('progress_msg').style.width = percent + "%";
              document.getElementById('progress_msg').innerHTML = percent + "%";
              //console.log(Math.min(100, parseInt(100.0 * evt.loaded / evt.total)));
          });
      }   
  };

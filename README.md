(function(){
    "use strict";
    angular
        .module('roadrunner')
        .factory("ProfileResource", ProfileResource);

    ProfileResource.$inject = ['$resource', 'api', 'student.model'];
    
        function ProfileResource($resource, api, studentModel){
            var odataUrl = api.getOdataBaseUrl() + '/Students'; 
            var apiUrl = api.getApiBaseUrl() + '/Student';                       
            var Resource = $resource(odataUrl, {},
                 {
                    'getStudentSections': { method: 'GET', url: apiUrl + '/GetStudentDetail' + '/:id'},
                    'updateProfile': { method: 'POST', url: apiUrl + '/UpdateProfile' + '/:id' },
                    'updateGuardian': { method: 'POST', url: apiUrl + '/UpdateGuardian' + '/:id' },
                    'updateContact': { method: 'POST', url: apiUrl + '/UpdateContact' + '/:id' },
                    'updateClassAllocation': { method: 'POST', url: apiUrl + '/UpdateClassAllocation' + '/:id' },
                    'addClassAllocation': { method: 'POST', url: apiUrl + '/AddClassAllocation' + '/:id' },
                    'save': { method: 'POST', url: apiUrl + '/EnrollStudent' }
                 },
                 {
                     isodatav4: true
                 }
            );
            
            studentModel.extend(Resource);

            return Resource;

        };    
}());

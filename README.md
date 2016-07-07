-----------------------------Teacher.resource.js----------------------------------------------
(function(){
    'use strict';
    
    angular
        .module('Repositories')
        .factory('TeacherRepository', TeacherRepository);
    
    TeacherRepository.$inject = ['$resource'];
    /* @ngInject */
    function TeacherRepository($resource){      
        var teacherURl = 'http://roadrunnerapi.apphb.com/odata/Teachers';
        return $resource(teacherURl,{},{
            getAllTeachers: {method: 'GET'},
            getTeacherDetails: {method: 'GET', url: teacherURl + '(:teacherId)'}
            getTeacherDetails: {method: 'GET', url: teacherURl + '/:teacherId'} // if just api
        });
    }
    
})();
-----------------------------------------------------------------------------------------------


-------------------------------Teacher.service.js---------------------------------------------
(function(){
    'use strict';
    
    angular
        .module('teacher.module')
        .factory('TeacherService', TeacherService);
    
    TeacherService.$inject = ['TeacherRepository'];
    /* @ngInject */
    function TeacherService(TeacherRepository){
        var service = {
            getAllTeachers: getAllTeachers
        };
        
        return service;
        
        function getTeacher(id){
            return TeacherRepository.getTeacherDetails({teacherId: id}).$promise;
        }
        
        function getAllTeachers(){
            return TeacherRepository.getAllTeachers().$promise;
        }
    }
})();
-----------------------------------------------------------------------------------------------


-------------------------------Teacher.controller.js---------------------------------------------

(function(){
    'use strict';
    
    angular
        .module('teacher.module')
        .controller('TeacherController', TeacherController);
    
    TeacherController.$inject = ['TeacherService'];
    /* @ngInject */    
    function TeacherController(TeacherService){
        var vm = this;
        vm.test = "Test"
        var promise = TeacherService.getAllTeachers();
        
        promise.then(function(data){
            vm.teachers = data.value;  // this may change qccording to the api 
        }, function(err){
            console.warn(err);
        });
        
        console.log(promise);
    }
    
})();


-----------------------------------------------------------------------------------------------

-------------------------------post form data angular.js---------------------------------------------


(function(){
    'use strict';
    
    angular
        .module('teacher.module')
        .controller('TeacherController', TeacherController);
    
    TeacherController.$inject = ['TeacherService'];
    /* @ngInject */    
    
    function TeacherController(){
    
    var Resource = $odataresource(odataApi/Teachers, {},
               {
                   'query': { method: 'GET', isArray: true },
                   'update': { method: 'PUT', url: odataUrl + '(:teacherId)' },
                   'getTeacherDetails': { method: 'GET', url: odataUrl + '(:teacherId)' }
               }
            );
        
        function loadNewTeacher() {
            vm.teacher = new TeacherResource();
        };
        
        
        function submitEnrolment() {
            vm.enrolment_saving = true;
            vm.teacher.$save().then(function (data) {
                notify.success("Save success");
            }, function (error) {
                notify.error("Save failed");
            }).finally(function () {
                vm.enrolment_saving = false;
            })
        }
    
    }
})();

-----------------------------------------------------------------------------------------------

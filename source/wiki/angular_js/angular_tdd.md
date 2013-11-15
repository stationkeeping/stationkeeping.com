---
title: "Angular TDD"
---

Please read [my blog post](http://newtriks.com/2013/06/11/automating-angularjs-with-yeoman-grunt-and-bower/) for general setup.

## Test Types

1. Unit tests (./test/spec) - focused on isolating and testing small functional units of code i.e. a single class and it's methods. [Jasmine](http://pivotal.github.io/jasmine/) is used for unit testing.

* Run unit tests:
    
    `grunt test:unit`

* Run unit tests with autowatch enabled so tests are auto run on code change:
    
   `karma start`

2. End to end tests (./test/e2e) -  scenario based testing i.e. a series of user defined steps (high level). [Angular Scenario Runner](https://github.com/angular/angular.js/blob/master/src/ngScenario/dsl.js) is used for e2e testing and is based on Jasmine but does not have all its features available.

* Run e2e tests:
    
    `grunt test:e2e`

**NOTE**

Ensure the karma proxy uses the same port as the grunt configured localhost server.

Each test class begins with a *describe* block which declares a group of context specific behaviors that will be tested. 

A method named *beforeEach()* is used to run any setup code required to run the tests and as the name describes, is run before every test. All unit tests will create a new instance of the context being tested e.g.

    // load the controller's module
    beforeEach(module('knowledgevisionHtml5PlaylistappApp'));

Finally the *it()* method creates an example of behavior expected. This conveniently uses an *expect()* method that utilises specific [matchers](https://github.com/pivotal/jasmine/wiki/Matchers) to test the code logic e.g.

    expect(1).toBe(1);

**Note**

Test classes are usually created automatically if using the [Angular generator](https://github.com/yeoman/generator-angular) for Yeoman.

## Controllers

Initialize the controller with a mock scope in a *beforeEach()* method, note injection of dummy data:

    var MainCtrl, scope;
    beforeEach(inject(function($controller, $rootScope) {
        // Create a new scope
        scope = $rootScope.$new();
        // Dummy data
        scope.foo = [{
          'id': '1',
          'type': 'string'
        }];
        // Store the controller instance in a class variable
        // injecting dummy scope
        MainCtrl = $controller('MainCtrl', {
          $scope: scope
        });
    }));

Example expectation:

    it('first item in foo array should have a type of string', function() {
        expect(scope.first.type).toBe('string');
    });

## Directives

### Element directive

Use a *beforeEach()* method to create a new instance of the template used by the directive (defined by templateURL):

    beforeEach(module('views/templates/templateOne.html'));

Use a *beforeEach()* method to store a new element based on code as used in the application to instantiate the directive and store in a class variable:

    beforeEach(inject(function($rootScope, $compile) {
        // we might move this tpl into an html file as well...
        elm = angular.element('<div class="hero-unit"><foobar ng-repeat="item in items" id="{{item.id}}" type="{{item.type}}""></foobar></div>');
        scope = $rootScope;
        $compile(elm)(scope);
        scope.$digest();
    }));

Use jQuery style syntax to target nodes etc within the compiled directive and run expectations:

    it("should display the number of items in the items list", function() {
        var list = elm.find('li');
        expect(list.length).toBe(2);
    });

### Attribute directive

If using a service/model within the directive, inject a service in a *beforeEach()* method to test attributed data:

    var elm, scope, testFooService;
    beforeEach(inject(function($rootScope, $compile, fooService) {
        testFooService = fooService;

Example expectation:

    it("should assign the correct item type", function() {
        var v = elm.find('item')[0];
        expect(v.getAttribute('type')).toBe('string');
    });
    
### Testing for elements

Use: `expect(element.find('ul').html()).toBeDefined();` and not: `expect(element.find('ul')).toBeTruthy();`. The latter will always be **true** due to `element.find()` returns `{}` if there is no element.

## Filters

Use a *beforeEach()* method to create a new instance of the filter to test:

    // initialize a new instance of the filter before each test
    var timeFilter;
    beforeEach(inject(function($filter) {
        timeFilter = $filter('timeFilter');
    }));

Pass a value into the filter with the expectation of the result:

    it('should return a number in a time format', function() {
        expect(timeFilter(0)).toBe('00:00');
    });

## Services

Use a *beforeEach()* method to inject and assign the service class to a class variable:

    // instantiate service
    var rootScope, testVideoService;

    beforeEach(inject(function($rootScope, $compile, fooService) {
        rootScope = $rootScope;
        testFooService = fooService;
    }));

Example testing the service exists:

    it('should have a service', function() {
        expect(testFooService).toNotEqual(null);
    });

## Method invocation (Spy)

1. Using the object that methods will be tested for invocation (optionally also testing data passed), create the spy [Object], [Method]:
    
    `spyOn(fooService, 'setFooElement').andCallThrough();`

2. Use one of the spy methods e.g. *toHaveBeenCalledWith()* to an expectation:

        it("should have called setFooElement on the fooService with the correct foo object", function() {
            var f = elm.find('foo')[0];
            expect(testFooService.setFooElement).toHaveBeenCalledWith(f);
        });

## Broadcasted events (Spy)

1. Create a spy mock without using an object i.e. 'listener'
2. Add an event handler for the broadcasted event
3. Run the functionality on the class that will trigger the broadcast
4. Run an expectation using the spy method e.g *toHaveBeenCalled()*

        it('should broadcast somethingUpdated event', function() {
            var somethingUpdatedListener = jasmine.createSpy('listener');
            rootScope.$on('somethingUpdated', somethingUpdatedListener);
            testFooService.something(20);
            expect(somethingUpdatedListener).toHaveBeenCalled();
        });

## DOM events

1. Create a new event
2. Initialize the event with the event name
3. Dispatch the event using the associated object.

        it('should broadcast some event', function() {
            var f = elm.find('foo')[0];
            var evt = document.createEvent('Event');
            evt.initEvent('some', true, true);
            f.dispatchEvent(evt);
            expect(f.something).toBe(14);
        });

## Mocks

### HTTP requests

Inject Angular's own fake HTTP service [$httpBackend](http://docs.angularjs.org/api/ngMock.$httpBackend) into a *beforeEach()* method and assign to a class variable:

    beforeEach(inject(function($controller, $rootScope, _$httpBackend_) {
        $httpBackend = _$httpBackend_;
    }
    
Mock the HTTP request (GET, POST etc) and add your own response data:

    $httpBackend.when('GET', 'some.json').respond({
      "template": "views/templateOne.html"
    });
    
Ensure there are no outstanding requests in an *afterEach()* method:

    afterEach(function() {
        $httpBackend.verifyNoOutstandingExpectation();
        $httpBackend.verifyNoOutstandingRequest();
    });
    
Flush following a test that utilises the fake service:

    it('should attach a list of items to the scope', function() {
        expect(scope.items.length).toBe(3);
        $httpBackend.flush();
      });

### Further example

Angular provides it's own built in mocks. However, in certain situations there maybe a need to create your own. This [article](http://southdesign.de/blog/mock-angular-js-modules-for-test-di.html) excellently explains how to do this.

## Handy Hints

* Use `console.log(angular.mock.dump(element));` to view the element html content as a string.

---

### Further reading

* Unit testing with [Jasmine](http://www.joezimjs.com/javascript/javascript-unit-testing-with-jasmine-part-2/)
* Jasmine spy [cheatsheet](http://tobyho.com/2011/12/15/jasmine-spy-cheatsheet/)
* Old but good [full-spectrum testing](http://www.yearofmoo.com/2013/01/full-spectrum-testing-with-angularjs-and-testacular.html)
* Vojtajina directive testing [example](https://github.com/vojtajina/ng-directive-testing)
* Good examples of component based testing in this [repos](https://github.com/angular-ui/bootstrap/blob/master/src/)
* [Matchers](https://github.com/pivotal/jasmine/wiki/Matchers)
* [Spies](https://github.com/pivotal/jasmine/wiki/Spies)
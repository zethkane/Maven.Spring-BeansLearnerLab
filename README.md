# Bean Flavored Learner Lab
* **Objective** - to implement a `ZipCodeWilmington` class which _mediates_ a _composite_ `Students` and `Instructors` _singleton_ reference.
* **Purpose** - to demonstrate the use of
	* Bean registration
	* Dependency Injection
	* IOC Container
	* `AnnotationConfigApplicationContext`
	* Annotations
		* `@Bean`
		* `@DependsOn`
		* `@Autowired`
		* `@PostConstruct`
		* `@Config`
		* `@SpringBootTest`
		* `@Resource`


## Developmental Notes
* You may structure this project and the packaging how you please, however keep in mind that `@Configuration` scans from current directory down.
* This project is nearly identical to the `LearnerLab` completed in the past until `Part 9`

### Part 0.0 - Generating Project
* Navigate to [start.spring.io](start.spring.io)
* Search for `Web` in the `Search for Dependencies` input box
* Select `Generate Project`
* After the project has completed downloading, navigate to the download directory and unzip the project folder.
* After unzipping the project folder, open the project via its `pom.xml` from IntelliJ > File > Open
	* Be sure to `Open as Project` when prompted 


### Part 1.0 - Create `Person` Class
* Create a `Person` class.
	* The class should declare a `final` field named `id` of type `long`.
	* The class should declare a field named `name` of type `String`.	
	* `Person` constructor should have a parameter of type `long` which sets the `id` field to the respective value.
	* The class should define a `getId()` method which returns the `Person` object's `id` field.
	* The class should define a `getName()` method which returns the `Person` object's `name` field.
	* The class should define a `setName()` method which sets the `Person` object's `name` field.

-
### Part 2.0 - Create `Learner` Interface
* Create a `Learner` interface.
	* `Learner` should declare one method signature:
		* Method name: `learn`
		* Method parameters: `double numberOfHours`
		* Method return-type: `void`

-
### Part 3.0 - Create `Student` Class
* Create a `Student` class such that:
	* `Student` is a subclass of `Person`
	* `Student` implements the `Learner` interface
	* `Student` should have an instance variable `totalStudyTime` of type `double`
	* `Student` should have a concrete implementation of the `learn` method which increments the `totalStudyTime` variable by the specified `numberOfHours` argument.
	* `Student` should have a `getTotalStudyTime()` method which returns the `totalStudyTime` instance variable.


-
### Part 4.0 - Create `Teacher` Interface
* Create a `Teacher` interface.
	* `Teacher` should declare a `teach` method signature:
		* Method name: `teach`
		* Method parameters:
			* `Learner learner`
			* `double numberOfHours`
		* Method return-type: `void` 

	* `Teacher` should declare a `lecture` method signature:
		* Method name: `lecture`
		* Method parameters:
			* `Learner[] learners`
			* `double numberOfHours`
		* Method return-type: `void`

		
-
### Part 5.0 - Create `Instructor` Class
* Create an `Instructor` class such that:
	* `Instructor` is a subclass of `Person`
	* `Instructor` implements the `Teacher` interface
	* `Instructor` should have a concrete implementation of the `teach` method which invokes the `learn` method on the specified `Learner` object.
	* `Instructor` should have a concrete implementation of the `lecture` method which invokes the `learn` method on each of the elements in the specified array of `Learner` objects.
		* `numberOfHours` should be evenly split amongst the learners.
			* `double numberOfHoursPerLearner = numberOfHours / learners.length;`

-
### Part 6.0 - Create `Students` 
* Create a `Students` class.
	* The class should be an _unextendable_ subclass of the `People` class.
	* The class should consume a variable number of `Student` objects upon construction and pass them to the super constructor.

-
### Part 7.0 - Create `Instructors` 
* Create a `Instructors` class.
	* The class should be an _unextendable_ subclass of the `People` class.
	* The class should consume a variable number of `Instructor` objects upon construction and pass them to the super constructor.



-
### Part 8.0 - Create `Classroom`
* Create a `Classroom` class.
	* The class should consume and set composite reference to an `Instructors` and `Students` object upon construction
	* The class should define a method `hostLecture` which makes use of a `Teacher teacher, double numberOfHours` parameter to host a `lecture` to the composite `personList` field in the `students` reference.
	

-
## Part 9.0 - Creating `Configuration` classes
* Each of the following `Config` classes should have a class-signature annotation of `@Configuration`
	* this annotation tells spring to scan for `@Bean` definitions within the scope of the class, and register them to the [IOC Container](https://www.tutorialspoint.com/spring/spring_ioc_containers.htm) for `Inject` and `Autowire` use later.

-
### Part 9.1 - Create `StudentConfig`
* **Note:** The creation of this class will demonstrate an implementation of bean registration in Spring.
* The class should define a method named `currentStudents()` which returns a `Students` representative of the current cohort of students.
	* the method should be annotated with `@Bean(name = "students")`
		* this ensures the Spring container registers the bean with the respective name.
		* a `@Bean` whose `name` attribute is not specified defaults to the name of the method it is annotating.
* The class should define a bean named `previousStudents()` which returns a `Students` representative of the previous cohort of students.	

-
### Part 9.2 - Create `InstructorsConfig`
* The class should define a bean named `tcUsaInstructors()` which returns an `Instructors` representative of the Tech Connect USA instructors.
* The class should define a bean named `tcUkInstructors()` which returns an `Instructors` representative of the Tech Connect UK instructors.
* The class should define a bean named `allInstructors` which returns all `Instructors` employed at ZipCodeWilmington



-
### Part 9.3 - Create `ClassroomConfig`
* The class should define a bean named `currentCohort()` which returns an `Classroom` object whose dependencies are `allInstructors` and `students`
* The class should define a bean named `previousCohort()` which returns an `Classroom` object whose dependencies are `allInstructors` and `previousStudents`
* **Note:** [it is sometimes useful](https://www.boraji.com/spring-dependson-annotation-example) (although not always necessary) to use the `@DependsOn` annotation to help the compiler and other readers of the code to understand what order beans should be executed.
	* `@DependsOn({"allInstructors", "students"})`

	


-
## Part 10 - Test `Config` classes
* Each of the following `Test` classes should be annotated with
	* `@SpringBootTest`
		* indicates that this class is a Spring Boot test class
		* provides support to scan for a `ContextConfiguration` that tells the test class how to load the `ApplicationContext`.
		* If no `ContextConfiguration` classes are specified as a parameter to the `@SpringBootTest` annotation, the default behavior is to load the `ApplicationContext` by scanning for a `@SpringBootConfiguration` annotation on a class in the package root.
* Each bean can be injected into the class scope using `@Autowired` along with `@Resource(name = "beanname")`



-
### Part 10.1 - Test `StudentConfig` Class
* Create a `TestStudentConfig` class in the `test` package.
* The class should ensure that each `Bean` in the `StudentConfig` class is configured as expected.
* **Tip:** You can use the `toString` method to get a representation of the aggregate state of any `People` object.


-
### Part 10.2 - Test `InstructorConfig` Class
* Create a `TestInstructorConfig` class in the `test` package.
* The class should ensure that each `Bean` in the `TestInstructorConfig` class is configured as expected.


-
### Part 10.3 - Test `ClassroomConfig` Class
* Create a `TestClassroomConfig` class in the `test` package.
* The class should ensure that each `Bean` in the `TestClassroomConfig` class is configured as expected.

## Part 11 - Using `@Component`
* Annotating a class signature class with `@Component` allows Spring to register this class as a `Bean` implicitly.
-
### Part 11 - Create `Alumni` Class
* Create an `Alumni` component which autowires `Students` of the previous and `Instructors`
* Create an `executeBootcamp` method which teaches each `Student` in the composite `Students` a `totalNumberOfHours` of `1200`.
	* Annotate this method with `@PostConstruct`
		* denotes that this method must be executed before the class is put into an IoC container

-
### Part 11.1 - Test `Alumni` Class
* Write a test class which ensures that each `Student` in the `Alumni` class has been taught `1200` hours upon injection of the `Alumni` dependency.

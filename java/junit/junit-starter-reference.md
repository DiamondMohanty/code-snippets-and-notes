# Project Structure To Avoid Build Path Issues
Maven uses project structure conventions to detect classes in build path. Having a proper project structure ensures the classes are available to the Test classes. The following project structure is recommended.

<pre>
/src
    /main
        /java
            -classes
        /resources
            -applications.properties
    /test
        /java
            -classes
        /resources
            -application.properties
    
</pre>

# Testing Repository Layer
1. Autowire the repository to test.
2. Declare model objects as required.
3. Provide @BeforeEach and @AfterEach methods.
4. Write the test methods.

## Following are some important APIs

``` java
    // Use to assert something
    assertThat(condition)
```

# Testing Service Layer
1. Mock the required repository classes.
2. Declare autocloseable instance.
3. Declare model objects as required.
4. Provide @BeforeEach and @AfterEach methods.
5. Instantiate the required service to test my passing the required repository instances already mocked.
6. Write the test methods.

## Following are some important APIs

``` java
    // Setup autocloseable
    AutoCloseable autocloseable = MockitoAnnotations.openMocks(this);

    // Mock Repository
    // 1. Mark the repository to mock
    @Mock
    SampleRepository sampleRespository;

    // 2. Mock it
    mock(SampleRepository.class);

    //3. Provide mock implementation
    // When this method execute is specifies that any time a service calls the "testMethod" it would be returned with a known value "testValue"
    when(sampleRepository.testMethod()).thenReturn(testValue);
    
    // Make Service call
    // Provided that sampleService.testMethod() callls sampleRepository.testMethod() internally to return a result.
    assertThat(sampleService.testMethod()).isEqualTo(testValue);

    // To test repository methods which returns void
    // 1. Mock repository class in a different way
    mock(sampleRepository.class, Mockito.CALLS_REAL_METHODS);

    // 2. Setting mock implementation
    doAnswer(Answers.CALLS_READ_METHODS).when(sampleRepository).methodWithVoidReturn();

    // 3. Make assertions
    assertThat(sampleService.testMethod()).isEqualTo(expectedReturnFromServiceMethod);
```

# Testing Controller Layer
1. Annotate the controller class a @WebMvcTest(SampleController.class), where SampleController.class is the controller class to test.
2. @Autowire MockMvc.
3. Mock the service with @MockBean.
4. Declare the required models.
5. Implement the test methods

## Following are some important APIs

```java
    // Mock the URL
    // Using MockMvc perform a get on the URL and if the URL return result properly mark the status as OK
    // A mock URL is created and the actual URL is not used.
    mockMvc.perform(MockMvcRequestBuilders.get("url-to-test")).andDo(MockMvcRequestBuilders.print()).andExpect(MockMvcRequestBuilders.status().isOk);

    // Rest Methods
    // 1. MockMvcRequestBuilders.get()
    // 2. MockMvcRequestBuilders.delete()
    // 3. MockMvcRequestBuilders.post()
    // Use serializer to convert Object to requestJSON
    this.mockMvc.peform(MockMvcRequestBuilders.post("url").contentType(MediaType.APPLICATION_JSON).content(requestJSON));

    // 4. MockMvcRequestBuilders.put()
    this.mockMvc.peform(MockMvcRequestBuilders.put("url").contentType(MediaType.APPLICATION_JSON).content(requestJSON));
    
```

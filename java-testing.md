# Java Testing 

<a name="index"></a> 
     
## Table of contents
1. [Monitoring](#jvisualvm)
2. [Use InjectMocks](#junit4-inject-mocks) 
3. [PowerMock : Standard mocking](#powermock-standard-mocking)                                               
4. [PowerMock : Mocking multiple values](#powermock-multiple-values-mocking)                                               
5. [PowerMock : Partial mocking](#powermock-partial-mocking)                                                 
6. [PowerMock : Mocking a static method](#powermock-static-method-mock)                                      
7. [PowerMock : Mocking a private method](#powermock-private-method-mock)
8. [PowerMock : Testing a private method](#powermock-call-private-method) 
9. [PowerMock : Mocking a final class](#powermock-final-class-mock)                                      
10. [PowerMock : Don't call a method with doNothing](#powermock-do-nothing)
11. [PowerMock : Force an exception on a mock with doThrow](#powermock-do-throw)
12. [PowerMock : Use stub to mock any public method (static or not)](#powermock-stub)
13. [Verify that an object has been called](#verify)
14. [Verify the arguments passed to an object](#argument-captor)
15. [Using a context xml file to initialize a constructor](#springboot-context-xml)
16. [Add customized error when assertion fails](#assert-custom-error)     
17. [Launch ordered tests and stop on first fail (junit 4)](#order-and-stop)
18. [JUnit 5 & ReflectionUtils : Standard mocking](#junit5-standard-mocking)
19. [JUnit 5 & ReflectionUtils : Testing a private method](#junit5-call-private-method)
20. [JUnit 5 : Mocking a static method](#junit5-mock-static)
21. [JUnit 5 : Mocking H2 current date](#junit5-mock-h2)
22. [JUnit 5 : Mockito and InjectMocks](#junit5-mockito)
23. [JUnit 5 : Using temp files](#junit5-temp-files)
24. [Spring Boot : set pom.xml](#springboot-pom)           
25. [Spring Boot : testing a rest controller](#springboot-rest-controller)           
26. [Spring Boot : testing a spring mvc controller](#springboot-mvc-controller)
27. [Spring Boot : testing a spring data datasource](#springboot-spring-data)
28. [Spring Boot : only allow some classes for a test](#springboot-allowed-classes)         
29. [Spring Boot : use test resources with a profile](#springboot-test-ressources)
30. [Spring Boot : test a MultipartFile](#springboot-test-multipartfile)
31. [Spring Boot : disable security in a test](#springboot-disable-security)    
32. [Spring Boot : external calls to rest services](#springboot-rest-external-calls)
33. [Spring Boot : testing a SOAP web service](#springboot-soap)
34. [Maven : Test a local library](#maven-test-library)

<a name="jvisualvm"></a>
### Using the monitoring

To launch the tool, open a terminal :
```bash
cd $JAVA_HOME/bin
./jvisualvm
```

<a href="#index">back to index</a>

<a name="junit4-inject-mocks"></a>
### Use InjectMocks

```java
@RunWith(MockitoJUnitRunner.class)
public class InsertMockTest {

    @Mock
    private FileReader fileReader;

    @InjectMocks
    private DummyController dummyController;

    @Test
    public void checkMockWithDoReturn() throws Exception {
        doReturn("no content in the file").when(fileReader).read();
        assertEquals("no content in the file", dummyController.getFileContent());
    }

    @Test
    public void checkMockWithThenReturn() throws Exception {
        when(fileReader.read()).thenReturn("no content in the file");
        assertEquals("no content in the file", dummyController.getFileContent());
    }
}
```
<a href="#index">back to index</a>

<a name="powermock-standard-mocking"></a>
### PowerMock : Standard mocking

```java
@RunWith(PowerMockRunner.class)
public class BasicMockTest {

    @Mock
    private FileReader fileReaderMock;

    private DummyController dummyController;

    @Before
    public void setup() {
        dummyController = new DummyController();
        Whitebox.setInternalState(dummyController, "fileReader", fileReaderMock);
    }

    @Test
    public void simpleMockWithDoReturn() throws Exception {
        doReturn("no content in the file").when(fileReaderMock).read();
        assertEquals("no content in the file", dummyController.getFileContent());
    }

    @Test
    public void simpleMockWithThenReturn() throws Exception {
        when(fileReaderMock.read()).thenReturn("no content in the file");
        assertEquals("no content in the file", dummyController.getFileContent());
    }
}
```

<a href="#index">back to index</a>


<a name="powermock-multiple-values-mocking"></a>
### PowerMock : Mocking multiple values

```java
@RunWith(PowerMockRunner.class)
public class MockMultipleValues {

    private DummyController dummyController;

    @Before
    public void setup() {
        dummyController = PowerMockito.spy(new DummyController());
    }

    @Test
    public void mockWithMultipleValues() {
        // default value (always the same)
        assertEquals("black", dummyController.getColor());

        // mocked values
        doReturn("red","green","blue").when(dummyController).getColor();
        assertEquals("red", dummyController.getColor());
        assertEquals("green", dummyController.getColor());
        assertEquals("blue", dummyController.getColor());
        assertEquals("blue", dummyController.getColor());
    }
}
```

<a href="#index">back to index</a>


<a name="powermock-partial-mocking"></a>
### PowerMock : Partial mocking

```java
@RunWith(PowerMockRunner.class)
public class PartialMock {

    private DummyController dummyController;

    @Before
    public void setup() {
        dummyController = PowerMockito.spy(new DummyController());
        // only one method will be mocked
        Mockito.when(dummyController.getTimeInfo()).thenReturn("no idea what time it is");
    }

    @Test
    public void getMessage() {
        // this method is standard, no mock
        assertEquals("hello world", dummyController.getMessage());
        Mockito.verify(dummyController, Mockito.times(1)).getMessage();
        Mockito.verify(dummyController, Mockito.times(1)).logEvent(anyString());
    }

    @Test
    public void getTimeInfo() {
        // this method was mocked
        assertEquals("no idea what time it is", dummyController.getTimeInfo());
        Mockito.verify(dummyController, Mockito.times(1)).getTimeInfo();
    }
}
```

<a href="#index">back to index</a>


<a name="powermock-static-method-mock"></a>
### PowerMock : Mocking a static method

```java
@RunWith(PowerMockRunner.class)
@PrepareForTest(TimeUtil.class)
@PowerMockIgnore("javax.management.*")
public class StaticMethodMockTest {

    private DummyController dummyController;

    @Before
    public void setup() {
        PowerMockito.mockStatic(TimeUtil.class);
        Mockito.when(TimeUtil.getSdfCurrentDateTime()).thenReturn("01/01/2010 10:00");
        dummyController = new DummyController();
    }

    @Test
    public void mockAstaticMethod() {
        assertEquals("this is current date and time : 01/01/2010 10:00", dummyController.getTimeInfo());
    }
}
```

<a href="#index">back to index</a>


<a name="powermock-private-method-mock"></a>
### PowerMock : Mocking a private method

```java
@RunWith(PowerMockRunner.class)
@PrepareForTest(FooController.class)
public class MockPrivateMethod {

    private FooController fooController;

    @Before
    public void setUp() {
        MockitoAnnotations.initMocks(this);
        fooController = spy(new FooController());
    }

    @Test
    public void basic() {
        String dummy = fooController.getSomethingFromPublicMethod();
        assertEquals("the value is 'foo bar'", dummy);
    }

    @Test
    public void mockPrivate() throws Exception {
        doReturn("anything").when(fooController, "getSomethingFromPrivateMethod", ArgumentMatchers.anyString());
        String dummy = fooController.getSomethingFromPublicMethod();
        assertEquals("the value is 'anything'", dummy);
    }
}
```

<a href="#index">back to index</a>


<a name="powermock-call-private-method"></a>
### PowerMock : Testing a private method

```java
import org.powermock.reflect.Whitebox;

public class PrivateMethodTest {

    private DummyController dummyController = new DummyController();

    @Test
    public void callPrivateMethod() throws Exception {
        String result = Whitebox.invokeMethod(dummyController, "getPrivateMessage");
        assertEquals("winter is coming", result);
    }
}
```

<a href="#index">back to index</a>


<a name="powermock-final-class-mock"></a>
### PowerMock : Mocking a final class

If mocking a final class fails, add the following dependency to your pom.xml :

```xml
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-inline</artifactId>
    <version>some version number</version>
    <scope>test</scope>
</dependency>
```

<a href="#index">back to index</a>


<a name="powermock-do-nothing"></a>
### PowerMock : Don't call a method with doNothing

This method can also be used on fully mocked objects.
```java
@RunWith(PowerMockRunner.class)
public class DoNothingTest {

    private DummyController dummyController;

    @Before
    public void setup() {
        dummyController = PowerMockito.spy(new DummyController());
    }

    @Test
    public void getMessageWithoutLog() {
        // avoid calling logEvent() from getMessage()
        doNothing().when(dummyController).logEvent(anyString());
        assertEquals("hello world", dummyController.getMessage());
        Mockito.verify(dummyController, Mockito.times(1)).getMessage();
    }

}
```

<a href="#index">back to index</a>

<a name="powermock-do-throw"></a>
### PowerMock : Force an exception on a mock with doThrow

```java
@RunWith(PowerMockRunner.class)
public class DoThrowTest {

    private DummyController dummyController;

    @Mock
    private MathService mathServiceMock;

    @Before
    public void setup() {
        dummyController = new DummyController();
        Whitebox.setInternalState(dummyController, "mathService", mathServiceMock);
    }

    @Test
    public void addTwoStringWithError() {
        doThrow(new NumberFormatException("cannot convert this string to integer")).when(mathServiceMock).add("twenty", "thirty");
        try {
            Integer result = dummyController.add("twenty", "thirty");
            fail("it cannot work");
        } catch(NumberFormatException numEx) {
            assertTrue(numEx.getMessage().contains("cannot convert this string to integer"));
        }
    }
}
```

<a href="#index">back to index</a>


<a name="powermock-stub"></a>
### PowerMock : Use stub to mock any public method (static or not)

The usage of stub method can be usefull when it's difficult to mock the controller containing this method.

```java
@RunWith(PowerMockRunner.class)
@PrepareForTest(DummyController.class)
public class NonStaticWithStubTest {

    private DummyController dummyController;

    @Before
    public void setup() {
        PowerMockito.stub(PowerMockito.method(DummyController.class, "getMessage")).toReturn("it is raining");
        dummyController = new DummyController();
    }

    @Test
    public void mockAstaticMethod() {
        assertEquals("it is raining", dummyController.getMessage());
        assertNotEquals("hello world", dummyController.getMessage());
    }
}
```
<a href="#index">back to index</a>


<a name="verify"></a>
### Verify that an object has been called

You can easily check that an object has been called or not, mentioning expected parameters or generic ones.

```java
public class DummyFactoryTest {

    private DummyFactory dummyFactory;

    @Mock
    private ResultLayout resultLayout;

    @Before
    public void setUp() {
        MockitoAnnotations.initMocks(this);
        dummyFactory = new DummyFactory(resultLayout);
    }

    @Test
    public void runWithOutputString() {
        Map<String, String> param = new HashMap<>;
        param.put("output", "string");
        dummyFactory.run(param);
        verify(resultLayout).display("hello world");
        verify(resultLayout, times(1)).display("hello world");
    }

    @Test
    public void runWithOutputTime() {
        Map<String, String> param = new HashMap<>;
        param.put("output", "time");
        dummyFactory.run(param);
        verify(resultLayout).display(anyString());
    }

    @Test
    public void runWithOutputUndefined() {
        Map<String, String> param = new HashMap<>;
        param.put("output", "foo");
        dummyFactory.run(param);
        verifyNoInteractions(resultLayout);
    }
}
```

<a href="#index">back to index</a>

<a name="argument-captor"></a>
### Verify the arguments passed to an object

In this example, DummyController is calling FileReader. ArgumentCaptor instruction will be used to validate which arguments are passed to FileReader.
```java
public class ArgumentCaptorTest {

    private DummyController dummyController;

    @Mock
    private FileReader fileReader;

    @Before
    public void setup() {
        MockitoAnnotations.initMocks(this);
        dummyController = new DummyController();
        Whitebox.setInternalState(dummyController, "fileReader", fileReader);
    }

    @Test
    public void getFileContent() throws IOException {
        doReturn("{\"foo\":\"bar\"}").when(fileReader).read(anyString());
        String result = dummyController.getFileContent("./tmp/foo.json");

        ArgumentCaptor<String> resultCaptor = ArgumentCaptor.forClass(String.class);
        verify(fileReader).read(resultCaptor.capture());
        assertEquals("./tmp/foo.json", resultCaptor.getValue());

        assertNotNull(result);
        assertEquals("{\"foo\":\"bar\"}", result);
    }
}
```

<a href="#index">back to index</a>

<a name="springboot-context-xml"></a>
### Using a context xml file to initialize a constructor

- **a configuration (or any other controller)**
```java
@Component
public class FileReader {

    private final String DEFAULT_PATH = "./tmp/foo.txt";
    private String filePath;

    public FileReader() {
        this.filePath = DEFAULT_PATH;
    }

    public FileReader(String filePath) {
        this.filePath = filePath;
    }

    public String read() throws Exception {
        return FileUtil.getFileContent(filePath);
    }
}
```

- **the xml context**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="eu.edu.dummy"/>

    <bean id="msgBean" class="java.lang.String">
        <constructor-arg value="hello world" />
    </bean>

    <bean id="defaultFileReader" class="eu.edu.dummy.FileReader" />

    <bean id="fooFileReader" class="eu.edu.dummy.FileReader">
        <constructor-arg name="filePath" value="src/test/resources/foo.txt"/>
    </bean>

    <bean id="barFileReader" class="eu.edu.dummy.FileReader">
        <constructor-arg index="0" value="src/test/resources/bar.txt"/>
    </bean>
</beans>
```

- **the test class**
```java
@RunWith(SpringRunner.class)
@SpringBootTest
@ContextConfiguration(locations= "classpath:spring-ctx-constructor.xml")
public class ContextAndConstructor {

    @Autowired
    private String msgBean;

    @Autowired
    private FileReader defaultFileReader;

    @Autowired
    private FileReader fooFileReader;

    @Autowired
    private FileReader barFileReader;

    @BeforeClass
    public static void setup() throws Exception {
        FileUtil.createDirectory("./tmp");
        assertTrue( FileUtil.doesPathExist("./tmp") );
        FileUtil.append("./tmp/foo.txt", "this is foo from defined path");
        assertTrue( FileUtil.doesPathExist("./tmp/foo.txt") );
    }

    @AfterClass
    public static void cleansing() throws Exception {
        FileUtil.clearDirectory("./tmp");
    }

    @Test
    public void checkingBeansWithConstructors() throws Exception {
        assertEquals("hello world", msgBean);
        assertEquals("this is foo from defined path", defaultFileReader.read());
        assertEquals("this is foo from resources folder", fooFileReader.read());
        assertEquals("this is bar from resources folder", barFileReader.read());
    }
}
```

<a href="#index">back to index</a>

<a name="assert-custom-error"></a>
### Add customized error when assertion fails

When launching all your tests with Maven, for example, it may save time to understand why an assertion has failed.   
You can do that simply, passing a message as first argument of any assertion instruction :
```
assertNotNull("result is null", result);
```

Then, when running the test sequence, the message will be displayed.  
Now you know which instruction failed before starting any investigation.

```
Failed tests:   testMethodName(edu.sandbox.BasicTest): result is null
```

<a href="#index">back to index</a>

<a name="order-and-stop"></a>
### Launch ordered tests and stop on first fail (junit 4)

Using junit 4, you may need to launch your tests in a certain order. You can achieve that using ```MethodSorters.NAME_ASCENDING```, and name your test methods accordingly.
If you also need to stop testing a class as soon as an error is met, you can proceed the following way :

```java
@FixMethodOrder(MethodSorters.NAME_ASCENDING)
public class OrderAndStopTest {

    private static final Logger LOG = LoggerFactory.getLogger(OrderAndStopTest.class);
    private static boolean shouldStopTesting = false;

    @Before
    public void beforeEachTest() {
        assertFalse("Testing has been aborted.", shouldStopTesting);
    }

    @Test
    public void bTest() {
        try {
            LOG.debug("test B");
            assertEquals("unexpected result", "foo","bar");
        } catch(Throwable th) {
            shouldStopTesting = true;
            throw th;
        }
    }

    @Test
    public void cTest() {
        try {
            LOG.debug("test C");
            assertTrue(true);
        } catch(Throwable th) {
            shouldStopTesting = true;
            throw th;
        }
    }

    @Test
    public void aTest() {
        try {
            LOG.debug("test A");
            assertTrue(true);
        } catch(Throwable th) {
            shouldStopTesting = true;
            throw th;
        }
    }
}
```

result :   
aTest is processed without errors  
bTest is processed and fails  
cTest is skipped

maven result :
```text
Failed tests:   bTest(edu.sandbox.OrderAndStopTest): unexpected result expected:<[foo]> but was:<[bar]>
  cTest(edu.sandbox.OrderAndStopTest): Testing has been aborted.
```

<a href="#index">back to index</a>


<a name="junit5-standard-mocking"></a>
### JUnit 5 & ReflectionUtils : Standard mocking

With this test, the service is initialised, in the controller, with `@Autowired` tag.

```java
public class DummyControllerTest {

    private DummyController dummyController;

    @Mock
    private DummyService dummyService;

    @BeforeEach
    public void setup() {
        MockitoAnnotations.initMocks(this);
        dummyController = new DummyController();
        ReflectionTestUtils.setField(dummyController, "dummyService", dummyService);
    }

    @Test
    public void getSeason() {
        doReturn("winter").when(dummyService).getSeason();
        assertEquals("winter", dummyController.getSeason());
    }
}
```

<a href="#index">back to index</a>


<a name="junit5-call-private-method"></a>
### JUnit 5 & ReflectionUtils : Testing a private method

```java
import org.springframework.util.ReflectionUtils;
import java.lang.reflect.Method;

public class HtmlCleanerTest {

    private HtmlCleaner htmlCleaner = new HtmlCleaner();
    
    @Test
    public void removeBasicTags() throws Exception {
        String html = "<b>hello</b> <i>world</i>";
        Method method = ReflectionUtils.findMethod(htmlCleaner.getClass(), "removeBasicTags", String.class);
        method.setAccessible(true);
        String result = method.invoke(htmlCleaner, html).toString();
        assertEquals("hello world", result);
    }
}
```

<a href="#index">back to index</a>


<a name="junit5-mock-static"></a>
### JUnit 5 : Mocking a static method

```java
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.MockedStatic;
import org.mockito.Mockito;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit.jupiter.SpringExtension;

import java.time.Instant;
import java.time.LocalDateTime;
import java.time.Month;
import java.time.ZoneId;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.mockito.ArgumentMatchers.anyLong;

@ExtendWith(SpringExtension.class)
@ContextConfiguration(classes = {SandboxUtil.class})
public class MockStaticInstantTest {
    private MockedStatic<Instant> mockInstant;
    private MockedStatic<SandboxUtil> mockSandboxUtil;

    @BeforeEach
    public void setup() {
        this.mockInstant = Mockito.mockStatic(Instant.class, Mockito.CALLS_REAL_METHODS);
        LocalDateTime localDateTime = LocalDateTime.of(2022, Month.MAY, 1, 11, 15);
        Instant instant = localDateTime.atZone(ZoneId.systemDefault()).toInstant();
        mockInstant.when(Instant::now).thenReturn(instant);

        this.mockSandboxUtil = Mockito.mockStatic(SandboxUtil.class, Mockito.CALLS_REAL_METHODS);
        mockSandboxUtil.when(() -> SandboxUtil.getLabel(anyLong())).thenReturn("foobar");
    }

    @AfterEach
    public void tearDown() {
        mockInstant.close();
        mockSandboxUtil.close();
    }

    @Test
    public void checkInstant() {
        Instant now = Instant.now();
        assertNotNull(now);

        ZoneId zoneId = ZoneId.systemDefault(); // Use the system default time zone
        LocalDateTime localDateTime = now.atZone(zoneId).toLocalDateTime();
        assertEquals(2022, localDateTime.getYear());
        assertEquals(5, localDateTime.getMonthValue());
        assertEquals(1, localDateTime.getDayOfMonth());
    }

    @Test
    public void checkUtilityClass() {
        String result = SandboxUtil.getLabel(8);
        assertEquals("foobar", result);
    }
}
```

<a href="#index">back to index</a>


<a name="junit5-mock-h2"></a>
### JUnit 5 : Mocking H2 current date

To mock `current date` from native sql queries, you have to mock some static methods of `DateTimeUtils`.

```java
import org.h2.util.DateTimeUtils;
import org.h2.util.TimeZoneProvider;
import org.h2.value.ValueTimestampTimeZone;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.MockedStatic;
import org.mockito.Mockito;

import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.ArgumentMatchers.*;
import static org.mockito.ArgumentMatchers.any;

@ExtendWith(SpringExtension.class)
@ContextConfiguration(classes = {SomethingRepository.class})
@DataJpaTest
@EnableJpaRepositories(basePackages = {"edu.repository"})
@EntityScan(basePackages = "edu.entity")
@ActiveProfiles("test")
@SqlGroup({@Sql(executionPhase = Sql.ExecutionPhase.BEFORE_TEST_METHOD, scripts = "classpath:h2-sample.sql")})
@Slf4j
public class SomethingRepositoryTest {

    @Autowired
    private SomethingRepository repository;

    private LocalDate currentDate;
    private Instant h2CurrentInstant;
    private MockedStatic<DateTimeUtils> mockDateTimeUtils;

    @BeforeEach
    public void setup() {
        // set the date for tests
        LocalDateTime h2DateTime = LocalDateTime.of(2024, Month.MAY, 5, 11, 15);
        this.currentDate = h2DateTime.toLocalDate();
        ValueTimestampTimeZone h2TsTimeZone = ValueTimestampTimeZone.fromDateValueAndNanos(
                DateTimeUtils.dateValue(h2DateTime.getYear(), h2DateTime.getMonthValue(), h2DateTime.getDayOfMonth()),
                h2DateTime.toLocalTime().toNanoOfDay(), 0);
        this.h2CurrentInstant = h2DateTime.atZone(ZoneId.systemDefault()).toInstant();

        // force current date and time from H2 sql functions
        this.mockDateTimeUtils = Mockito.mockStatic(DateTimeUtils.class, Mockito.CALLS_REAL_METHODS);
        mockDateTimeUtils.when(() -> DateTimeUtils.getEpochSeconds(anyLong(), anyLong(), anyInt()))
                .thenReturn(h2CurrentInstant.getEpochSecond());
        mockDateTimeUtils.when(() -> DateTimeUtils.currentTimestamp(any(TimeZoneProvider.class), any(Instant.class)))
                .thenReturn(h2TsTimeZone);
    }

    @AfterEach
    public void tearDown() {
        mockDateTimeUtils.close();
    }


    @Test
    public void currentH2Date() {
        Object currentDate = repository.currentDate();
        log.debug("h2 current date : {} ", currentDate);
        assertNotNull(currentDate);
        assertEquals("2024-05-05", currentDate.toString());
    }
}
```

<a href="#index">back to index</a>


<a name="junit5-mockito"></a>
### JUnit 5 : Mockito and InjectMocks

```java
@Controller
public class DummyController {

    private final DummyService dummyService;

    public DummyController(DummyService dummyService) {
        this.dummyService = dummyService;
    }
}
```

Spring Boot 2.5.2 comes with junit 5 and Mockito, no extra dependency is required.

```java
@ExtendWith(MockitoExtension.class)
public class DummyControllerTest {

    @Mock
    private DummyService dummyService;

    @InjectMocks
    private DummyController dummyController;
    
    // test something here
} 
```

With this approach, there is no need to call ```MockitoAnnotations.openMocks(this)```.

<a href="#index">back to index</a>


<a name="junit5-temp-files"></a>
### JUnit 5 : Using temp files

A subfolder will automatically be created on machine temp folder.  
It will be cleared when the tests are processed, whatever if it worked of failed.
```java
@TempDir Path tempDir;

Files.createDirectories(Paths.get(tempDir.toString()));
Files.writeString(Paths.get(tempDir.toString() + File.separator + "test.json"), "{\"foo\":\"bar\"}", CREATE);
```

<a href="#index">back to index</a>

<a name="springboot-pom"></a>
### Spring Boot : set pom.xml

* only use JUnit 5
```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <!-- only use junit 5 -->
            <exclusions>
                <exclusion>
                    <groupId>junit</groupId>
                    <artifactId>junit</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
```

* integration tests setup : just name your class SomethingIT.java
```xml
            <!-- Split unit tests and integration tests -->
            <plugin>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.0.0-M5</version>
            </plugin>
            <plugin>
                <artifactId>maven-failsafe-plugin</artifactId>
                <version>3.0.0-M5</version>
                <executions>
                    <execution>
                        <id>integration-test</id>
                        <goals>
                            <goal>integration-test</goal>
                            <goal>verify</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
```

<a href="#index">back to index</a>

<a name="springboot-rest-controller"></a>
### Spring Boot : testing a rest controller

* JUnit 5 : test the controller from url, mocking the data
```java
@ExtendWith(SpringExtension.class)
@WebMvcTest(RestPostgresController.class)
@ContextConfiguration(classes = {RestPostgresController.class, PostgresJdbcRepository.class})
@AutoConfigureMockMvc
public class RestPostgresControllerTest {

    private ObjectMapper objectMapper = new ObjectMapper();

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private PostgresJdbcRepository repository;

    @Test
    public void getUsers() throws Exception {
        doReturn(TestUtil.getUsers()).when(repository).getAllUsers();
        MvcResult mvcResult = this.mockMvc
                .perform(MockMvcRequestBuilders.get("/api/rest/postgres/users/all"))
                .andExpect(status().isOk())
                .andDo(print())
                .andReturn();
        assertEquals(200, mvcResult.getResponse().getStatus()); // same as isOk()
        String jsonResponse = mvcResult.getResponse().getContentAsString();
        List<User> result = objectMapper.readValue(jsonResponse, new TypeReference<List<User>>(){});
        assertNotNull(result);
        assertFalse(result.isEmpty());
        assertEquals(2, result.size());
    }
}
```

* JUnit 5 : integration test
```java
@ExtendWith(SpringExtension.class)
@WebMvcTest(RestPostgresController.class)
@ContextConfiguration(classes = {AppProperties.class, PostgresConfiguration.class, RestPostgresController.class,
        GlobalExceptionHandler.class, PostgresJdbcRepository.class, PostgresUserRowMapper.class})
@ActiveProfiles("docker")
public class RestPostgresControllerIT {

    private ObjectMapper objectMapper = new ObjectMapper();

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void getUsers() throws Exception {
        MvcResult mvcResult = this.mockMvc
                .perform(MockMvcRequestBuilders.get("/api/rest/postgres/users/all"))
                .andExpect(status().isOk())
                .andDo(print())
                .andReturn();
        String jsonResponse = mvcResult.getResponse().getContentAsString();
        List<User> result = objectMapper.readValue(jsonResponse, new TypeReference<List<User>>(){});
        assertNotNull(result);
        assertFalse(result.isEmpty());
        assertEquals(91, result.size());
    }
}    
```

* rest service with post
 
```java
    @Test
    public void postStuff() throws Exception {
        MvcResult mvcResult = this.mockMvc.perform(post("/api/rest/sandbox")
                .content(objectMapper.writeValueAsString(new Country("Wakanda", "10000")))
                .contentType(MediaType.APPLICATION_JSON))
                .andDo(print())
                .andExpect(status().isOk())
                .andReturn();
        assertEquals("Wakanda", mvcResult.getResponse().getContentAsString());
    }
```

* JUnit 4
```java
@SpringBootTest(classes = {BasicController.class, BasicService.class})
@AutoConfigureMockMvc
@EnableWebMvc // usefull when a service send ResponseEntity<SomeBean>
public class BasicControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Autowired
    private BasicController basicController;

    @Test
    public void getPerson() throws Exception {
        // version 1
        MvcResult mvcResult = mockMvc.perform(
                get("/rest/getperson")
                .contentType(MediaType.APPLICATION_JSON)
        ).andReturn();
        assertEquals(200, mvcResult.getResponse().getStatus());
        JSONObject jsonObject = new JSONObject(mvcResult.getResponse().getContentAsString());
        assertEquals("Bob", jsonObject.getString("firstName"));
        assertEquals("Razovsky", jsonObject.getString("lastName"));

        // version 2
        this.mockMvc.perform(
                get("/rest/getperson")
                .contentType(MediaType.APPLICATION_JSON)
            )
            //.andDo(print())
            .andExpect(status().isOk())
            .andExpect(content().json("{\"firstName\":\"Bob\",\"lastName\":\"Razovsky\"}"))
            .andExpect(jsonPath("$.firstName", is("Bob")))
            .andExpect(jsonPath("$.lastName", is("Razovsky")))
        ;
    } 
}
```

<a href="#index">back to index</a>

<a name="springboot-mvc-controller"></a>
### Spring Boot : testing a spring mvc controller

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
public class UserControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void getSpringJdbcUsersPage() throws Exception {
        this.mockMvc.perform(get("/springjdbc/users/all"))
                //.andDo(print())
                .andExpect(status().isOk())
                .andExpect(view().name("users"))
                .andExpect(model().attributeExists("accessType"))
                .andExpect(model().attributeExists("userList"))
                .andExpect(forwardedUrl("/WEB-INF/views/users.jsp"));
    }
}
```

<a href="#index">back to index</a>

<a name="springboot-spring-data"></a>
### Spring Boot : testing a spring data datasource

* testing the service with insert in the test class
```java
@ExtendWith(MockitoExtension.class)
@ContextConfiguration(classes = {H2UserService.class, H2UserDataRepository.class})
@DataJpaTest
@EnableJpaRepositories(basePackages = {"edu.persistence.h2"})
@EntityScan(basePackages = "edu.entity")
@ActiveProfiles("test")
public class H2UserServiceTest {

    private final Logger LOG = LoggerFactory.getLogger(H2UserServiceTest.class);

    @Autowired
    private H2UserService h2UserService;

    @Test
    @Sql(statements = {
            "delete from Users; " +
            "INSERT INTO Users (UserID, FirstName, LastName, Address, City, PostalCode, Country, Phone) VALUES (1,'Maria','Anders','Obere Str. 57','Berlin','12209','Germany','030-0074321');" +
            "INSERT INTO Users (UserID, FirstName, LastName, Address, City, PostalCode, Country, Phone) VALUES (2,'Ana','Trujillo','Avda. de la Constituciùn 2222','México D.F.','05021','Mexico','(5) 555-4729');" +
            "INSERT INTO Users (UserID, FirstName, LastName, Address, City, PostalCode, Country, Phone) VALUES (3,'Antonio','Moreno','Mataderos  2312','México D.F.','05023','Mexico','(5) 555-3932');" +
            "INSERT INTO Users (UserID, FirstName, LastName, Address, City, PostalCode, Country, Phone) VALUES (4,'Thomas','Hardy','120 Hanover Sq.','London','WA1 1DP','UK','(171) 555-7788');" +
            "INSERT INTO Users (UserID, FirstName, LastName, Address, City, PostalCode, Country, Phone) VALUES (5,'Christina','Berglund','Berguvsvègen  8','Luleî','S-958 22','Sweden','0921-12 34 65');" +
            "commit;"})
    @Commit
    public void getAllUsers() {
        List<User> users = h2UserService.getAllUsers();
        assertNotNull(users);
        assertFalse(users.isEmpty());
        assertEquals(5, users.size());
    }
}
```

* testing the service with a sql file (in the resources)  
`users.sql` will contain delete/one or multiple insert/commit
```java
@ExtendWith(MockitoExtension.class)
@ContextConfiguration(classes = {H2UserService.class, H2UserDataRepository.class})
@DataJpaTest
@EnableJpaRepositories(basePackages = {"edu.persistence.h2"})
@EntityScan(basePackages = "edu.entity")
@ActiveProfiles("test")
public class H2UserServiceImportTest {

    @Autowired
    private H2UserService h2UserService;

    @Test
    @Transactional
    @Sql({"/users.sql"})
    public void getAllUsers() {
        List<User> users = h2UserService.getAllUsers();
        assertNotNull(users);
        assertFalse(users.isEmpty());
        assertEquals(8, users.size());
    }
}
```

* testing the service with data.sql  
`data-sandbox.sql` will contain : drop table/create table/one or multiple insert
```java
@ExtendWith(SpringExtension.class)
@ContextConfiguration(classes = {H2UserService.class, H2UserDataRepository.class})
@DataJpaTest
@EnableJpaRepositories(basePackages = {"edu.persistence.h2"})
@EntityScan(basePackages = "edu.entity")
@ActiveProfiles("test")
@SqlGroup({@Sql(executionPhase = Sql.ExecutionPhase.BEFORE_TEST_METHOD, scripts = "classpath:data-sandbox.sql")})
public class H2UserServiceDataSqlTest {

    private H2UserService h2UserService;

    @Autowired
    private H2UserDataRepository h2UserDataRepository;

    @BeforeEach
    public void setup() {
        h2UserService = new H2UserService(h2UserDataRepository);
    }
    
    // then call the service in the tests
}    
```

* testing the controller 

```java
@ExtendWith(SpringExtension.class)
@ContextConfiguration(classes = {RestH2Controller.class, H2UserService.class, H2UserDataRepository.class})
@DataJpaTest
@EnableJpaRepositories(basePackages = {"edu.persistence.h2"})
@EntityScan(basePackages = "edu.entity")
@ActiveProfiles("test")
public class RestH2ControllerIntegrationTest {

    private ObjectMapper objectMapper = new ObjectMapper();

    private MockMvc mockMvc;

    @Autowired
    private H2UserService h2UserService;

    private RestH2Controller restH2Controller;

    @BeforeEach
    public void setup() {
        restH2Controller = new RestH2Controller(h2UserService);
        this.mockMvc = MockMvcBuilders.standaloneSetup(restH2Controller)
                            .setControllerAdvice(new GlobalExceptionHandler())
                            .build();
    }

    @Test
    @Sql(statements = {
            "delete from Users; " +
            "INSERT INTO Users (UserID, FirstName, LastName, Address, City, PostalCode, Country, Phone) VALUES (1,'Maria','Anders','Obere Str. 57','Berlin','12209','Germany','030-0074321');" +
            "INSERT INTO Users (UserID, FirstName, LastName, Address, City, PostalCode, Country, Phone) VALUES (2,'Ana','Trujillo','Avda. de la Constituciùn 2222','México D.F.','05021','Mexico','(5) 555-4729');" +
            "INSERT INTO Users (UserID, FirstName, LastName, Address, City, PostalCode, Country, Phone) VALUES (3,'Antonio','Moreno','Mataderos  2312','México D.F.','05023','Mexico','(5) 555-3932');" +
            "INSERT INTO Users (UserID, FirstName, LastName, Address, City, PostalCode, Country, Phone) VALUES (4,'Thomas','Hardy','120 Hanover Sq.','London','WA1 1DP','UK','(171) 555-7788');" +
            "INSERT INTO Users (UserID, FirstName, LastName, Address, City, PostalCode, Country, Phone) VALUES (5,'Christina','Berglund','Berguvsvègen  8','Luleî','S-958 22','Sweden','0921-12 34 65');" +
            "commit;"})
    //@Commit
    public void getUsers() throws Exception {
        MvcResult mvcResult = this.mockMvc
                .perform(MockMvcRequestBuilders.get("/api/rest/h2/users/all"))
                .andExpect(status().isOk())
                .andDo(print())
                .andReturn();
        String jsonResponse = mvcResult.getResponse().getContentAsString();
        List<User> result = objectMapper.readValue(jsonResponse, new TypeReference<List<User>>(){});
        assertNotNull(result);
        assertFalse(result.isEmpty());
        assertEquals(5, result.size());
    }
}
```

<a href="#index">back to index</a>

<a name="springboot-allowed-classes"></a>
### Spring Boot : only allow some classes for a test

For this test, we only allow some classes so that we are not impacted by any other configuration that would fail.

```java
@ExtendWith(SpringExtension.class)
@SpringBootTest(classes = {AppProperties.class, PostgresConfiguration.class, PostgresJdbcRepository.class})
@ActiveProfiles("docker")
public class PostgresJdbcRepositoryIT {

    @Autowired
    private PostgresJdbcRepository repository;

    @Test
    public void getAllUsers() throws PostgresException {
        List<User> userList = repository.getAllUsers();
        assertNotNull(userList);
        assertFalse(userList.isEmpty());
        assertEquals(91, userList.size());
    }
}
```

<a href="#index">back to index</a>

<a name="springboot-test-ressources"></a>
### Spring Boot : use test resources with a profile

Using "test" profile will automatically make the test class use test resources : application-test.properties, logback-test.xml, etc
```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = {AppProperties.class})
@ActiveProfiles("test")
public class AppPropertiesTest {

    @Autowired
    private AppProperties appProperties;

    // define some tests
}
```

An alternate way, useful when working with multiple versions of one file :
```java
@TestPropertySource(locations = {"classpath:application-test.properties"})
@TestPropertySource(properties = { "spring.config.location=classpath:application-test.yml" })
@TestPropertySource(properties = { "logging.config=./src/test/resources/logback-test.xml" })
```

remark : the test class will use main resources if no profile or file path is defined (!)

<a href="#index">back to index</a>

<a name="springboot-test-multipartfile"></a>
### Spring Boot : test a MultipartFile

```java
import org.springframework.mock.web.MockMultipartFile;  // spring-boot-starter-test
import org.springframework.web.multipart.MultipartFile; // spring-boot-starter-web

// create file with commons-io
FileUtils.writeStringToFile(new File("./tmp/foo"), "bar", UTF_8);
File file = new File("./tmp/foo");
FileInputStream input = new FileInputStream(file);

MultipartFile multipartFile = new MockMultipartFile("foo",
    file.getName(), "text/plain", IOUtils.toByteArray(input));

this.mockMvc.perform(multipart("/api/rest/sandbox/upload")
    .file(multipartFile))
    .andDo(print())
    .andExpect(status().isOk());
```

<a href="#index">back to index</a>

<a name="springboot-disable-security"></a>
### Spring Boot : disable security in a test

```java
@AutoConfigureMockMvc(addFilters = false)
```

<a href="#index">back to index</a>

<a name="springboot-rest-external-calls"></a>
### Spring Boot : external calls to rest services

```java
@ExtendWith(SpringExtension.class)
@ActiveProfiles("test")
public class RestExternalCallIT {

    private RestTemplate restTemplate;

    @BeforeEach
    public void setup() {
        restTemplate = new RestTemplate();
    }

    @Test
    public void isAliveStringWithGetForObject() {
        String url = "http://localhost:8088/api/rest/isAlive";
        String result = restTemplate.getForObject(url, String.class);
        assertNotNull(result);
        assertEquals("I am alive", result);
    }

    @Test
    public void isAliveJsonWithGetForObject() {
        String url = "http://localhost:8088/api/rest/isAlive/json";
        Map<String, Object> map = restTemplate.getForObject(url, Map.class);
        assertNotNull(map);
        assertNotNull(map.get("isAlive"));
        assertEquals(true, map.get("isAlive"));
    }

    @Test
    public void personsWithGetForObject() {
        String url = "http://localhost:8088/api/rest/persons";
        Person[] personArray = restTemplate.getForObject(url, Person[].class);
        List<Person> personList = Arrays.asList(personArray);
        assertNotNull(personList);
        assertFalse(personList.isEmpty());
        assertEquals(5, personList.size());
    }

    @Test
    public void personsWithGetForEntity() {
        String url = "http://localhost:8088/api/rest/persons";
        ResponseEntity<Person[]> response = restTemplate.getForEntity(url, Person[].class);
        assertNotNull(response);
        assertEquals(HttpStatus.OK, response.getStatusCode());

        Person[] personArray = response.getBody();
        List<Person> personList = Arrays.asList(personArray);
        assertNotNull(personList);
        assertFalse(personList.isEmpty());
        assertEquals(5, personList.size());
    }

    @Test
    public void getPersonsWithExchange() {
        String url = "http://localhost:8088/api/rest/persons";
        HttpHeaders headers = new HttpHeaders();
        headers.setAccept(Arrays.asList(MediaType.APPLICATION_JSON));
        ResponseEntity<List<Person>> response = restTemplate.exchange(
                url, HttpMethod.GET, new HttpEntity<String>(headers),
                new ParameterizedTypeReference<List<Person>>() {});

        List<Person> personList = response.getBody();
        assertNotNull(personList);
        assertFalse(personList.isEmpty());
        assertEquals(5, personList.size());
    }

    @Test
    public void sandboxPostForEntity() {
        String url = "http://localhost:8088/api/rest/sandbox";
        Country country = new Country("Wakanda","10000");
        HttpEntity<Country> request = new HttpEntity<>(country);
        ResponseEntity<String> response = restTemplate.postForEntity(url, request, String.class);
        assertNotNull(response);
        assertEquals(HttpStatus.OK, response.getStatusCode());

        String result = response.getBody();
        assertNotNull(result);
        assertEquals("Wakanda", result);
    }

    @Test
    public void sandboxPostWithExchange() {
        String url = "http://localhost:8088/api/rest/sandbox";
        HttpHeaders headers = new HttpHeaders();
        headers.setAccept(Arrays.asList(MediaType.APPLICATION_JSON));
        Country country = new Country("Wakanda","10000");
        HttpEntity<Country> request = new HttpEntity<>(country, headers);
        ResponseEntity<String> response = restTemplate.exchange(url, HttpMethod.POST, request, String.class);

        assertEquals(HttpStatus.OK, response.getStatusCode());
        String result = response.getBody();
        assertNotNull(result);
        assertEquals("Wakanda", result);
    }
}
```

<a href="#index">back to index</a>

<a name="springboot-soap"></a>
### Spring Boot : testing a SOAP web service

* integration test in client project
```java
public class SoapClientIT {

    private SoapConnectorClient client;

    @BeforeEach
    public void setup() {
        client = new SoapConnectorClient();
        Jaxb2Marshaller marshaller = new Jaxb2Marshaller();
        marshaller.setContextPath("generated.soap.wsdl");  // generate package
        client.setMarshaller(marshaller);
        client.setUnmarshaller(marshaller);
    }

    @Test
    public void getUserById() {
        GetUserByIdRequest request = new GetUserByIdRequest();
        request.setId(1L);
        GetUserByIdResponse response = (GetUserByIdResponse) client.callWebService("http://localhost:8088/soap/sandbox", request);
        assertNotNull(response);
        assertEquals(1L, response.getUser().getId());
        assertEquals("Maria", response.getUser().getFirstName());
        assertEquals("Anders", response.getUser().getLastName());
        assertEquals("Germany", response.getUser().getCountry());
    }
}
```

* unit test in server project
```java
@SpringBootTest(classes = {SqliteUserEndpoint.class, SqliteUserJdbcRepository.class})
public class SqliteUserEndpointTest {

    @Autowired
    private SqliteUserEndpoint client;

    @MockBean
    private SqliteUserJdbcRepository repository;

    @Test
    public void getUser() {
        doReturn(TestUtil.getUsers().get(0)).when(repository).getUser(1);
        GetUserByIdRequest request = new GetUserByIdRequest();
        request.setId(1L);
        GetUserByIdResponse response = client.getUser(request);
        assertNotNull(response);
        assertEquals("Smith", response.getUser().getLastName());
        assertEquals("John", response.getUser().getFirstName());
        assertEquals("USA", response.getUser().getCountry());
    }
}
```

<a href="#index">back to index</a>

<a name="maven-test-library"></a>
### Maven : Test a local library

When developing a library, you may want to test it from a client project, before uploading it on a remote repository.  
First approach is to use your local repository, installing the jar with ```mvn clean install some-project```.  
Second approach, when you already have an external jar, and can't install it in your local repository, is to specify its path.  
remark : this practice is just for testing, don't commit your pom with this reference 

```xml
<dependency>
    <groupId>edu.something</groupId>
    <artifactId>library-name</artifactId>
    <version>1.0.0</version>
    <scope>system</scope>
    <systemPath>${basedir}/lib/something.jar</systemPath>
</dependency> 
```

<a href="#index">back to index</a>
        

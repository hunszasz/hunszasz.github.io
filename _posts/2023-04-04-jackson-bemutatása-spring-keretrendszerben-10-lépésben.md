---
layout: post
title: Jackson bemutatása Spring keretrendszerben 10 lépésben
date: 2023-04-04 10:55 +0200
categories: [Fejlesztői környezet, Hasznos, Trend]
seo.type: BlogPosting
image: /assets/jackson.webp
description: 10 lépésben megtanulhatod a Jackson működését Spring keretrendszeren belül
---

# Cél
Ha elolvasod a bejegyzést, akkor
* betekintést nyersz a Spring keretrendszerben hogyan hasznosulnak a Jackson moduljai,
* tudni fogod, milyen lehetőségeid vannak a Jackson konfigurálására
* kivételek kezelésre kapsz lehetőségeket

Amennyiben ezt kihagyod, úgy kockáztatod, hogy
* nem fogod érteni, mi történik automatikusan a REST kommunikációdban
* feleslegesen terheled a hálózatot - nem optimális az előállt json

# Általában a Jackson-ról
A Jackson a Java egyik legnépszerűbb adatfeldolgozó eszközkészlete. A benne található API-k zászlóshajója
az egyszerű, de nagyon hatékony **JSON parser/generator** a Java objektumok és JSON adatok közötti átalakítást végzi.
A Jackson integrálva van a Spring keretrendszerrel.

# Gyakorló 10 lépésben
A Spring Boot egy nagyon népszerű Java keretrendszer, amely lehetővé teszi a fejlesztők számára, hogy gyorsan és hatékonyan hozzanak létre Java alapú alkalmazásokat. Az alábbiakban egy 10 lépéses gyakorlót mutatok be, amely segítséget nyújt a Spring Boot Jackson ObjectMapper testreszabásában.

1. Első lépésként hozzunk létre egy új Spring Boot projektet. \
   Inicializáljunk egy projektet a [Spring Initializer](https://start.spring.io/#!type=maven-project&language=java&platformVersion=3.0.5&packaging=jar&jvmVersion=17&groupId=hu.hunszasz.example.spring&artifactId=spring-jackson&name=Spring%20Jackson%20Demo%20Config&description=Demo%20project%20for%20Spring%20Boot&packageName=hu.hunszasz.example.spring.spring-jackson&dependencies=web) segítségével az alábbi beállításokkal. \
    ![img.png](/assets/initializer.webp) &nbsp;
2. Hozzunk létre egy egyszerű osztályt, amely tartalmaz néhány mezőt. \
   Adjuk a függőségek közé a [Lombok projektet](https://projectlombok.org/), hogy még egyszerűbb legyen a kódunk felépítése:
   
   ```xml
   <dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
   </dependency>
   ```
   Majd az alábbi osztályt használjuk mintaként.
   ```java
   @Data
   @Builder
   @JsonIgnoreProperties("text")
   public class TestData {
       int anInt;
       String text;
       boolean aBoolean;
       LocalDateTime localDateTime;
   }
   ```
3. Készítsünk egy Service-t ami használja az **ObjectMapper**-t a Java objektumunk átalakítására JSON formátumúvá.

   ```java
   @Service
   public class TestService {
   ObjectMapper objectMapper;
   
       public TestService(ObjectMapper objectMapper) {
           this.objectMapper = objectMapper;
       }
   
       public String getTestDataAsJson(TestData o) throws JsonProcessingException {
           return objectMapper.writerWithDefaultPrettyPrinter().writeValueAsString(o);
       }
   }
   ```
4. Hozzunk létre egy JUnit tesztet, amellyel a Service-ünk metódusát fogjuk tesztelni.
   
   ```java
   class SpringJacksonApplicationTests {
       @Autowired
       TestService testService;
   
       @Test
       void doTestWithService() throws JsonProcessingException {
       TestData testData=TestData.builder().text("Ez egy teszt objektum").anInt(42).aBoolean(false).localDateTime(LocalDateTime.of(2023,
            Month.APRIL, 6, 19, 30, 40)).build();
       String jsonString = testService.getTestDataAsJson(testData);
       assertEquals("""
           {    
             "anInt" : 42,
             "text" : "Ez egy teszt objektum",
             "localDateTime" : [ 2023, 4, 6, 19, 30, 40 ],
             "aboolean" : false
           }""", jsonString);
       }
   }
   ```
5. A tesztben változtassunk a sorrenden, hagyjuk ki a text mezőt és legyen formázott a localDateTime. Legyen az összehasonlítás elvárt értéke a következő:
   
   ```json
   {    
     "anInt" : 42,
     "aboolean" : false,
     "localDateTime" : "2023.04.06 19:30"
   }
   ```
   Ezt követően a jUnit teszt hibára fog futni. A helyes teszt futáshoz módosítsuk a TestData osztályt úgy, hogy az alábbi annotációkat adjuk hozzá:
   ```java
   @Data
   @Builder
   @JsonIgnoreProperties("text")
   @JsonPropertyOrder({"anInt", "aboolean", "localDateTime"})
   public class TestData {
    
   }
   ```
   Illetve szükséges az időformátum beállításához, egyrészt tiltani a timestamp-ként való kiírást - ezt properties-ben a következő bejegyzéssel tehetjük meg:
   ```properties
   spring.jackson.serialization.write-dates-as-timestamps=false
   ```
   Másrészt készítsük el azt a konfigurátor osztályt, amely a jackson ObjectMapper-ünket fogja kezelni.
   ```java
   @Configuration
   public class JacksonConfig {
   
       DateTimeFormatter dateFormatter = DateTimeFormatter.ofPattern("yyyy.MM.dd");
       DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy.MM.dd HH:mm");
       LocalDateTimeSerializer localDateTimeSerializer = new LocalDateTimeSerializer(dateTimeFormatter);
       LocalDateTimeDeserializer localDateTimeDeserializer = new LocalDateTimeDeserializer(dateTimeFormatter);
       LocalDateSerializer localDateSerializer = new LocalDateSerializer(dateFormatter);
       LocalDateDeserializer localDateDeserializer = new LocalDateDeserializer(dateFormatter);
       
       @Bean
       @Primary
       public ObjectMapper jsonObjectMapper() {
           ObjectMapper mapper = new ObjectMapper();
           mapper.configure(SerializationFeature.FAIL_ON_EMPTY_BEANS, false);
           mapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
           mapper.configure(MapperFeature.DEFAULT_VIEW_INCLUSION, true);
           mapper.registerModule(new JavaTimeModule());
   
           SimpleModule customTimeModule = new SimpleModule();
           customTimeModule.addSerializer(localDateSerializer);
           customTimeModule.addSerializer(localDateTimeSerializer);
           customTimeModule.addDeserializer(LocalDate.class, localDateDeserializer);
           customTimeModule.addDeserializer(LocalDateTime.class, localDateTimeDeserializer);
           mapper.registerModule(customTimeModule);
   
           return mapper;
       }
   }
   ```
6. Ezek után jöhet a REST kommunikáción való konfigurálás. Készítsünk el egy "@RestController" annotációval ellátott osztályt, az alábbiak szerint:
   
   ```java
   @RestController
   public class TestController {
   Logger logger = LoggerFactory.getLogger(this.getClass());
   
       @Autowired
       ObjectMapper objectMapper;
   
       @PostMapping("/testData")
       public TestData getTestData() throws JsonProcessingException {
           TestData tst = TestData.builder().text("tst")
                   .aBoolean(true)
                   .localDateTime(LocalDateTime.now())
                   .anInt(2).build();
   
           if (logger.isDebugEnabled()) {
               logger.debug(objectMapper.writeValueAsString(tst));
           }
           return tst;
       }
   }
   ```
7. Inicializáljunk Swagger UI-t, hogy lássuk mit ad vissza a végpont. Adjuk hozzá a függőségeinkhez a következő artifact-ot:

   ```xml
   <dependency>
      <groupId>org.springdoc</groupId>
      <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
      <version>2.1.0</version>
   </dependency>
   ```
8. Ellenőrizzük a konzolon kiírt és REST lábon visszaadott json formáját. Nyissuk meg böngészőben a [Swagger ui-t a http://localhost:8080/swagger-ui/index.html](http://localhost:8080/swagger-ui/index.html)
   ![img.png](/assets/swagger.webp)

   Illetve a konzol

   ![img.png](/assets/console.webp)

9. Hozzuk létre a GET végpontunkat tesztelő metódust. Ehhez első körben a teszt metódusokat befoglaló osztályt kell felannotálni a **@WebMvcTest(TestController.class)**, illetve be kell kötni a MockMvc objektumot: - a Service réteg tesztelő metódusához hasonlóan, az elvárt formátumot adjuk meg - az alábbiak szerint:
   
    ```java
     @WebMvcTest(TestController.class)
     class SpringJacksonApplicationTests{
        @Autowired
        MockMvc mockMvc;
   
        @Test
        void doTestWithRest() throws Exception {
            mockMvc.perform(MockMvcRequestBuilders.get("/testData").accept(MediaType.APPLICATION_JSON))
            .andDo(print())
            .andExpect(status().isOk())
            .andExpect(MockMvcResultMatchers.jsonPath("$.text").doesNotExist())
            .andExpect(MockMvcResultMatchers.jsonPath("$.anInt").value(42))
            .andExpect(MockMvcResultMatchers.jsonPath("$.aboolean").value(false))
            .andExpect(MockMvcResultMatchers.jsonPath("$.localDateTime").value("2023.04.06 19:30"));
        }
        /*...*/
     }
     ```
     Láthatjuk a kimeneten, hogy valami nem stimmel, a teszt nem fut le hiba nélkül:
     ```shell
     java.lang.AssertionError: Got a list of values [2023,4,6,19,30,40] instead of the expected single value 2023.04.06 19:30
     ```
10. Konfiguráljuk az MVC-t, hogy a korábban beállított ObjectMapper-t használja. Ehhez hozzunk létre egy új konfigurátort:
    
    ```java
    @Configuration
    public class MvcConfig implements WebMvcConfigurer {
   
        private ObjectMapper objectMapper;
   
        public MvcConfig(ObjectMapper objectMapper) {
            this.objectMapper = objectMapper;
        }
   
        @Override
        public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
            converters.stream()
                    .filter(MappingJackson2HttpMessageConverter.class::isInstance)
                    .map(c -> (MappingJackson2HttpMessageConverter) c)
                    .forEach(c -> c.setObjectMapper(objectMapper));
        }
    }
    ```
A következő teszt futás már megfelelő eredményt fog adni. További információkért, lehetőségekért ajánlom a lenti linkeket.

A forráskód megtalálható a következő [GitHub repository-ban](https://github.com/hunszasz/spring-jackson).

# Hasznos linkek
* [Jackson GitHub](https://github.com/FasterXML/jackson)
* [JavaTPoint jackson](https://www.javatpoint.com/jackson)
* [Baeldung jackon tutorial](https://www.baeldung.com/jackson)
* [Swagger ui integration](https://springdoc.org/v2/)
* [Lombok projektet](https://projectlombok.org/)
* [Spring Initializer](https://start.spring.io/)
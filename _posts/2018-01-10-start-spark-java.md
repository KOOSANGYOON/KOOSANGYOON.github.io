---
layout: post
title: SPARK JAVA 의 기초 학습
date: 2018-01-10T17:32:40.000Z
image: 'https://github.com/KOOSANGYOON/TIL/blob/master/TIL201801/spark.jpg?raw=true'
description: TIL (SPARK 가 튀는 불꽃놀이)
category: TIL
tags:
  - TIL
  - CODING
  - SPARK
  - JAVA
twitter_text: SPARK.JAVA 의 기초 학습
introduction: SPARK.JAVA 의 기초 학습
---

# (2018.01.10)

## TIL

0. why use template engine ?

1. spark.java 시작

2. JAVA-racingCar 에의 적용 (WEB UI사용)

---

#### why use template engine ?
- HTML 문법 만으로는 if/for/while과 같은 프로그래밍이 가능하지 않다. 즉, HTML만 활용하는 경우 항상 같은 데이터(정적인)만 서비스가 가능하다.

- 사용자에 따라 다른 HTML, 데이터에 따라 다른 HTML을 제공하려면 if/for/while과 같은 프로그래밍이 가능해야 한다. 이와 같이 서로 다른 HTML(동적인)을 제공하기 위한 도구가 template engine이다.

- template engine의 역할은 JSP, ASP, PHP가 하는 역할과 같다고 생각하면 된다.

---

### 1\. spark.java 시작

bowling 과제를 끝마치고 level 3 과정을 시작했다. level 3 과정은 console 창이 아닌, WEB UI 를 이용하여 프로그래밍 하는 과정이다. 이를 위해 먼저 spark 웹 프레임 워크의 사용법에 대해 학습했다.

---
#### 1-1) spark. java ?

JAVA 진영에서 사용할 수 있는 상당히 가벼운 웹 프레임 워크 중 하나이다. 초기 학습비용이 낮고, 웹 애플리케이션을 경험해 보는데 굉장히 유용한 도구이다. 웹에 대한 학습을 시작하는 도구로 사용하기에 아주 적합하다.

---

#### 1-2) gradle project 생성

spark 웹 프레임 워크를 사용하기 위해 처음으로 해야 할 것은 gradle project 를 생성하는 것이다. 이를 위해 eclipse 에서 STS (spring tool suite) plugin 을 설치하여 실행할 수도 있고, STS를 직접 설치해서 사용할 수도 있다. 나는 후자를 택해 진행했다.

STS 를 실행한 후, eclipse 를 실행할 때 처럼 `command + n` 단축키를 이용해 프로젝트를 생성할 수 있다. `type` 을 `gradle (buildship 2.x)` 로 생성하면 끝이 난다.

---

#### 1-3) 시작 세팅

gradle build 도구의 기본 파일은 `build.gradle` 라는 파일이다. 이 파일을 켠 뒤에, 필요없는 코드들을 삭제하고, (현재는 spark.java 를 이용하므로, spring boot 관련 부분을 삭제한다.) main 부분과 test 부분에 있는 기본 spring boot 용 default 파일들을 삭제한다.

여기까지 완료했다면, `project and external dependencies` 폴더 안에 있는 파일들을 refresh 해 줄 필요가 있다.

1. `build.gradle 파일에 우클릭` 해준 뒤,

2. `gradle` 클릭

3. `refresh gradle project` 클릭

  이렇게 refresh가 가능하다.

여기까지 완료 되었다면, gradle 의 빌드 도구들을 dependencies에 넣어주어야 한다. spark java 홈페이지에 가보면 추가할 내용을 알 수 있다. 현재는,

> Gradle : compile "com.sparkjava:spark-core:2.7.1" // add to build.gradle (for Java users)

로 되어있다. 이 내용을 dependencies 에 적어준다. 완료 후에 다시 refresh를 해주면, 기본 세팅이 완료된다.

---

#### 1-4) main class 생성

main class 생성은 기존 eclipse 사용시와 동일하다.

클래스를 생성하고, 코딩을 한 뒤에 웹 서버를 띄워주기 위해서는

1. 우클릭 후 `Run as` 클릭

2. `Java application` 클릭

으로 서버를 띄워줄 수 있다. 단축키로는, `command` + `option(alt)` + `x` + `j` 이다. 서버의 주소는,

> localhost:4567

로 시작한다. 이를 원하는 주소로 코드에서 바꿔줄 수 있다. `controller` 에서 코드를 추가해본다. (controller 란, main() 을 말한다.)
- controller code
  ```java
  public class MainController {
      public static void main(String[] args) {
      port(8080);
    }
  }
  ```
  > 이 코드가 실행 된 후에는 localhost:8080 으로 서버의 주소가 설정된다.

웹의 내용을 수정한 뒤, 서버를 재시작 할 때에는 다시 클릭하는 과정들을 다 거치지 않고, 상단의 Relaunch 버튼을 클릭하면 바로 재시작 된다.

---

#### 1-5) 동적인 값 전달방법 `(get() 방식 이용하기)`

- get() 내에서의 사용법

  (.params() 사용법)

  - get("/hello", (req, res) -> { . . . })

    > localhost:4567/hello 로 갔을 때의 로직을 구현해준다. (정적인 주소)

  - get("/hello/:name", (req, res) -> { . . . req.params(":name") . . . })

    > localhost:4567/hello/koo 와 같이 뒤에 이름을 동적 값으로 넘겨줄 수 있다.

  이처럼 동적인 변수를 넘겨줄 때에는 url에서 변수이름 앞에 `: (콜론)` 을 붙여주어야 하고,

  로직 구현부에서는 `req.params("변수이름");` 로 받아주어야 반영이 가능하다.

  (.queryParams() 사용법)

  - 로직 구현부에서 `return "Hello " + req.queryParams("name");` 로 구현되었을 때,

    > 주소창에 `localhost:4567/hello?name=koo` 와 같이 입력하여 사용 가능하다.

  2개 이상의 값을 출력해줄 때에는, 로직 구현부에 `return "Hello " + req.queryParams("name") + " 나이는 " + req.queryParams("age");` 와 같이 구현하고,

  주소창에 `localhost:4567/hello?name=koo&age=20` 과 같이 `&(앤드)` 연산자를 주소창에 넣어서 사용이 가능하다.

---

#### 1-6) `html 파일 생성 / UI 생성` 과 `post() 로 서버에 데이터 전달하기`

- post() 방식이 왜 필요한가?

  - 기존의 get() 방식을 이용하면, 개인의 입력 정보들(ex. 이름, 나이, 아이디 등)이 url에 모두 표기가 된다.

    > localhost:4567/hello/name=koo&age=29

  이와 같이 url에 남는다.

  - 비밀번호와 같은 중요 정보(물론 나이, 이름 등도 개인정보이므로 중요.)들은 url에 보여서는 안된다. post() 방식은 이처럼 url에 정보를 뜨지 않게 전달하는 방식이다.

  - 또한, 데이터베이스 등의 "정보" 에 접근하는 모든 행위들은 post()로 구현한다.

- 어떻게 사용하는가 ?

  html 파일의 form의 기본 default 값은 `"get"` 이다. 따라서 html 문서 내에서 `<form>` 속성을 `"post"` 로 변경해주면 된다. 이를 구현하는 방법은 아래와 같다.

  - 기본 default 값(get방식 적용됨)

    ```sts
    <form action="/hello">
    ```

  - post 방식 사용 시

    ```sts
    <form action="/hello" method="post">
    ```

    ## ※ _추후 get() 방식과 post() 방식의 차이점에 대해 학습 필요!_

---

#### 1-7) 동적인 화면 구성하기 (template engine)

- 기존의 main 문은 아래와 같았다.

  ```java
  public class HelloWorld {
    public static void main(String[] args) {
        staticFiles.location("/static");

        post("/hello", (req, res) -> {
            return "get Hello " + req.queryParams("name") + " 나이는 "

                    + req.queryParams("age");
        });
    }
  }
  ```

- 이를 동적인 정보를 받아오는 html 창으로 변경하기 위해서는 첫번째로 직접 타이핑할 수 있다.

  ```java
  //example 1
  public class HelloWorld {
    public static void main(String[] args) {
        staticFiles.location("/static");

        post("/hello", (req, res) -> {
            return "<html>" +
                    "<body>" +
                    "<h1>회원 가입 결과</h1>" +
                    "이름 : " + req.queryParams("name") +
                    "<br /><br />" +
                    "나이 : " + req.queryParams("age") +
                    "</body>" +
                    "</html>";
        });
    }
  }
  ```

- 하지만 너무 하드코딩이다.. (코드가 길어지면 답이 없다..) 이럴 때는 템플릿 엔진을 이용하면 된다.

  - 먼저 sparkjava 홈페이지에서 handlebars 템플릿 엔진을 찾는다.

    ```sts
    <dependency>
    <groupId>com.sparkjava</groupId>
    <artifactId>spark-template-handlebars</artifactId>
    <version>2.7.1</version>
    </dependency>
    ```

    여기서 `groupId` 와 `artifactId` 를 gradle 파일의 `dependencies` 에 복사해준다.

    ```java
    dependencies {
    compile "com.sparkjava:spark-core:2.7.1"
    compile "com.sparkjava:spark-template-handlebars:2.7.1"
    }
    ```

    그 후에 gradle 파일을 우클릭한 후, refresh를 해주면, project and external dependencies 안에 template-handlebars 가 생성된 것을 확인할 수 있다.

  - 다음으로 helloWorld 파일을 수정한다. (render 메서드의 소스는 spark java에서 구할 수 있다.)

    ```java
    import static spark.Spark.*;

    import java.util.HashMap;
    import java.util.Map;

    import spark.ModelAndView;
    import spark.template.handlebars.HandlebarsTemplateEngine;

    public class HelloWorld {
       public static void main(String[] args) {
           staticFiles.location("/static");

           post("/hello", (req, res) -> {
               Map<String, Object> model = new HashMap<>();
               model.put("name", req.queryParams("name"));
               model.put("age", req.queryParams("age"));

               return render(model, "/result.html");
           });
       }

       public static String render(Map<String, Object> model,
                                        String templatePath) {
           return new HandlebarsTemplateEngine().render(
                        new ModelAndView(model, templatePath));
       }
    }
    ```

  - 마지막으로 result.html 을 수정한다.

    ```html
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="UTF-8">
    <title>회원 가입</title>
    </head>
    <body>
    <h1>회원 가입 결과2</h1>
    이름 : {{name}}
    <br />
    <br />
    나이 : {{age}}
    </body>
    </html>
    ```

    > 여기서 `{{. . .}}` 는 helloworld 에서 name 이름에 해당하는 값을 받아옴을 의미한다.

  - 수정 후에 resource 폴더 안에 templates 폴더를 만들어 주고, result.html을 이동시킨다. (default 값이 가르키는 곳이 resource -> templates 이다.)

---
#### 1-7-2) (추가)

- {-{. . .}} 의 사용법 ('-'는 일부러 적어둔 것이다. 없다고 생각하자.)

만약, `main (controller)` 에서 다음과 같이 설정했다고 하자.
```java
public class Person {
  private string name;
  private int age;
  //생성자
  public person(String name, int age) {
    this.name = name;
    this.age = age;
  }
  //'name'을 리턴
  public String getName() {
    return name;
  }
  //'age'를 리턴
  public int getAge() {
    return age;
  }
}
```

```java
//main start
public static void main (String[] args) {
  Scanner scan = new Scanner(System.in);

  post("/home", (req, res) -> {
    String name = scan.nextLine();  //이름 입력받음.
    int age = scan.nextInt();
    Person person1 = new Person(name, age); //person1 객체생성

    String name = scan.nextLine();  //이름 입력받음.
    int age = scan.nextInt();
    Person person2 = new Person(name, age); //person2 객체생성

    List<Person> persons = new ArrayList<Person> ();
    persons.add(person1);
    persons.add(person2);

    Map<String, Object> model = new HashMap<>();
    model.put("humans", persons);
  });
}

//render method
public static String render(Map<String, Object> model,
                              String templatePath) {
		return new HandlebarsTemplateEngine().render(
              new ModelAndView(model, templatePath));
}
```
> 문법은 약간 틀렸을 수 있다.. 구조만 눈에 보자.

  1. 이름과 나이를 입력받아 Person 객체를 생성하고,
  그 객체의 이름을 person1, person2 으로 명명했다.

  2. 이 후, render 메서드를 이용해서 html 문서로 보내주었다.

이와 같은 경우에 html 에서 `{{...}}` 를 이용해서 어떻게 값을 출력해 줄 수 있을까?
많은 구현 방법이 있겠지만, 아래와 같이도 가능하다.

```html
<body>
  {{#humans}}
	 <div id="standings">이름 : {{name}} , 나이 : {{age}}</div>
  {{/humans}}
</body>
```
> 첫째로, `이름 : {-{name}} , 나이 : {-{age}}` 라고 쓸 수 있는것이 중요하다.

즉, 객체를 인자로 전달하면, 그 객체의 `인스턴스변수` 를 {{name}} 와 같은
방법으로 끌어 쓸 수 있다는 것이다.

(물론 getName과 같은 메서드를 필수로 가지고 있어야한다.)

> 둘째로, `{-{#humans}} . . . {-{/humans}}` 안에 있는 코드는 `for()문` 을 돌듯이 동작한다는 것이다. 이는 `List` 를 인자로 넘길 때, 유용하게 사용된다.

- 코드를 요약 해보자면,

  1. java 코드에서 persons 리스트를 html 로 보내주었고, 그것을 html에서는 "humans" 로 받았으며,

  2. {-{#humans}} ... {-/humans} 와 같은 for()문 안에서 person 각각을 돌며 작동하고,

  3. person class의 인스턴스 변수인 name 과 age 를 출력해주는 것이다.

이와 같은 동작 원리를 더 머리속에 익혀두는 것이 중요한 것 같다.

---

#### 1-8) 여러개의 동적 값을 출력하기

여러개의 값을 출력하기 이전에 자바의 문법에 대해 알아야 한다. 예를들어 아래와 같이 User class를 만들었다고 하자.
- JAVA file (User)
  ```java
  public class User {
      private String name;
      private String age;

      public String getName() {
          return name;
      }

      public void setName(String name) {
          this.name = name;
      }

      public String getAge() {
          return age;
      }

      public void setAge(String age) {
          this.age = age;
      }

  }
  ```

그리고 main 에서 user들을 모아두는 list를 만들어서 관리한다고 하자.
- JAVA file (UserMain)
  ```java
  public class UserMain {
    public static void main(String[] args) {
      staticFiles.location("/static");
      List<User> users = new ArrayList<>();

      post("/users", (req, res) -> {
        User user = new User();
        user.setName(req.queryParams("name"));
        user.setAge(req.queryParams("age"));
        users.add(user);
        Map<String, Object> model = new HashMap<>();
        model.put("users", users);

        return render(model, "/result.html");
      });
    }
    public static String render(Map<string, object=""> model,
                                String templatePath) {
      return new HandlebarsTemplateEngine().render(
                      new ModelAndView(model, templatePath));
    }
  }
  ```

그리고 나서 html문서에 user가 아닌, users (List)를 보내서 결과를 얻고싶다.

html 에서 user 의 name 에 접근하려고 할 때, 이렇게 사용할 것이다.
- html file
  ```html
  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="UTF-8">
  <title>회원 가입</title>
  </head>
  <body>
  <h1>회원 가입 결과2</h1>
  {{#users}}
  이름 : {{name}}, 나이 : {{age}}
  <br />
  {{/users}}
  </body>
  </html>
  ```

일단 첫째로, 위와같이 코드를 작성한다면, List 내에 있는 모든 user들의 정보가 for문을 돌듯이 출력되어 나온다. (한명이 아닌 전체가) 이 문법을 기억하자.

두번째로, 중괄호 사이에 있는 name 과 age 는 과연 User 클래스의 인스턴스 변수일까?

> 정답은 `X` 다.

여기서 name과 age는 인스턴스 변수에 직접 접근하는것이 아닌, getName() 과 getAge() 내에 있는 name 과 age 이다. 이는 자바의 기본 문법이므로 숙지해야겠다.

---

### 2\. JAVA-racingCar 에의 적용 (WEB UI사용)

- WEB UI 를 적용하기 전에 먼저 클래스가 덜 분리된 부분을 수정했다. main()이 RacingCar 클래스 내에 정의되어 있었고, 그 내부에는 여러 메소드들이 존재하고 있었다.

  > MainController 클래스를 새로 만들고, main()을 옮겨주었다.

- 이후에, 기존의 console을 사용하던 것을 web UI를 적용해야 했기 때문에, 코드를 아래와 같이 수정했다.
  ```java
  package car;

  import java.util.ArrayList;
  import java.util.Arrays;
  import java.util.HashMap;
  import java.util.Map;

  import spark.ModelAndView;
  import spark.template.handlebars.HandlebarsTemplateEngine;

  import static spark.Spark.*;

  public class MainController {
      public static void main(String[] args) {
          ArrayList<Car> cars = new ArrayList<Car>();
          port(8080);

          get("/", (req, res) -> {
              return render(new HashMap<> (), "index.html");
          });

          post("/name", (req, res) -> {
              String inputName = req.queryParams("names");
              ArrayList<String> carNameList = new ArrayList<String>
              (Arrays.asList(RacingCar.splitName(inputName)));

              for (String name : carNameList) {
                  cars.add(new Car(name));
              }

              Map<String, Object> model = new HashMap<>();
              model.put("cars", cars);
              return render(model, "game.html");
          });

          get("/result", (req, res) -> {
              int inputNum = Integer.parseInt(req.queryParams("turn"));

              for (int i = 0; i < inputNum; i++) {
                  RacingCar.startRace(cars);
              }

              Map<String, Object> model = new HashMap<>();
              model.put("cars", cars);
              return render(model, "result.html");
          });
      }

      public static String render(Map<String, Object> model,
                                  String templatePath) {
          return new HandlebarsTemplateEngine().render(
                                new ModelAndView(model, templatePath));
      }
  }
  ```

- 이를 받아주는 html 문서들을 수정했다.

  ```html
  <table>
  {-{#cars}}
  <tr>
  <td>이름 : {{name}}<td>
  </tr>
  {-{/cars}}
  </table>
  ```

  > cars 리스트를 for문 돌듯이 순회하면서, name을 출력해주는 부분

  > #cars 부분에 '-' 는 없애야 함. (보기 위해 찍어놓음.)

  ```html
  <h3>결과입니다</h3>
  {-{#cars}}
  <div id="standings"> {{name}} : <pre>{{result}}&#128652;</pre></div>
  {-{/cars}}
  ```

  > 결과를 출력해주는 부분

---

### 학습을 마치며 . . .

단순히 eclipse 만 사용했을 때는 전혀 어렵지 않던 아주 기본적인 문법들이 STS를 사용해서 코딩하다보니 생각보다 어색하고 정리해 두어야 할 것이 많다고 느껴졌다. 앞으로 level3을 진행하면서, 계속해서 사용하다보면 언젠간 어색하지 않게 코드에 녹여낼 수 있는 날이 올 것이라 믿는다.

POBI(박재성 님)의 YOUTUBE 강의는 초보자의 눈높이에 맞춘 강의여서 이해가 잘 되고있다. 동영상 학습을 바탕으로 계속해서 직접 코드에 사용해보면서 손과 뇌에 익히는것이 중요한 것 같다.

앞으로의 level3 과제들이 기대되는 하루다. 화이팅!!

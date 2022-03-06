---
layout: post
title:  "[Android, Java] Retrofit을 이용하여 서버와 통신하기"
date:   2022-03-07 01:23:00+0900
categories: jekyll update
tags: [Android]
---
<p align="center"><img src="/assets/img/blog/정보/안드로이드.png"></p>

# Retrofit을 이용하여 통신하기
**Retrofit에 대한 설명은 [Retrofit이란?](https://daegom.com/main/android-post1/) 포스팅을 참고해주세요**  
  
`Retrofit` 통신 라이브러리를 사용하여 `jsonplaceholder` (http://jsonplaceholder.typicode.com/posts) 사이트에 있는 json 데이터를 불러와보자.  

# jsonplaceholder 사이트 확인
`jsonplaceholder`이라는 사이트는 무료 `json` 데이트를 제공한다. 직접 들어가보면 아래와 같이 json 데이터를 제공한다.

```json
[
  {
    "userId": 1,
    "id": 1,
    "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
    "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
  },
  {
    "userId": 1,
    "id": 2,
    "title": "qui est esse",
    "body": "est rerum tempore vitae\nsequi sint nihil reprehenderit dolor beatae ea dolores neque\nfugiat blanditiis voluptate porro vel nihil molestiae ut reiciendis\nqui aperiam non debitis possimus qui neque nisi nulla"
  },
  {
    "userId": 1,
    "id": 3,
    "title": "ea molestias quasi exercitationem repellat qui ipsa sit aut",
    "body": "et iusto sed quo iure\nvoluptatem occaecati omnis eligendi aut ad\nvoluptatem doloribus vel accusantium quis pariatur\nmolestiae porro eius odio et labore et velit aut"
  },
  {
    "userId": 1,
    "id": 4,
    "title": "eum et est occaecati",
    "body": "ullam et saepe reiciendis voluptatem adipisci\nsit amet autem assumenda provident rerum culpa\nquis hic commodi nesciunt rem tenetur doloremque ipsam iure\nquis sunt voluptatem rerum illo velit"
  },
  ... 이하 생략
```

# 1. Gradle 추가와 Android 권한 설정
`Retrofit`을 사용하기 위하여 `Gradle`에 의존성을 추가하자.

```java
# Gradle:Module
implementation 'com.squareup.retrofit2:retrofit:2.9.0'
implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
```
또한 **통신을 진행하기 `Manifests`에 인터넷 퍼미션을 추가한다.**
```java
# Manifests 일부
<uses-permission android:name="android.permission.INTERNET"/>
```
**안드로이드에서는 보안상의 문제로 `HTTP`접근을 허용하지 않는다.** 만약, 서버가 `HTTPS`가 아닌 `HTTP`라면 예외처리를 따로 해줘야하는 문제가 발생한다. 따라서 안드로이드 API가 `28` 이상이라면 `Manifests`에 `cleartext HTTP`를 활성화 시켜야 한다. 따라서 서버의 주소가 `Http`라면 아래의 줄을 `Manifest`에 추가한다.
```java
android:usesCleartextTraffic="true"
```
# 2. API에 대한 명세를 호출하기 위한 인터페이스(Interface) 작성

```java
public interface RetrofitAPI {
    @GET("/posts")
    Call<List<Post>> getData(@Query("userId") String id);
}
```

`Retrofit`은 `REST`기반이기에 API어노테이션을 사용할 수 있다. 종류는 아래와 같이 4가지가 존재한다.  
1. GET  
2. POST  
3. DELETE  
4. PUT  
`@GET`어노테이션은 `Retrofit` 클래스를 구현한 `baseUrl`주소의 뒷 주소이다. (서버의 주소 중 뒷부분)  
즉 위 코드는 `Call`객체를 선언하여 요청을 서버로 보내는 의미이며 `Call`의 제네릭 타입은 서버로부터 받아오는 JSON 데이터를 제네릭 타입으로 받는다는 의미이다. 따라서 Call의 제네릭은 `DTO`나 `VO` 클래스로 지정한다.  

# 3. JSON을 받기 위한 DTO(VO) 클래스 작성
```java
import com.google.gson.annotations.SerializedName;

public class Post {
    @SerializedName("userId")
    private int userId;
    @SerializedName("id")
    private int id;
    @SerializedName("title")
    private String title;
    @SerializedName("body")
    private String body;

    public int getUserId() {
        return userId;
    }

    public void setUserId(int userId) {
        this.userId = userId;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getBody() {
        return body;
    }

    public void setBody(String body) {
        this.body = body;
    }
}
```
단순한 `DTO` 클래스이다. 이때 클래스 이름은 `Retrofit` 인터페이스에 있는 `Call`의 제네릭으로 명시한다. 또한 `@SerializedName` 어노테이션은 서버로부터 받아오는 JSON 객체와 변수를 매칭시켜주는 역할이다.  
`jsonplaceholder`의 JSON 데이터 일부는 아래와 같다.  
```json
    "userId": 1,
    "id": 1,
    "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
```
위 데이터처럼 `userId`, `title`처럼 Key 값이 설정되어 있는데 해당 값을 위 `DTO`클래스의 변수와 동일한 이름으로 짓고 `@SerializedName` 어노테이션을 명시하여 연결해주어야 한다.  

# 4. 원하는 액티비티에서 Retrofit 객체 선언
```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        userId = (EditText)findViewById(R.id.userId);
        userPassword = (EditText)findViewById(R.id.userPassword);
        loginButton = (Button)findViewById(R.id.loginButton);
        registerButton = (Button) findViewById(R.id.registerButton);

        Retrofit retrofit = new Retrofit.Builder()
                .baseUrl("https://jsonplaceholder.typicode.com/posts/")
                        .addConverterFactory(GsonConverterFactory.create())
                                .build();
        RetrofitAPI retrofitAPI = retrofit.create(RetrofitAPI.class);
        retrofitAPI.getData("1").enqueue(new Callback<List<Post>>(){
            @Override
            public void onResponse(Call<List<Post>> call, Response<List<Post>> response) {
                if(response.isSuccessful()){
                    List<Post> data = response.body();
                    Log.d("통신","성공");
                    Log.d("통신된 데이터 - userId : ", String.valueOf(data.get(0).getUserId()));
                    Log.d("통신된 데이터 - Id : ", String.valueOf(data.get(0).getId()));
                    Log.d("통신된 데이터 - title : ",data.get(0).getTitle());
                    Log.d("통신된 데이터 - body :",data.get(0).getBody());
                }
            }

            @Override
            public void onFailure(Call<List<Post>> call, Throwable t) {
                    Log.d("통신","실패");
                    t.printStackTrace();
            }
        });

        // 회원가입 버튼 클릭 이벤트
        registerButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent(MainActivity.this, RegisterPage.class);
                startActivity(intent);
            }
        });
    }
```
위 코드는 임시적으로 작성한 `MainActivity`의 일부이다.
```java
        Retrofit retrofit = new Retrofit.Builder()
                .baseUrl("https://jsonplaceholder.typicode.com/posts/")
                        .addConverterFactory(GsonConverterFactory.create())
                                .build();
```
여기서 `Retrofit` 객체를 생성한 뒤 `baseUrl`에 `jsonplaceholder` 서버에 접근하기 위한 URL을 적어주자. **이때, `baseUrl`에 작성하기 위한 URL 맨 끝에 `/`을 필수적으로 적어야 한다. 그렇지 않으면 예외가 발생하기에 꼭 `/`을 추가해주자.**  
또한, JSON을 `GSON`을 사용하여 변환하고자 하니 `addConverterFactory`에 `GsonConverterFactory` (Gson 컨버터)를 명시한다. 그 뒤 `.build()`를 통하여 `Retrofit`을 빌드한다.  
```java
        retrofitAPI.getData("1").enqueue(new Callback<List<Post>>(){
            @Override
            public void onResponse(Call<List<Post>> call, Response<List<Post>> response) {
                if(response.isSuccessful()){
                    List<Post> data = response.body();
                    Log.d("통신","성공");
                    Log.d("통신된 데이터 - userId : ", String.valueOf(data.get(0).getUserId()));
                    Log.d("통신된 데이터 - Id : ", String.valueOf(data.get(0).getId()));
                    Log.d("통신된 데이터 - title : ",data.get(0).getTitle());
                    Log.d("통신된 데이터 - body :",data.get(0).getBody());
                }
            }

            @Override
            public void onFailure(Call<List<Post>> call, Throwable t) {
                    Log.d("통신","실패");
                    t.printStackTrace();
            }
        });

        // 회원가입 버튼 클릭 이벤트
        registerButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent(MainActivity.this, RegisterPage.class);
                startActivity(intent);
            }
        });
```
해당 코드는 앞서 만든 인터페이스를 구현한다. 앞서 만든 인터페이스
```java
Call<List<Post>> getData(@Query("userId") String id);
```
에 `getData` 메소드가 있다. 해당 메소드에 찾고자 하는 JSON 데이터의 `id`값을 파라미터로 보낸 뒤 큐(`enqueue`)에 데이터를 삽입한다. 그 다음 `enqueue`에 오버라이딩 된 메소드 `onResponse`와 `onFailure`가 있다. `onResponse`는 성공적으로 통신이 응답이 되었다면 실행이 되며 `onFailure`는 통신에 실패했을 시 발생한다.  
```java
Log.d("통신","성공");
Log.d("통신된 데이터 - userId : ", String.valueOf(data.get(0).getUserId()));
Log.d("통신된 데이터 - Id : ", String.valueOf(data.get(0).getId()));
Log.d("통신된 데이터 - title : ",data.get(0).getTitle());
Log.d("통신된 데이터 - body :",data.get(0).getBody());
```
찾고자 하는 데이터는 JSON 데이터 중 id 1에 해당하는 데이터의 `userId`, `Id`, `title`, `body`이다. 아래의 JSON 파일과 통신 성공시 출력되는 `Log`의 출력이 동일하면 통신 성공이다.  
```json
  {
    "userId": 1,
    "id": 1,
    "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
    "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
  },
```
  
<p align="center"><img src="/assets/img/blog/정보/레트로핏 통신성공.png"></p>
  
**통신 성공!**
  
  
  
  
---
[출처 및 참고]  
[REST란?](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)  
[salix97](https://salix97.tistory.com/204)  
[galid1](https://galid1.tistory.com/m/617)  
[스틱코드](https://stickode.tistory.com/181)  
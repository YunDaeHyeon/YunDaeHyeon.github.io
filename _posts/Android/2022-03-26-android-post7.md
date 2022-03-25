---
layout: post
title:  "[Android] SharedPreferences를 이용한 앱 최초 실행 파악하기 + 카카오 로그인 전송 예시"
date:   2022-03-26 00:19:00+0900
categories: jekyll update
tags: [Android]
---
<p align="center"><img src="/assets/img/blog/정보/안드로이드.png"></p>

# 앱이 최초 실행했는지 확인하려면?
이런 경우에 사용되는 것이 `SharedPreferences`이다.
`SharedPreferences` 객체는 **Key-Vaule 쌍이 포함된 파일을 가리키며 해당 쌍을 읽고 쓸수 있는 간단한 메소드를 제공하는 API**이다. 아래의 메소드를 사용해서 간단하게 핸들을 가져올 수 있다!

```java
Context context = getActivity();
SharedPreferences sharedPref = context.getSharedPreferences(
        getString(R.string.preference_file_key), Context.MODE_PRIVATE);
```

쉽게 말하면 `SharedPreferences`는 안드로이드에서 DB를 사용하지 않고 어떠한 값을 저장시킬 때 사용한다. `SharedPreferences`는 **파일 형태로 저장되며 *앱을 삭제하기 전까지 유지가 되고, 앱을 삭제하거나 앱 내에서 쌍을 지우면 삭제되는*** 객체이다.  
이러한 특성을 이용하면 사용자가 **해당 어플리케이션을 설치 후 최초로 실행하였는지에 대한 여부를 파악할 수 있다.**  
  
만약, 앱이 최초로 실행했을 때의 `boolean` 변수가 `false`로 코드를 작성하여서 `if(변수 == false)`라는 조건이 성립하였을 시 앱이 최초로 실행되었다는 것이며 변수의 값을 `true`로 바꾸어주면 다음 앱 실행 때 위의 조건식은 건너뛰는 형태로 코드를 작성하면 된다.  

```java
    public void firstApplicationRoadCheck(){
        SharedPreferences pref = getSharedPreferences("CHECK", Activity.MODE_PRIVATE);
        boolean check = pref.getBoolean("CHECK", false);
        if(check==false){
            Log.d("앱 실행 판단", "최초로 실행");
            SharedPreferences.Editor editor = pref.edit();
            editor.putBoolean("CHECK",true);
            editor.commit();
            // commit() 아래에 앱이 최초에 실행되었을 때 동작되는 것들을 작성하면 된다.
        }else{
            Log.d("앱 실행 판단", "최초로 실행된 것이 아님.");
        }
    }
```
  
필자는 어플리케이션 공부를 할 때 사용자가 처음으로 로그인 할 시 DB로 로그인한 사용자의 정보들이 서버로 전달되어야 하는 경우이다. 사용자가 최초로 로그인을 수행하고, 로그인이 성공적으로 이루어졌을때 앱이 **최초로 실행되었을 경우에만 서버와의 통신을 `Retrofit`를 이용하여 전송되도록 하였다.** 또한, 로그인 방식은 많은 개발자들이 사용하는 카카오 API를 사용하였으며 `Retrofit`은 `싱글톤` 패턴을 사용하였다.  

```java
    public void firstApplicationRoadCheck(){
        SharedPreferences pref = getSharedPreferences("CHECK", Activity.MODE_PRIVATE);
        boolean check = pref.getBoolean("CHECK", false);
        if(check==false){
            Log.d("앱 실행 판단", "최초로 실행");
            SharedPreferences.Editor editor = pref.edit();
            editor.putBoolean("CHECK",true);
            editor.commit();
            UserApiClient.getInstance().me(new Function2<User, Throwable, Unit>() {
                @Override
                public Unit invoke(User user, Throwable throwable) {
                    // 로그인이 되어있으면
                    if (user!=null){
                        RetrofitClient retrofitClient = RetrofitClient.getInstance();
                        if(retrofitClient != null){
                            retrofitAPI = RetrofitClient.getRetrofitAPI();
                            retrofitAPI.sendUser(user.getKakaoAccount().getEmail(),user.getKakaoAccount().getProfile().getNickname()).enqueue(new Callback<Message>() {
                                @Override
                                public void onResponse(Call<Message> call, Response<Message> response) {
                                    if(response.isSuccessful()){
                                        final Message message = response.body();
                                        Toast.makeText(getApplicationContext(), "서버에 값을 전달하였습니다."+message.getMessage(), Toast.LENGTH_SHORT).show();
                                    }else{
                                        Log.d("오류 발생","onResponse 실패 ( 3xx, 4xx 오류)");
                                        Toast.makeText(getApplicationContext(), "onResponse 실패, 3xx, 4xx 오류", Toast.LENGTH_SHORT).show();
                                    }
                                }
                                @Override
                                public void onFailure(Call<Message> call, Throwable t) {
                                    t.printStackTrace();
                                    Toast.makeText(getApplicationContext(), "서버와 통신중 에러가 발생하였습니다.", Toast.LENGTH_SHORT).show();
                                }
                            });
                        }
                        Log.d("userEmail = ",userEmail);
                        Log.d("userNickname = ",userNickname);
                    }else {
                        Log.d("Error : ","userProfileRoad Error");
                    }
                    return null;
                }
            });
        }else{
            Log.d("앱 실행 판단", "최초로 실행된 것이 아님.");
        }
    }
```
  
  
  
---  
[참고]  
[Android 공식](https://developer.android.com/training/data-storage/shared-preferences?hl=ko)  
[kiwinam](https://kiwinam.com/posts/1/android-shared-preferences/)  
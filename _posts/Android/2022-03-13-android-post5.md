---
layout: post
title:  "[Android] Fragment를 유지시키고 싶을 때"
date:   2022-03-13 18:40:00+0900
categories: jekyll update
tags: [Android]
---
<p align="center"><img src="/assets/img/blog/정보/안드로이드.png"></p>

# Fragment에 있는 데이터 증발 현상

<p align="center"><img src="/assets/img/blog/안드로이드/프래그먼트 전.gif"></p>

위와 같이 프래그먼트에서 어떤 데이터를 뿌려놓고 다른 프래그먼트로 이동한 뒤 기존 프래그먼트로 이동하면 데이터가 날아가는 현상이 발생하였다.  
어떻게 해결할 수 있는 방법이 없을까 하고 코드를 살펴보았다.  

```java
        // 프래그먼트를 액티비티에 올리기 ( 초기화면 설정 - 계획(planFragment) )
        getSupportFragmentManager().beginTransaction().replace(R.id.containers,planFragment).commit();
        NavigationBarView navigationBarView = findViewById(R.id.bottom_navigationview);
        navigationBarView.setOnItemSelectedListener(new NavigationBarView.OnItemSelectedListener() {
            @Override
            public boolean onNavigationItemSelected(@NonNull MenuItem item) {
                switch(item.getItemId()){
                    case R.id.ic_repository: // 보관소 프래그먼트
                        getSupportFragmentManager().beginTransaction().replace(R.id.containers,repositoryFragment).commit();
                        return true;
                    case R.id.ic_planning: // 계획 프래그먼트
                        getSupportFragmentManager().beginTransaction().replace(R.id.containers,planFragment).commit();
                        return true;
                    case R.id.ic_profile: // 프로필 프래그먼트
                        getSupportFragmentManager().beginTransaction().replace(R.id.containers,profileFragment).commit();
                        return true;
                }
                return false;
            }
        });
```
기존의 코드는 `NavigationBarView`의 아이템 선택 리스너를 넣어 해당 아이템을 클릭할 때 프래그먼트가 바뀌도록 하였다. 여기서 `getSupportFragmentManager()`의 `replace()`는 해당 프래그먼트를 **다시 만드는** 용도이다. 그렇기 때문에 다른 프래그먼트로 넘어간 뒤 **기존 프래그먼트로 다시 올 시 프래그먼트는 *다시 그려지기* 때문에 데이터가 날아간다.** 이를 해결하기 위하여 `getSupportFragmentManager()`의 `add()`와 `hide()`, `show()`를 이용하였다.

```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        planFragment = new PlanFragment();
        // 프래그먼트를 액티비티에 올리기 ( 초기화면 설정 - 계획(planFragment) )
        getSupportFragmentManager().beginTransaction().replace(R.id.containers,planFragment).commit();
        NavigationBarView navigationBarView = findViewById(R.id.bottom_navigationview);
        navigationBarView.setOnItemSelectedListener(new NavigationBarView.OnItemSelectedListener() {
            @Override
            public boolean onNavigationItemSelected(@NonNull MenuItem item) {
                switch(item.getItemId()){
                    case R.id.ic_repository: // 보관소 프래그먼트
                        if(repositoryFragment == null){
                            repositoryFragment = new RepositoryFragment();
                            getSupportFragmentManager().beginTransaction().add(R.id.containers, repositoryFragment).commit();
                        }

                        if(repositoryFragment != null) getSupportFragmentManager().beginTransaction().show(repositoryFragment).commit();
                        if(planFragment != null) getSupportFragmentManager().beginTransaction().hide(planFragment).commit();
                        if(profileFragment != null) getSupportFragmentManager().beginTransaction().hide(profileFragment).commit();
                        return true;

                    case R.id.ic_planning: // 계획 프래그먼트
                        if(planFragment == null){
                            planFragment = new PlanFragment();
                            getSupportFragmentManager().beginTransaction().add(R.id.containers, planFragment).commit();
                        }

                        if(repositoryFragment != null) getSupportFragmentManager().beginTransaction().hide(repositoryFragment).commit();
                        if(planFragment != null) getSupportFragmentManager().beginTransaction().show(planFragment).commit();
                        if(profileFragment != null) getSupportFragmentManager().beginTransaction().hide(profileFragment).commit();
                        return true;
                    case R.id.ic_profile: // 프로필 프래그먼트
                        if(profileFragment == null){
                            profileFragment = new ProfileFragment();
                            getSupportFragmentManager().beginTransaction().add(R.id.containers, profileFragment).commit();
                        }

                        if(repositoryFragment != null) getSupportFragmentManager().beginTransaction().hide(repositoryFragment).commit();
                        if(planFragment != null) getSupportFragmentManager().beginTransaction().hide(planFragment).commit();
                        if(profileFragment != null) getSupportFragmentManager().beginTransaction().show(profileFragment).commit();
                        return true;
                }
                return false;
            }
        });
```

처음 어플리케이션이 실행될 때 화면에 보여질 프래그먼트만 `reaplce`를 사용하여 뿌려준 뒤 나머지 프래그먼트가 선택될 때 마다 `null`인지 아닌지를 판결하여 `null`이라면 해당 프래그먼트에 해당하는 객체를 생성한 뒤 `add()`를 이용하여 액티비티 위에 해당 프래그먼트를 뿌려주었다.  
`null`이 아니라면 기존에 프로그먼트가 있다는 의미이므로 선택된 프래그먼트만 `show`로 화면에 보여주고 나머지 프래그먼트는 모두 `hide`를 이용하여 숨기는 방식을 사용하였다.  

# 결과

<p align="center"><img src="/assets/img/blog/안드로이드/프래그먼트 후.gif"></p>


  
  
  
---  
  
**사용자가 뒤로 탐색하여 기존 프래그먼트를 복원시키는 경우 기존 프래그먼트를 다시 시작시키는 방식이 있다고 한다. `FragmentTransaction`을 커밋하기 전에 `addToBackStack()`을 호출시켜 기존 프래그먼트를 복원하는 방식을 주로 사용한다고 한다.**  
  
[안드로이드 공식 문서](https://developer.android.com/training/basics/fragments/fragment-ui?hl=ko)  
*프래그먼트 관리(Fragment Manager)에 대해 너무 잘 설명되어있는 블로그가 있다. 아래의 링크를 확인하자.*  
[kimtx200](https://tedrepository.tistory.com/6)  
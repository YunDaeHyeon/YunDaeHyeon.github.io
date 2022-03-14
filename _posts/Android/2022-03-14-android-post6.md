---
layout: post
title:  "[Android] Fragment위에 있는 RecyclerView 수정(메뉴 활용)"
date:   2022-03-14 20:26:00+0900
categories: jekyll update
tags: [Android]
---
<p align="center"><img src="/assets/img/blog/정보/안드로이드.png"></p>

# 메뉴를 활용한 RecyclerView 수정
Fragment 위에 구현된 RecyclerView에 대한 Item이 수정을 시키기 위하여 `OnCreateContextMenuListener`을 통한 메뉴 리스너를 지정시켜 다이얼로그를 띄우려고 한다.

우선, RecyclerView가 있는 프래그먼트의 Adapter을 바꾸자.
```java
# PlanFragment.java
// 리사이클러뷰 연결
planningCardViewAdapter = new PlanningCardViewAdapter(getActivity(), planningItemDTO);
recyclerView.setLayoutManager(new LinearLayoutManage(getActivity()));
recyclerView.setAdapter(planningCardViewAdapter);
```
`PlanningCardViewAdapter`의 생성자 매개변수의 `Context`를 보내준다. 이때, 보내게 되는 클래스는 **프래그먼트(Fragment)이기 때문에 꼭 `getActivity()`를 통한 Fragment의 `Context`를 보낸다.**  
보낸 `Context`를 활용하여 메뉴를 구현하기 위해 `PlanningCardViewAdapter`라는 `RecuclerView`의 `Adapter`로 이동한다.  

그 다음 메뉴를 클릭했을 때 화면에 뿌려지는 다이얼로그의 레이아웃을 구성하자.  

```xml
# fragment_planning_edit_dialog.xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:padding="10dp"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_gravity="bottom"
    android:background="@drawable/plan_dialog_round"
    android:orientation="vertical">

    <TextView
        android:id="@+id/edit_planning_time"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="16dp"
        android:hint="시간 설정"
        android:textSize="24dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <EditText
        android:id="@+id/edit_planning_contents"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:selectAllOnFocus="true"
        android:lines="5"
        android:gravity="top|left"
        android:minWidth="10.0dip"
        android:maxWidth="5.0dip"
        android:layout_margin="16dp"
        android:layout_marginTop="16dp"
        android:inputType="textMultiLine"
        android:scrollHorizontally="false"
        android:hint="계획"
        android:minHeight="48dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/edit_planning_time" />

    <Button
        android:id="@+id/planning_edit_btn"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="16dp"
        android:text="수정"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/edit_planning_contents" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

그 뒤 `Adapter`의 코드를 변경한다.  

```java
# PlanningCardViewAdapter.java
public class PlanningCardViewAdapter extends RecyclerView.Adapter<PlanningCardViewAdapter.PlanningCardViewHolder>{

    ArrayList<PlanningItemDTO> planningItemDTO;
    private TimePickerDialog timePickerDialog; // 타임피커 선언
    private Context context;

    // 생성자
    public PlanningCardViewAdapter(Context context, ArrayList<PlanningItemDTO> planningItems) {
        this.context = context;
        this.planningItemDTO = planningItems;
    }
    .
    .
    .
```
`Adapter` 생성자로 온 `Context`를 `this`를 사용하여 할당한다.  
  
**RecyclerView.ViewHoler**을 상속받은 `Adapter`의 내부 클래스 `Holder`에서 메뉴를 사용하기 위한 인터페이스 `OnCreateContextMenuListener`을 `implements` 해야한다.  

```java
# PlanningCardViewAdapter.java의 내부 클래스
    class PlanningCardViewHolder extends RecyclerView.ViewHolder implements View.OnCreateContextMenuListener{
        TextView plan_date, plan_contents;
        Button delete_plan_item_btn;
        public PlanningCardViewHolder(@NonNull View itemView) {
            super(itemView);
            this.plan_date = itemView.findViewById(R.id.plan_date);
            this.plan_contents = itemView.findViewById(R.id.plan_contents);
            this.delete_plan_item_btn = itemView.findViewById(R.id.delete_plan_item_btn);
            itemView.setOnCreateContextMenuListener(this); // 메뉴 클릭 리스너 연결
        }
```
그 뒤 `itemView`라는 `RecyclerView`의 `View` 객체 자체를 메뉴 클릭 리스너와 연결시킨다.  
그 다음 컨텍스트 메뉴를 생성시킬 필요가 있기에 `OnCreateContextMenuListener` 인터페이스에 있는 `onCreateContextMenu` 메소드를 오버라이딩하여 코드를 작성한다.  

```java
        @Override
        public void onCreateContextMenu(ContextMenu contextMenu, View view, ContextMenu.ContextMenuInfo contextMenuInfo) {
            // 컨텍스트 메뉴를 생성시킨 뒤 메뉴 항목 선택 시 호출되는 리스너 등록
            MenuItem Edit = contextMenu.add(Menu.NONE, 1001, 1, "수정");
            Edit.setOnMenuItemClickListener(onEditMenu);
        }
```
`onCreateContextMenu`는 컨텍스트 메뉴를 생성시키는 메소드이다. `add()`에 지정된 파라미터 `1001`이 `수정`이라는 메뉴의 `ID`가 될 것이다.  
**그 뒤 생성된 메뉴에 해당 항목을 선택할 시 호출되는 리스너를 등록한다.** 리스너를 등록하였으면 해당 메뉴를 클릭했을 때 발생되는 동작을 구현해야한다. 해당 동작을 구현한다.  

```java
        private final MenuItem.OnMenuItemClickListener onEditMenu = new MenuItem.OnMenuItemClickListener() {
            @Override
            public boolean onMenuItemClick(MenuItem menuItem) {
                switch(menuItem.getItemId()){
                    case 1001 : // 수정 항목 선택 시
                        AlertDialog.Builder builder = new AlertDialog.Builder(context);
                        View view = LayoutInflater.from(context).inflate(R.layout.fragment_planning_edit_dialog,null,false);
                        builder.setView(view);

                        final TextView edit_planning_time = (TextView) view.findViewById(R.id.edit_planning_time);
                        final EditText edit_planning_contents = (EditText) view.findViewById(R.id.edit_planning_contents);
                        final Button planning_edit_btn = (Button) view.findViewById(R.id.planning_edit_btn);

                        // 입력되어있던 데이터를 다이얼로그에 뿌려줌
                        edit_planning_time.setText(planningItemDTO.get(getAbsoluteAdapterPosition()).getDate());
                        edit_planning_contents.setText(planningItemDTO.get(getAbsoluteAdapterPosition()).getContents());

                        final AlertDialog dialog = builder.create();

                        // 시간 클릭 시
                        edit_planning_time.setOnClickListener(new View.OnClickListener() {
                            @Override
                            public void onClick(View view) {
                                final Calendar calendar = Calendar.getInstance();
                                int hour = calendar.get(Calendar.HOUR);
                                int minute = calendar.get(Calendar.MINUTE);
                                timePickerDialog = new TimePickerDialog(context, new TimePickerDialog.OnTimeSetListener() {
                                    @Override
                                    public void onTimeSet(TimePicker view, int hourOfDay, int minute) {
                                        edit_planning_time.setText(String.format("%02d시 %02d분",hourOfDay,minute));
                                    }
                                },hour,minute,false);
                                timePickerDialog.show();
                            }
                        });
                        // 수정버튼 클릭 시
                        planning_edit_btn.setOnClickListener(new View.OnClickListener() {
                            @Override
                            public void onClick(View view) {
                                String date = edit_planning_time.getText().toString();
                                String contents = edit_planning_contents.getText().toString();
                                PlanningItemDTO editItem = new PlanningItemDTO(date, contents);
                                planningItemDTO.set(getAbsoluteAdapterPosition(), editItem);
                                notifyItemChanged(getAbsoluteAdapterPosition());
                                dialog.dismiss();
                            }
                        });
                        dialog.show();
                        break;
                }
                return true;
            }
        };
```

`AlertDialog`를 활용하여 메뉴를 클릭했을 때 실행되는 다이얼로그가 출력되도록 하였다. 다이얼로그에 있는 수정 버튼을 클릭 시 사용자가 입력한 View에서 `getText()`를 통해 값을 가져와 `RecyclerView`와 연결된 `gettter, setter` 클래스에 저장함에 이어 `notifyItemChanged`를 통해 `RecyclerView`의 아이템을 수정시켰다. 그 뒤 `dismiss()`를 호출하여 다이얼로그를 종료한다.  
**위 코드에서 `RecyclerView`에 있는 아이템의 위치(Position)을 알아내기 위해 `getAbsoluteAdapterPosition()`을 사용하였는데 기존 `getAdapterPosition()`는 `deprecated` 되었기에 대체 메소드로 사용하였다.**  
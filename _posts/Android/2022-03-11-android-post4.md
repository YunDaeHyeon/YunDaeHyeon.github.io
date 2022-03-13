---
layout: post
title:  "[Android] RecyclerView의 OnBindViewHolder()에 대한 고찰"
date:   2022-03-11 17:24:00+0900
categories: jekyll update
tags: [Android]
---
<p align="center"><img src="/assets/img/blog/정보/안드로이드.png"></p>

# RecyclerView에 있는 개별 아이템의 클릭 이벤트를 수행시키고자 할 때...
안드로이드에서는 리스트뷰나 리사이클러뷰의 아이템을 클릭했을 때 원하는 이벤트를 수행시키고자 하면 `ClickListener`를 달아야 한다. 그렇다면 `OnBindViewHolder()`에 리스너를 달아야 할까?

## OnBindViewHolder()에 리스너를 달아보자.
```java
    @Override
    public void onBindViewHolder(@NonNull PlanningCardViewAdapter.PlanningCardViewHolder holder, int position) {
        holder.plan_date.setText(planningItemDTO.get(position).getDate());
        holder.plan_title.setText(planningItemDTO.get(position).getTitle());
        holder.plan_contents.setText(planningItemDTO.get(position).getContents());

        // 삭제 버튼 클릭 시 해당 위치의 아이템 Delete
        holder.delete_plan_item_btn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                AlertDialog.Builder builder = new AlertDialog.Builder(view.getContext());
                builder.setTitle("삭제");
                builder.setMessage("해당 항목을 삭제하시겠습니까?");

                // "예"를 클릭했을 시
                builder.setPositiveButton("예",
                        new DialogInterface.OnClickListener() {
                            @Override
                            public void onClick(DialogInterface dialogInterface, int i) {
                                planningItemDTO.remove(position);
                                notifyItemRemoved(position);
                                notifyItemRangeChanged(position, planningItemDTO.size());
                            }
                        });
                // "아니요" 클릭 시
                builder.setNegativeButton("아니요",
                        new DialogInterface.OnClickListener() {
                            @Override
                            public void onClick(DialogInterface dialogInterface, int i) {
                                dialogInterface.cancel();
                            }
                        });
                builder.show();
            }
        });
    }
```
위 코드에서 `delete_plan_item_btn`을 클릭했을 리사이클러뷰에 있는 아이템의 `position`의 값을 가져와 해당 아이템을 삭제한다. 즉, `onBindViewHolder`에 있는 `delete_plan_item_btn` 버튼에 리스너를 달았는데 .... 파라미터에 있는 `position`에 `Warning`이 발생하였다.


```console
Do not treat position as fixed; only user immediately and call holder.getAdapterPosition() to look it up later
RecyclerView will not call onBindViewHolder again when the position of the item changes in the data set unless the item itself is invalidated or the new position cannot be determined. For this reason, you should only use the position parameter while acquiring the related data item inside this method, and should not keep a copy of it. If you need the position of an item later on(e.g. in a click listener), use getAdapterPosition() which will have the updated adapter position.
# 이게 뭐임
```
이게 뭐지 .. 하고 `StackOverFlow`나 구글링을 열심히 해보았다. 그 결과 나는 `onBindViewHolder()`에 있는 버튼에 `OnClick()`을 사용하였다. 하지만, `onBindViewHolder()`이 호출되는 시점과  `OnClick()`이 호출되는 시점이 **다르기에 문제가 발생한다고 한다.** 즉, **리사이클러뷰에 있는 `position`을 고정된 값으로 여기지 말고 `(e.g. in a click listener)` 와 같은 클릭 리스너를 사용하여 `position` 값을 참조해서는 안된다고 한다.** 만약 `position`값이 필요하다면 **`holder.getAdapterPosition()` 함수를 사용하라고 한다.**  
오.. 새로운 정보를 또 얻게 되었다.  

# OnCreateViewHolder()에 리스너를 사용하자.
[안드로이드 문서](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)의 `ViewHolder Reference`를 살펴본 결과 `getAdapterPosition()`은 **ViewHolder`가 나타내는 항목의 위치를 리턴한다고 한다.** 이를 아용하여 `OnCreateViewHolder()`에 리스너를 구현시키면 될 것 같다.**

```java
    @NonNull
    @Override // 카드뷰 연결
    public PlanningCardViewAdapter.PlanningCardViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.planning_cardview_item,
                parent,false);
        // ViewHolder 객체 생성
        PlanningCardViewHolder planningCardViewHolder = new PlanningCardViewHolder(view);
        planningCardViewHolder.delete_plan_item_btn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                AlertDialog.Builder builder = new AlertDialog.Builder(view.getContext());
                builder.setTitle("삭제");
                builder.setMessage("해당 항목을 삭제하시겠습니까?");

                // "예"를 클릭했을 시
                builder.setPositiveButton("예",
                        new DialogInterface.OnClickListener() {
                            @Override
                            public void onClick(DialogInterface dialogInterface, int i) {
                                planningItemDTO.remove(planningCardViewHolder.getAdapterPosition());
                                notifyItemRemoved(planningCardViewHolder.getAdapterPosition());
                                notifyItemRangeChanged(planningCardViewHolder.getAdapterPosition(), planningItemDTO.size());
                            }
                        });
                // "아니요" 클릭 시
                builder.setNegativeButton("아니요",
                        new DialogInterface.OnClickListener() {
                            @Override
                            public void onClick(DialogInterface dialogInterface, int i) {
                                dialogInterface.cancel();
                            }
                        });
                builder.show();
            }
        });
        return planningCardViewHolder;
    }
```
이런식으로 `getAdapterPosition`을 사용했는데 ...

```console
'getAdapterPosition()' is deprecated 
# 아 진짜
```
deprecated 되었다고 한다.. 또 찾아보니 **getAdapterPosition() 메소드는 Adapter가 다른 Adapter를 중첩하는 경우 어느 adapter의 위치인지 혼란스럽기 때문이다.** 따라서 `getBindingAdapterPosition()` 혹은 `getAbsoluteAdapterPosition()`을 사용하여 다시 수정하였다. 이때, 두 개의 메서드는 차례대로 **Adater내의 위치를 반환하거나 RecyclerView에서 위치를 반환하는 역할을 수행한다.**

```java
builder.setPositiveButton("예",
        new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
                planningItemDTO.remove(planningCardViewHolder.getAbsoluteAdapterPosition());
                notifyItemRemoved(planningCardViewHolder.getAbsoluteAdapterPosition());
                notifyItemRangeChanged(planningCardViewHolder.getAbsoluteAdapterPosition(), planningItemDTO.size());
            }
        });
```
  

**해결 !**
---
layout: post
title: 키보드 내려갈 때 특정 처리 하기
description: "키보드 내려갈 때 특정 처리 하기"
modified: 2019-08-20
tags: [editText,keyboard]
categories: [android]
---

### 키보드 내려갈 때 포커스 클리어 처리 하기

키보드 보여주기 및 포커스 , 숨기기 및 포커스 클리어 function 

```

 public static void showKeyboard(EditText editText) {
        if (editText == null) {
            return;
        }
        editText.requestFocus();
        mInputMethodManager.showSoftInput(editText, InputMethodManager.SHOW_FORCED | InputMethodManager.HIDE_IMPLICIT_ONLY);
 }

 public static void clearFocus(Activity activity) {
        View v = activity.getCurrentFocus();
        if (v == null) {
            return;
        }
        InputMethodManager imm = (InputMethodManager) activity.getSystemService(Context.INPUT_METHOD_SERVICE);
        imm.hideSoftInputFromWindow(v.getWindowToken(), 0);
        v.clearFocus();
        activity.getWindow().setSoftInputMode(WindowManager.LayoutParams.SOFT_INPUT_STATE_ALWAYS_HIDDEN);
 }

```

키보드 내려갈 때 감지 해서 특정한 처리가 필요 했다.  
editText에서 지원 되는 키보드 감지 리스너는 보이지 않아서 따로 구현이 필요 했다.

```
  int lastHeightDiff = 0;
    boolean isOpenKeyboard = false;
    private final ViewTreeObserver.OnGlobalLayoutListener mOnGlobalLayoutListener = new ViewTreeObserver.OnGlobalLayoutListener() {
        @Override
        synchronized public void onGlobalLayout() {
            View activityRootView = mActivity.getWindow().getDecorView().findViewById(android.R.id.content);
            int heightDiff = activityRootView.getRootView().getHeight() - activityRootView.getHeight();
            if (lastHeightDiff == 0) {
                lastHeightDiff = heightDiff;
            }
            if (heightDiff > lastHeightDiff) { //keyboard show
                isOpenKeyboard = true;
            } else {//keyboard hide
                if (isOpenKeyboard) {
                    clearFocus();
                    isOpenKeyboard = false;
                }
            }
        }
    };
```

해당 리스너는 onCreate 시 등록 onDestroy 해제 시켜줘야 한다.  
한번 불리는게 아니라 여러번 불리기 때문에 isOpenKeyboard 를 사용해서 clearFocus 처리를 해주었다.
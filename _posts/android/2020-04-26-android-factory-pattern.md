---
layout: post
title:  Factory Pattern (디자인 패턴 4장)
description: "Factory Pattern (디자인 패턴 4장)"
modified: 2020-04-26
tags: [Factory Pattern]
categories: [java,kotlin]
---

### 팩토리 메소드 패턴과 추상 팩토리 패턴

#### 팩토리 메소드 정의
객체를 생성하기 위한 인터페이스를 정의하는데, 어떤 클래스의 인스턴스를 만들지는 서브클래스에서 결정하게 한다.

```java
class FactoryMethodPatternTest {
    @Test
    fun testCreateModel() {
        val yellowMemo = ClassFactory().create("Yellow")
        yellowMemo.onClick()
        val blueMemo = ClassFactory().create("Blue")
        blueMemo.onClick()
        val empty = ClassFactory().create("Else")
        empty.onClick()
    }

    abstract class ModelFactory {
        abstract fun create(className: String): Model
    }

    class ClassFactory : ModelFactory() {
        override fun create(className: String): Model {
            return when (className.toLowerCase()) {
                "yellow" -> {
                    YellowMemo()
                }
                "blue" -> {
                    BlueMemo()
                }
                else -> {
                    Empty()
                }
            }
        }
    }

    abstract class Model {
        abstract fun onClick()
    }

    class YellowMemo : Model() {
        override fun onClick() {
            println("YellowMemo 클릭")
        }
    }

    class BlueMemo : Model() {
        override fun onClick() {
            println("BlueMemo 클릭")
        }
    }

    class Empty : Model() {
        override fun onClick() {
            println("Empty 클릭")
        }
    }
}


```

위의 코드 테스트 결과이미지
<figure>
	<img src="/images/2020-04-26-android-factory-pattern-01.png" alt="">
</figure>

추상클래스 ModelFactory 의 create fun 추상 메소드를 통해 모델을 만들고 각각을 클릭 했을 때 print 내용은 달라진다.  
모델을 직접 만들지 않고 Factory 를 통해 만들어서 사용하였다.  
하지만 이렇게 되면 create 될때 className 에 의해 모두 모델을 ClassFactory에서 만들어 주어야 한다.  
모델이 셀수 없게 많아지고 해당 모델들을 관리 해야 한다면??  
모든 모델들을 ClassFactory에 추가 해야하기 때문에 비효율적이다.  
위의 문제점을 보완한 아래의 내용을 보자.  

#### 추상팩토리 정의
추상 팩토리 패턴에서는 인터페이스를 이용하여 서로 연관된 또는 의존하는 객체인 구상 클래스를 지정하지 않고도 생성할 수 있다.  
Android Architecture Component 에 있는 ViewModel 을 들여다 보자.  

```java
val viewModel = ViewModelProviders.of(this).get(MemoViewModel::class.java)
```

ViewModelProviders.of(this) 는 ViewModelProvider 를 반환 하고 get(MemoViewModel::class.java)을 통해 MemoViewModel 을 반환한다.  
ViewModelProvider를 자세히 살펴 보자.  

```java
public class ViewModelProvider {

@NonNull
@MainThread
public <T extends ViewModel> T get(@NonNull Class<T> modelClass) {
    String canonicalName = modelClass.getCanonicalName();
    if (canonicalName == null) {
        throw new IllegalArgumentException("Local and anonymous classes can not be ViewModels");
    }
    return get(DEFAULT_KEY + ":" + canonicalName, modelClass);
}

@NonNull
@MainThread
public <T extends ViewModel> T get(@NonNull String key, @NonNull Class<T> modelClass) {
    ViewModel viewModel = mViewModelStore.get(key);

    if (modelClass.isInstance(viewModel)) {
        return (T) viewModel;
    } else {
        if (viewModel != null) {
            }
        }
        if (mFactory instanceof KeyedFactory) {
            viewModel = ((KeyedFactory) (mFactory)).create(key, modelClass);
        } else {
            viewModel = (mFactory).create(modelClass);
        }
        mViewModelStore.put(key, viewModel);
        return (T) viewModel;
    }

abstract static class KeyedFactory implements Factory {
    @NonNull
    public abstract <T extends ViewModel> T create(@NonNull String key,
            @NonNull Class<T> modelClass);

    @NonNull
    @Override
    public <T extends ViewModel> T create(@NonNull Class<T> modelClass) {
        throw new UnsupportedOperationException("create(String, Class<?>) must be called on "
                + "implementaions of KeyedFactory");
    }
}

public static class AndroidViewModelFactory extends ViewModelProvider.NewInstanceFactory {

    private static AndroidViewModelFactory sInstance;

    @NonNull
    public static AndroidViewModelFactory getInstance(@NonNull Application application) {
        if (sInstance == null) {
            sInstance = new AndroidViewModelFactory(application);
        }
        return sInstance;
    }

    private Application mApplication;

    public AndroidViewModelFactory(@NonNull Application application) {
        mApplication = application;
    }

    @NonNull
    @Override
    public <T extends ViewModel> T create(@NonNull Class<T> modelClass) {
        if (AndroidViewModel.class.isAssignableFrom(modelClass)) {
            try {
                return modelClass.getConstructor(Application.class).newInstance(mApplication);
            } catch (NoSuchMethodException e) {
                throw new RuntimeException("Cannot create an instance of " + modelClass, e);
            } catch (IllegalAccessException e) {
                throw new RuntimeException("Cannot create an instance of " + modelClass, e);
            } catch (InstantiationException e) {
                throw new RuntimeException("Cannot create an instance of " + modelClass, e);
            } catch (InvocationTargetException e) {
                throw new RuntimeException("Cannot create an instance of " + modelClass, e);
            }
        }
        return super.create(modelClass);
    }
}	

public static class NewInstanceFactory implements Factory {

    @SuppressWarnings("ClassNewInstance")
    @NonNull
    @Override
    public <T extends ViewModel> T create(@NonNull Class<T> modelClass) {
        try {
            return modelClass.newInstance();
        } catch (InstantiationException e) {
            throw new RuntimeException("Cannot create an instance of " + modelClass, e);
        } catch (IllegalAccessException e) {
            throw new RuntimeException("Cannot create an instance of " + modelClass, e);
        }
    }
}

public interface Factory {
    @NonNull
    <T extends ViewModel> T create(@NonNull Class<T> modelClass);
}

}
```

소스가 좀 길지만 필요한 부분만 짤랐다.  
mFactory 는 AndroidViewModelFactory 이다.  
Factory 라는 인터페이스로 인해 구상 클래스를 지정하지 않고도 생성할 수 있다.  
mViewModelStore 에 viewModel 이 있으면 해당 viewModel 을 반환하고 아니면 Factory에 의해 생성되게 된다.  

간단한 로직의 객체 생성이라면 팩토리 메소드를 사용해도 괜찮을것 같고 Android Architecture Component 처럼 구상 클래스를 지정하고 싶지 않을때는
추상 팩토리 패턴을 사용하면 좋을것 같다.  
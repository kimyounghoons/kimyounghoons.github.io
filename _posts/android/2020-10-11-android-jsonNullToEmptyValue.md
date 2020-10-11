---
layout: post
title:  Json field null 에서 empty 값 변경
description: Json field null 에서 empty 값 변경
modified: 2020-10-11
tags: [Android, Kotlin]
categories: [Android, Kotlin]
---

서버와 데이터를 주고 받을 때 Json 형식으로 주고 받는데 서버에서 내려주는 데이터 field 가 가끔씩 의도하지 않은 null 값이 내려올 때가 있다.  
Kotlin 언어를 사용할 때 nullable 타입인지 아닌지 구분해서 필드를 만들 수 있는데 이 경우는 예상하지 못하기 때문에 무조건 nullable 형태로 사용해야 한다.  
이렇게 사용하면 뭔가 깔끔하지 못한 느낌이 들어서 다른 해결 방안을 생각해 보았다.  

이 문제를 해결한 방법은 gson 생성 시 TypeAdapter 를 커스텀해서 사용하는 방법이었다.

우선 array 배열에 [null] 형식으로 오는 값과 필드(Boolean , Long , String)가 null 로 오는 두 케이스를 다뤘다.

```java
class GsonHelperTest {
    @Test
    fun `Null to Empty Value`() {
        val jsonString = "{\n" +
                "\"stringValue\": null,\n" +
                "\"timestamp\": \"1602158882721\",\n" +
                "\"longValue\": null,\n" +
                "\"intValue\": null,\n" +
                "\"booleanValue\": null,\n" +
                "\"items\": [null]\n" +
                "    }"

        val gson = GsonBuilder()
                .registerTypeAdapter(ArrayList::class.java, NonNullListDeserializer<Any>())
                .registerTypeAdapter(String::class.java, StringTypeAdapter())
                .registerTypeAdapter(Long::class.java, LongTypeAdapter())
                .registerTypeAdapter(Boolean::class.java, BooleanTypeAdapter())
                .disableHtmlEscaping()
                .create()

        val nonNullData: NonNullData = gson.fromJson(jsonString, NonNullData::class.java)

        println(nonNullData.stringValue.isEmpty())
        println(nonNullData.timestamp)
        println(nonNullData.items.size)
        println(nonNullData.longValue)
        println(nonNullData.intValue)
        println(nonNullData.booleanValue)

        assert(
                nonNullData.stringValue.isEmpty()
        )
    }

    data class NonNullData(
            @SerializedName("stringValue")
            var stringValue: String = "",
            @SerializedName("timestamp")
            var timestamp: String = "",
            @SerializedName("items")
            var items: ArrayList<Long> = arrayListOf(),
            @SerializedName("longValue")
            var longValue: Long = -1L,
            @SerializedName("intValue")
            var intValue: Int = -1,
            @SerializedName("booleanValue")
            var booleanValue: Boolean = true
    )
}

class NonNullListDeserializer<T> : JsonDeserializer<ArrayList<T>> {
    @Throws(JsonParseException::class)
    override fun deserialize(json: JsonElement, typeOfT: Type, context: JsonDeserializationContext): ArrayList<T> {
        if (json is JsonArray) {
            val array = json
            val size = array.size()
            if (size == 0) {
                return arrayListOf()
            }
            val list: ArrayList<T> = ArrayList(size)
            for (i in 0 until size) {
                val elementType: Type = `$Gson$Types`.getCollectionElementType(typeOfT, ArrayList::class.java)
                val value: T = context.deserialize(array[i], elementType)
                if (value != null) {
                    list.add(value)
                }
            }
            return list
        }
        return arrayListOf()
    }
}


class StringTypeAdapter : TypeAdapter<String>() {
    override fun write(out: JsonWriter, value: String?) {
    }

    override fun read(`in`: JsonReader): String {
        if (`in`.peek() === JsonToken.NULL) {
            `in`.nextNull()
            return ""
        }
        return `in`.nextString()
    }

}

Booelan, Long Type Adapter 는 같은 구조라 생략
```

NonNullListDeserializer 를 사용해서 items: [null] 식으로 오는 데이터를 items [] 로 변환 할 수 있고 StringTypeAdapter 를 사용해서 String 으로 보내주는 경우인데 null 값으로 보내주는 경우 null 값이 아닌 "" 빈값 으로 변경 시켜 줄 수 있다.

아래는 테스트 결과 이다.

<figure>
	<img src="/images/2020-10-11-android-json-response.png" alt="">
</figure>

Retrofit 에 적용 시키는 경우에는 아래와 같이 적용하면 된다.

```java
    private fun createRetrofitClient(okHttpClient: OkHttpClient): Retrofit {
        val builder = Retrofit.Builder()
                .baseUrl(baseUrl)
                .addCallAdapterFactory(rxJava2CallAdapterFactory)
                .addConverterFactory(GsonConverterFactory.create(
                        GsonBuilder().registerTypeAdapter(ArrayList::class.java, NonNullListDeserializer<Any>())
                                .registerTypeAdapter(String::class.java, StringTypeAdapter())
                                .registerTypeAdapter(Long::class.java, LongTypeAdapter())
                                .registerTypeAdapter(Boolean::class.java, BooleanTypeAdapter())
                                .disableHtmlEscaping()
                                .create())
                )
                .addConverterFactory(enumConverterFactory)
        return builder.client(okHttpClient).build()
    }
```

위와 같이 사용하게 되면 데이터 모델을 만들때 NonNull 타입으로 만들어도 해당 필드나 List 가 null 이 되지 않는걸 신뢰할 수 있다.
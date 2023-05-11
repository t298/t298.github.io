---

layout:     post   				    		# 使用的布局（不需要改）
title:      使用JacksonConfig处理id的时候只处理16位以上的id		# 标题 
subtitle:  									# 副标题
date:       2023-05-11						# 时间
author:     t298							# 作者
header-img: img/fj.jpg					#这篇文章标题背景图片
catalog: 	true 								# 是否归档
tags:										#标签

    - bug

---

我们在使用`JacksonConfig`来处理雪花id过长时会发现将所有的id都转换成了string，所以我们需要对id进行一个处理

```java
JacksonConfig:
    
@Configuration
public class JacksonConfig {
    @Bean
    public MappingJackson2HttpMessageConverter jackson2HttpMessageConverter() {
        final Jackson2ObjectMapperBuilder builder = new Jackson2ObjectMapperBuilder();
        builder.serializationInclusion(JsonInclude.Include.NON_NULL);
        final ObjectMapper objectMapper = builder.build();
        SimpleModule simpleModule = new SimpleModule();
        // Long 转为 String 防止 js 丢失精度
        simpleModule.addSerializer(Long.class, new LongSerializer());
        objectMapper.registerModule(simpleModule);
        // 忽略 transient 关键词属性
        objectMapper.configure(MapperFeature.PROPAGATE_TRANSIENT_MARKER, true);
        // 时区设置
        objectMapper.setTimeZone(TimeZone.getTimeZone("GMT+8"));
//        objectMapper.setDateFormat(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"));
        return new MappingJackson2HttpMessageConverter(objectMapper);
    }
}
```

```java
同级创建LongSerializer:

// 返回给前端的超过16位Long类型id会被转换成String
public class LongSerializer extends JsonSerializer<Long> {
    @Override
    public void serialize(Long value, JsonGenerator gen, SerializerProvider serializers) throws IOException {
        if (value != null && value.toString().length() >= 16) {
            gen.writeString(value.toString());
        } else {
            gen.writeNumber(value);
        }
    }
}

```


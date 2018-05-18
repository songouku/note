自定义注解
===
1、利用类的反射，获取注解
***
注解定义
```java
package com.wbx.cms;

/**
 * @author wangbaixing
 * @date 2018/4/11
 */
public class User {

    @CmsLogTest(value = "2333")
    private String name;
}

```
注解拦截
```java
package com.wbx.cms;

import java.lang.annotation.*;

/**
 * @author wangbaixing
 * @date 2018/4/11
 */
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
public @interface CmsLogTest {
    String value() default "1";
}

```
```java
package com.wbx.cms;


import com.google.common.collect.Lists;
import lombok.extern.slf4j.Slf4j;
import org.junit.Test;

import java.lang.annotation.Annotation;
import java.lang.reflect.Field;

/**
 * @author wangbaixing
 * 2017/11/12 0012
 */
@Slf4j
public class TestCase {

    @Test
    public void testGen() {
        Field[] declaredFields = User.class.getDeclaredFields();
        for (Field declaredField : declaredFields) {
            if(declaredField.isAnnotationPresent(CmsLogTest.class)){
                CmsLogTest annotation = declaredField.getAnnotation(CmsLogTest.class);
                System.out.println(annotation.value());
            }
        }
    }
}

```

2、spring aop 切面注解
***
注解定义
```java
package com.wbx.cms.config.aop;


import java.lang.annotation.*;

/**
 * @author wangbaixing
 * @date 2018-04-01
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
public @interface CmsLog {
    String params() default "";

    String path() default "";
}

```
注解拦截
```java
package com.wbx.cms.config.aspect;

import cn.hutool.core.convert.Convert;
import com.alibaba.fastjson.JSONObject;
import com.bx.common.entity.system.SysLog;
import com.google.common.collect.Lists;
import com.wbx.cms.config.aop.CmsLog;
import com.wbx.cms.service.system.SysLogService;
import jodd.datetime.JDateTime;
import lombok.extern.slf4j.Slf4j;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.subject.Subject;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import java.util.ArrayList;

/**
 * @author wangbaixing
 * @date 2018-04-01
 */
@Aspect
@Component
@Slf4j
public class CmsLogAspect {

    @Autowired
    private SysLogService sysLogService;

    /**
     * 切入点
     */
    @Pointcut("execution(* com.wbx.cms..*(..)) && @annotation(com.wbx.cms.config.aop.CmsLog)")
    private void pointcut() {

    }

    /**
     * 在方法执行前后
     *
     * @param point
     * @param cmsLog
     * @return
     */
    @Around(value = "pointcut() && @annotation(cmsLog)")
    public Object around(ProceedingJoinPoint point, CmsLog cmsLog) {
        Subject subject = SecurityUtils.getSubject();
        ArrayList<String> params = Lists.newArrayList(cmsLog.params().split(","));
        JSONObject param = new JSONObject();
        for (int i = 0; i < point.getArgs().length; i++) {
            param.put(Convert.toStr(point.getArgs()[i]), params.get(i));
        }
        SysLog sysLog = new SysLog();
        sysLog.setRequestUrl(cmsLog.path());
        sysLog.setContent(param.toJSONString());
        JSONObject user = JSONObject.parseObject(subject.getPrincipals().toString());
        sysLog.setUserName(user.getString("userName"));
        sysLog.setCreateTime(new JDateTime().convertToDate());
        sysLogService.saveLog(sysLog);
        try {
            //执行程序
            return point.proceed();
        } catch (Throwable throwable) {
            log.error(throwable.getMessage());
        }
        return null;
    }

}

```

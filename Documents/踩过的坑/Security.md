- springboot版本2.3.4.RELEASE开始

- ~~NoOpPasswordEncoder~~已删除，无法使用

**原本:**

```java
@Configuration
public class WebSecurityConfig extends WebSecurityConfigurerAdapter{
  /** <p>创建"密码加密器"对象交给Spring框架进行管理!</p> ...*/
  @Bean
  public PasswordEncoder passwordEncoder(){
    return NoOpPasswordEncoder.getInstance();
  }
  
  @Override
  protected void configure(AuthenticationManagerBuilder auth) throws Exception {
    // 在内存中授权
    auth.inMemoryAuthentication()
      	// 指定用户名
      	.withUser("root")
        // 指定密码
        .password("1234")
        // 指定授权
        .authorities("/admin/list");
    }
}
```

**解决办法:**

- 添加`.password("{noop}password")`到安全性配置文件

```java
@Configuration
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        // 在内存中授权
        auth.inMemoryAuthentication()
                // 指定用户名
                .withUser("root")
                // 指定密码
                .password("{noop}1234")
                // 指定授权
                .authorities("/admin/list");
    }
}
```


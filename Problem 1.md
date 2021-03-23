Problem1:

```java
package com.albert.test.Controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

@RestController
public class Conroller {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    @GetMapping("insert/{str}")
    public String insert(@PathVariable("str") String s) throws ClassNotFoundException, SQLException {
        int[][] dp = new int[s.length()][s.length()];
        int max = 0;
        int gl = -1;
        int gr = -1;
        for (int i=s.length()-1; i >= 0; i--){
            for (int j=i; j<s.length(); j++){
                if (i == j){
                    dp[i][j] = 1;
                } else if(i + 1 == j){
                    dp[i][j] = s.charAt(i) == s.charAt(j)? 2 : 0;
                } else{
                    dp[i][j] = dp[i+1][j-1] != 0 && s.charAt(i) == s.charAt(j)? dp[i+1][j-1] + 2 : 0;
                }
                if (max < dp[i][j]){
                    max = dp[i][j];
                    gl = i;
                    gr = j;
                }
            }
        }
        String newStr = s.substring(gl, gr+1);

        String sql = "insert into mysql_datasource ('number', 'string') values (?, ?)";
        if (jdbcTemplate.update(sql, 1, newStr) <= 0){
            return "insertion failed!";
        }
        return "insertion success!";
    }

    @GetMapping("/query/{num}")
    public String query(@PathVariable("num") int num){
        String sql = "select * from user where name = ?";

        String str = (String) jdbcTemplate.queryForObject(sql, String.class);
        return str;
    }
}

/*Configure the application.properties and add following content:

spring.datasource.url=jdbc:mysql://127.0.0.1:3306/springbootdemo
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.driver-class-name=com.mysql.jdbc.Driver*/

```




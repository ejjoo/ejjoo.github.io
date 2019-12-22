# 개념
웹 페이지를 동적으로 생성하기 위한 자바 EE 스펙으로, Server Applet의 줄여 Servlet이라고 한다. 동적으로 요청을 처리한다는 점에서 JSP와 비슷하지만, JSP는 HTML 코드안에 자바 코드를 포함하는 반면에 서블릿은 자바 코드 안에 HTML을 포함한다는 점에서 차이가 있다. 사실상 JSP도 Servlet으로 컴파일되기 때문에 근본적으로 둘은 동일하다.

서블릿이 동작하기 위해서는 클라이언트로 부터 요청을 받아 이를 적절한 서블릿으로 전달해주는 객체가 필요한데, 이런 역할을 하는 객체를 서블릿 컨테이너라고 한다. 서블릿 컨테이너는 요청을 받으면 요청을 처리하기 위한 스레드를 생성한 뒤 서블릿에 HttpServletXXX객체를 전달하고, 다시 서블릿에서 처리한 결과를 클라이언트에 전달한다.

서블릿은 3.0을 기준으로 많은 변화가 일어났는데, 가장 큰 변화는 설정 파일없이 서블릿을 작성할 수 있게 되었다는 점이다. 

Sequence
클라이언트로부터 요청이 들어오면 요청을 처리하기 위한 HttpServletRequest, HttpServletResponse 객체를 생성하고, 서블릿을 분석해 요청을 처리하기 위한 서블릿의 service 메소드를 호출하고, http 메소드에 따라 doGet, doPost를 호출한다. doXXX메소드는 동적 페이지를 생성하고, HttpServletResponse객체를 통해 응답을 보낸다. 응답이 완료되면 두 서블릿 요청, 응답 객체는 소멸한다.

서블릿 매핑을 위한 xml은 다음 예시와 같다.
```
<web-app...>
	<servlet>
		<servlet-name>some servlet</servlet-name>
		<servlet-class>some.class</servlet-class>
		<url-pattern>/some</url-pattern>
 	</servlet>
</web-app>
```

```
import java.io.IOException;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class ServletLifeCycleExample extends HttpServlet {
    @Override
    public void init(ServletConfig config) throws ServletException {
        super.init(config);
    }

    //이 경우 doGet, doPost는 동작하지 않는다. 
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.getWriter().write("hello");
    }

    @Override
    public void destroy() {
        getServletContext().log("destroy() called");
    }

}
```

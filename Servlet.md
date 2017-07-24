# Filter Interface in servlet
** A filter is an object that is invoked at the preprocessing and postprocessing of a request.
**Filter API

Like servlet filter have its own API. The javax.servlet package contains the three interfaces of Filter API.

1.Filter
2.FilterChain
3.FilterConfig

** Filter interface

For creating any filter, you must implement the Filter interface. Filter interface provides the life cycle methods for a filter.

 **FilterChain interface

The object of FilterChain is responsible to invoke the next filter or resource in the chain.This object is passed in the doFilter method of Filter interface.The FilterChain interface contains only one method:

1.public void doFilter(HttpServletRequest request, HttpServletResponse response): it passes the control to the next filter or resource.


# Example programs
```
# MyFilter.java

import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.Filter.*;
import javax.servlet.annotation.WebFilter;
@WebFilter(filterName="MyFilter", urlPatterns="/First")
 public class MyFilter implements Filter {
   
    public void init(FilterConfig fc) throws ServletException {}
    
    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException {
        PrintWriter out = response.getWriter();
        String pass = request.getParameter("pass");
        if(pass.equals("1234"))
        {
         chain.doFilter(request, response);   
        }
        else
        {
            out.println("You have enter a wrong password");
            RequestDispatcher rs = request.getRequestDispatcher("index.jsp");
            rs.include(request, response);
        }
    }
   public void destroy() { }
}

```
```
# index.jsp
<form method="post" action="First">
    Name:<input type="text" name="user" /><br/>
    Password:<input type="password" name="pass" /><br/>
    <input type="submit" value="submit" />
</form>
```
```
# First.java
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.Filter;
import javax.servlet.annotation.WebServlet;
@WebServlet(name="First", urlPatterns="/First")

 public class First extends HttpServlet {

 public void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException 
     {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        String user = request.getParameter("user");
        out.println("Wellcome "+user);
     }
}
```
# Session Tracking
** Session simply means a particular interval of time.
Session Tracking is a way to maintain state (data) of an user. It is also known as session management in servlet.**

# Why use Session Tracking?
** To recognize the user It is used to recognize the particular user.**

# Session Tracking Techniques:
** There are four techniques used in Session tracking:

1.Cookies
2.Hidden Form Field
3.URL Rewriting
4.HttpSession **

# Cookies in Servlet
** javax.servlet.http.Cookie class provides the functionality of using cookies. It provides a lot of useful methods for cookies.
 A cookie is a small piece of information that is persisted between the multiple client requests.
A cookie has a name, a single value, and optional attributes such as a comment, path and domain qualifiers, a maximum age, and a version number.**
# How Cookie works?
** By default, each request is considered as a new request. In cookies technique, we add cookie with response from the servlet. So cookie is stored in the cache of the browser. After that if request is sent by the user, cookie is added with request by default. Thus, we recognize the user as the old user.**
# Types of Cookie
** There are 2 types of cookies in servlets.
1.Non-persistent cookie
2.Persistent cookie **
# Non-persistent cookie
** It is valid for single session only. It is removed each time when user closes the browser.**
# Persistent cookie
** It is valid for multiple session . It is not removed each time when user closes the browser. It is removed only if user logout or signout.**
# Advantage of Cookies:
**1.Simplest technique of maintaining the state.
2.Cookies are maintained at client side.**
# Disadvantage of Cookies:
**1.It will not work if cookie is disabled from the browser.
2.Only textual information can be set in Cookie object.**
#How to create Cookie?
 ```
Cookie ck=new Cookie("user","sonoo jaiswal");
response.addCookie(ck);  

```
# How to delete Cookie?
```
Cookie ck=new Cookie("user","");  
ck.setMaxAge(0);
response.addCookie(ck); 

```
# How to get Cookies?
```
Cookie ck[]=request.getCookies();  
for(int i=0;i<ck.length;i++){  
 out.print("<br>"+ck[i].getName()+" "+ck[i].getValue());
}  

```
# Hidden Form Field:
** In case of Hidden Form Field a hidden (invisible) textfield is used for maintaining the state of an user.

In such case, we store the information in the hidden field and get it from another servlet. This approach is better if we have to submit form in all the pages and we don't want to depend on the browser.

Let's see the code to store value in hidden field.
 <input type="hidden" name="uname" value="Vimal Jaiswal"> **
# application of hidden form field
 **It is widely used in comment form of a website. 
 In such case, we store page id or page name in the hidden field so that each page can be uniquely identified.**
# Advantage of Hidden Form Field
 ** 1.It will always work whether cookie is disabled or not.**
# Disadvantage of Hidden Form Field:
**
1.It is maintained at server side.
2.Extra form submission is required on each pages.
3.Only textual information can be used.**
# URL Rewriting:
**In URL rewriting, we append a token or identifier to the URL of the next Servlet or the next resource. We can send parameter name/value pairs using the following format:

url?name1=value1&name2=value2&??

A name and a value is separated using an equal = sign, a parameter name/value pair is separated from another parameter using the ampersand(&). When the user clicks the hyperlink, the parameter name/value pairs will be passed to the server. From a Servlet, we can use getParameter() method to obtain a parameter value.**
# Advantage of URL Rewriting
**
1.It will always work whether cookie is disabled or not (browser independent).
2.Extra form submission is not required on each pages.**
# Disadvantage of URL Rewriting
**
1.It will work only with links.
2.It can send Only textual information.**
# HttpSession interface:
**In such case, container creates a session id for each user.The container uses this id to identify the particular user.An object of HttpSession can be used to perform two tasks:
bind objects view and manipulate information about a session, such as the session identifier, creation time, and last accessed time.**
# How to get the HttpSession object ?

** The HttpServletRequest interface provides two methods to get the object of HttpSession:

public HttpSession getSession():Returns the current session associated with this request, or if the request does not have a session, creates one.
public HttpSession getSession(boolean create):Returns the current HttpSession associated with this request or, if there is no current session and create is true, returns a new session.**
# Example program
```
<form method="post" action="Login">
  User: <input type="text" name="username" /><br/>
  Password: <input type="text" name="password" ><br/>
  <input type="submit" value="submit">
</form>
```
```
import java.io.*;  
import javax.servlet.*;  
import javax.servlet.http.*;  
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.annotation.WebServlet;  
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
@WebServlet("/Login")
public class Login extends HttpServlet {  

public void doPost(HttpServletRequest request, HttpServletResponse response)  
        throws ServletException, IOException {  
            try{
    response.setContentType("text/html");
        PrintWriter out = response.getWriter();  
        String name = request.getParameter("username");
        String pass = request.getParameter("password");
        
        if(pass.equals("1234"))
        {
            HttpSession session = request.getSession();
            session.setAttribute("hello",name);
            response.sendRedirect("Wel");
            out.close();
        }

    else
    {  
        response.setContentType("text/html");
        out.println("Sorry UserName  Password Error!");  
       
        }  
    } 
   catch(Exception e){System.out.println(e);}  
  
}  
}
```
```
import java.io.*;  
import javax.servlet.*;  
import javax.servlet.http.*;  
  
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.annotation.WebServlet; 
import javax.servlet.http.HttpServletResponse;
@WebServlet("/Wel")
public class Wel extends HttpServlet {  
  
    public void doGet(HttpServletRequest request, HttpServletResponse response)  
        throws ServletException, IOException {  
            try{
       response.setContentType("text/html");  
    PrintWriter out = response.getWriter();  
      HttpSession session=request.getSession(false);    
   String name=(String)session.getAttribute("hello"); 
    out.print("hello"+name);  
        } 
    catch(Exception e){System.out.println(e);}  
 }
  
}  

```
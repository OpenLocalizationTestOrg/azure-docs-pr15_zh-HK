<properties
    pageTitle="返回通过访问控制服务 (Java) 的 SAML 视图"
    description="了解如何查看 SAML 返回位于 Azure 的 Java 应用程序中访问控制服务。"
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-view-saml-returned-by-the-azure-access-control-service"></a>如何查看 SAML Azure 访问控制服务返回的

本指南将向您介绍如何查看基础安全声明标记语言 (SAML) 返回到您的应用程序通过 Azure 访问控制服务 (ACS)。 指南 》 生成[如何将 Web 用户身份验证使用 Azure 访问控制服务使用 Eclipse][]的主题，通过提供显示 SAML 信息的代码。 完整的应用程序将类似于以下。

![SAML 输出示例][saml_output]

在 ACS 上的详细信息，请参阅[后续步骤](#next_steps)部分。

> [AZURE.NOTE]
> Azure Access Services 控制筛选器是社区技术预览。 作为预发行版软件，它不正式支持微软。

## <a name="prerequisites"></a>系统必备组件

若要完成本指南中的任务，完成[方法验证与 Azure 访问控制服务使用 Eclipse 的 Web 用户][]的示例，本教程将其用作起始点。

## <a name="add-the-jspwriter-library-to-your-build-path-and-deployment-assembly"></a>JspWriter 将库添加到生成路径和部署程序集

添加包含**javax.servlet.jsp.JspWriter**类的生成路径和部署程序集的库。 如果您正在使用 Tomcat，库是**jsp api.jar**，它位于 Apache **lib**文件夹中。

1. 日蚀式的项目资源管理器，右键单击**MyACSHelloWorld****生成路径**单击**配置生成路径**，单击**库**标签中，和，然后单击**添加外部 JARs**。
2. 在**jar/文件夹选择**对话框中，导航到所需的 JAR，选择它，然后单击**打开**。
3. 与仍然打开**的 MyACSHelloWorld 属性**对话框中，单击**部署程序集**。
4. 在**Web 部署程序集**对话框中，单击**添加**。
5. 在**新程序集指令**对话框中，单击**Java 构建路径条目**，然后单击**下一步**。
6. 选择相应的库，然后单击**完成**。
7. 单击**确定**以关闭**MyACSHelloWorld 的属性**对话框。

## <a name="modify-the-jsp-file-to-display-saml"></a>修改要显示 SAML 的 JSP 文件

修改**index.jsp**使用下面的代码。

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {
        
            try
            {
                String nodeName;
                int nChild, i;
                
                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");
                   
                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }
                   
                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                   {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    
    
                                 // If it is a text node, just print the text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out the child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        
                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate the names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish the sentence.
                                            out.print(".");
                                        }
                                            
                                     }
                                     out.println("<br>");
                                     
                                     // Process the child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>
    
        <%
        try 
        {
            String data  = (String) request.getAttribute("ACSSAML");
            
            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();
            
            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA); 
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();
            
            // Iterate the child nodes of the doc.
            NodeList list = doc.getChildNodes();
    
            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e) 
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }
        
        %>
    </body>
    </html>

## <a name="run-the-application"></a>运行应用程序

1. 在计算机模拟器上运行应用程序或部署到 Azure，使用记录在[如何将 Web 用户身份验证使用 Azure 访问控制服务使用 Eclipse][]的步骤。
2. 启动浏览器并打开您的 web 应用程序。 登录到您的应用程序后，您将看到 SAML 信息，包括标识提供程序提供的安全断言。

## <a name="next-steps"></a>下一步行动

为进一步探讨 ACS 的功能和试验更复杂的方案，请参阅[访问控制服务 2.0][]。

[Prerequisites]: #pre
[Modify the JSP file to display SAML]: #modify_jsp
[Add the JspWriter library to your build path and deployment assembly]: #add_library
[Run the application]: #run_application
[Next steps]: #next_steps
[访问控制服务 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[如何通过使用 Eclipse 的 Azure 访问控制服务的 Web 用户进行身份验证]: ../active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
 
--- 
layout: post
title: Upload com Spring MVC
tags: 
- Java
- Spring MVC
- Upload
status: publish
type: post
published: true
meta: 
  _edit_last: "1"
  _syntaxhighlighter_encoded: "1"
  _wp_old_slug: ""
  dsq_thread_id: "175767059"
---
Esse post é um tutorial passo a passo para fazer upload com Spring MVC.

Os passos desdes posts foram feitos com o projeto template "Spring MVC Project" do [SpringSource Tool Suite](http://pablocantero.com/blog/2010/08/27/springsource-tools-suite-e-nao-e-que-e-bom/), a versão do Spring MVC utilizada foi a 3.0.1.RELEASE.

##Primeiro passo

Adicionar as dependências commons-fileupload e commons-io no pom.xml.

    <properties>
     <commons-fileupload.version>1.2.1</commons-fileupload.version>
     <commons-io.version>1.4</commons-io.version>
    </properties>
    <dependency>
     <groupId>commons-fileupload</groupId>
     <artifactId>commons-fileupload</artifactId>
     <version>${commons-fileupload.version}</version>
     </dependency>
     <dependency>
     <groupId>commons-io</groupId>
     <artifactId>commons-io</artifactId>
     <version>${commons-io.version}</version>
    </dependency>

##Segundo passo

Adicionar o CommonsMultipartResolver no mvc-config.xml. Graças a ele será possível fazer o cast (MultipartHttpServletRequest) request


    <bean id="multipartResolver"
     class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
     <!-- one of the properties available; the maximum file size in bytes -->
     <property name="maxUploadSize" value="1000000" />
    </bean>

##Terceiro passo

Criar o formulário multipart/form-data  para upload.

    <form action="/meu-projeto/upload" method="post" enctype="multipart/form-data">
     <p>
     <label for="file">Arquivo para fazer upload</label>
     <input
     type="file" name="file" />
     <input type="submit" name="submit"
     value="Upload" />
    </p>
    </form>

##Quarto é último passo

Criar a action de upload.

    @RequestMapping(value = "upload", method = RequestMethod.POST)
     public String upload(HttpServletRequest request) {
     MultipartHttpServletRequest multipartRequest = (MultipartHttpServletRequest) request;
     MultipartFile multipartFile = multipartRequest.getFile("file");
     return "redirect:upload-success";
     }


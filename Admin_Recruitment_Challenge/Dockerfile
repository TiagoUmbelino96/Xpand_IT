# Definição de CentOS 7 como base image
FROM centos:7

# Instalação de packages necessários jdk
RUN yum -y install java-1.8.0-openjdk-devel && \
    yum clean all

# Definição de variáveis
ENV CATALINA_HOME /opt/tomcat
ENV PATH $CATALINA_HOME/bin:$PATH

# Install Tomcat 8.5.99
RUN curl -O https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.99/bin/apache-tomcat-8.5.99.tar.gz && \
    mkdir -p /opt/tomcat && \
    tar -xzf apache-tomcat-8.5.99.tar.gz -C /opt/tomcat --strip-components=1 && \
    rm apache-tomcat-8.5.99.tar.gz

# Copiar o ficheiro sample.war para o diretorio do container
COPY sample.war /opt/tomcat/webapps/sample.war

# Copiar os certificados e configurações para diretorio do container. (estão ambos comentados, pq não consegui que o certificado fosse gerado, e crashava a aplicação)
#COPY ssl.crt $CATALINA_HOME/conf/
#COPY ssl-private.key $CATALINA_HOME/conf/
#COPY server.xml $CATALINA_HOME/conf/

# Atualizar o ficheiro server.xml para ativar SSL/TLS para o endpoint 4041. (estão ambos comentados, pq não consegui que o certificado fosse gerado, e crashava a aplicação)
#RUN sed -i 's/redirectPort="4041"/redirectPort="4041" scheme="https" secure="true" port="4041" keystoreFile="C:\Users\tiago\Desktop\Xpand_IT\Admin_Recruitment_Challenge\ssl.crt" keystorePass="" keyAlias="" keyPass=""/' $CATALINA_HOME/conf/server.xml


# Expor a porta 4041
EXPOSE 4041

# Iniciar o tomcat
CMD ["catalina.sh", "run"]

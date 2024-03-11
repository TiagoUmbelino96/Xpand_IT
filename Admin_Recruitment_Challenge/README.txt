PROCEDIMENTO PARA CORRER O CONTAINER

Assumindo que já tem o docker desktop instalado e a correr na máquina.
1.-> Fazer download do projeto em "" para o ambiente de trabalho

2.-> Entrar no diretorio onde está o Dockerfile e o ficheiro sample.war inputando o seguinte comando na powershell:
	> cd Desktop\Xpand_IT\Admin_Recruitment_Challenge

3.-> Build Docker image inputando o seguinte comando:
	> docker build -t sample-web .
Nota:(Este comando vai fazer o build da imagem do docker atribuido-lhe o nome "sample-web" com base no ficheiro Dockerfile nesse mesmo diretório).

4.-> Run Docker container inputando o seguinte comando:
docker run -d -p 4041:8080 --name sample-container sample-web

Nota:(Este comando vai correr o container gerado anteriormente em modo detached (-d), e expondo a porta 4041 do container para a porta 8080(http)* da máquina).

5.-> Por fim podemos aceder a nossa aplicação web a correr na versão Tomcat 8.5.99 em Dockercontainer:
	Abrir o browser e navegar até ao endereço: "http://localhost:4041/sample"

*Depois de muitas tentativas com alterações de código não consegui ativar o SSL/TLS para colocar a porta 4041 como endpoint. 
**Nem consegui, através do docker gerar o certificado digital que autentica o website e que nos permite uma conexão encriptada com o mesmo.
***Os certificados que estão no repositório foram gerados manualmente através do OpenSSL, para testes, mas sem sucesso.
    Está um ficheiro Generate_SSL_Certificate.txt que mostra como foram gerados.

=================================================================================================================================================================================

FICHEIRO DOCKERFILE

Neste dockerfile temos a configuração para correr um container com imagem centos7 através da versão 8.5.99 do Tomcat expondo a porta 4041.

Dockerfile:

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


=================================================================================================================================================================================

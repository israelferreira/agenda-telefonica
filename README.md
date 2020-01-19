# agenda-telefonica
## Esse é um projeto de uma Agenda Telefônica desenvolvido com a arquitetura REST. No front-end foi usado o Angular 8.2. No back-end: Java EE 8 com EJB3, RESTEasy (JAX-RS), Hibernate/JPA e o gerenciador de dependências Maven. O banco de dados é o MySQL 8.

## Screenshots:
**Adicionando um contato**
![Criar contato](https://user-images.githubusercontent.com/37079133/72674825-67bb0980-3a5a-11ea-90fd-03ef26a01750.PNG)
**Lista de contatos:**
![Lista de Contatos](https://user-images.githubusercontent.com/37079133/72674766-76ed8780-3a59-11ea-8b10-ef95202903e1.PNG)
**Detalhes do contato**
![Detalhes do contato](https://user-images.githubusercontent.com/37079133/72674765-76ed8780-3a59-11ea-95e2-5c45690cbfc5.PNG)

### Para o projeto funcionar:
O back-end foi desenvolvido para funcionar no servidor de aplicações WildFly 17, então algumas configurações extras são necessárias.
Na pasta do WildFly, ir até modules\layers\base\com e criar uma pasta chamada mysql, depois outra com o nome main.
O caminho completo fica assim: ...\wildfly-17.0.1.Final\modules\system\layers\base\com\mysql\main.
Baixar o arquivo jar do mysql-connector no site do Maven e colar na pasta main.
Ainda na pasta main, criar um arquivo chamado module.xml e colar o seguinte texto dentro:

```
<?xml version="1.0" encoding="UTF-8"?>
<module xmlns="urn:jboss:module:1.0" name="com.mysql">
	<resources>
		<resource-root path="mysql-connector-java-8.0.18.jar" />
	</resources>
	<dependencies>
		<module name="javax.api"/>
		<module name="javax.transaction.api"/>
	</dependencies>
</module>
```

É necessário adaptar a linha abaixo para a versão do MySQL que foi baixada
```
<resource-root path="mysql-connector-java-8.0.18.jar" />
```

Após isso, ir no diretório wildfly-17.0.1.Final\standalone\configuration e abrir o arquivo standalone.xml.
Dentro da tag datasources, adicione o seguinte texto:

```
<datasource jta="true" jndi-name="java:/jdbc/AgendaTelefonica" pool-name="AgendaTelefonica" enabled="true" use-java-context="true" use-ccm="true">
    <connection-url>jdbc:mysql://localhost:3306/agenda_telefonica?useSSL=false&amp;useTimezone=true&amp;serverTimezone=America/Sao_Paulo</connection-url>
    <driver>com.mysql</driver>
    <security>
        <user-name>root</user-name>
        <password>root</password>
    </security>
</datasource>
```
Substitua o nome de usuário e a senha para a versão utilizada por você.

Dentro da tag drivers, adicione o seguinte texto:
```
<driver name="com.mysql" module="com.mysql">
    <driver-class>com.mysql.cj.jdbc.Driver</driver-class>
    <xa-datasource-class>com.mysql.cj.jdbc.MysqlXADataSource</xa-datasource-class>
</driver>
```

Após esses passos, o projeto deverá funcionar normalmente.
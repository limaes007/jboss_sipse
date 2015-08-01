# jboss_sipse
Jboss 7.1.1 SIPSE SDA

La documentacion del servidor se puede consultar en https://docs.jboss.org/author/display/AS71/Getting+Started+Guide

1. Descargar el jboss 7.1

2. Start jboss con 

	To use the full profile with clustering capabilities, use the following syntax from $JBOSS_HOME/bin:

	./standalone.sh --server-config=standalone-ha.xml - linux
	./standalone.bat --server-config=standalone-ha.xml - windows

	Similarly to run OSGi only server in  domain mode:
	./domain.sh --domain-config=domain-osgi-only.xml - linux
	domain.bat --domain-config=domain-osgi-only.xml - windows

 	Alternatively, you can create your own selecting the additional subsystems you want to add, remove, or modify.

3. Si todo salio bien (casi nunca pasa) el sistema le presenta una pagina de bienvenida en http://127.0.0.1:8080/

4. Para crear a la consola de administracion de jboss se debe crear un usuario de administracion, usando el archivo add-user.bat (windows) o add-user.sh (linux)
el archivo se encuentra en la carpeta bin, y presentara una especie de wizard que le permitira crear el usuario

	What type of user do you wish to add?
	 a) Management User (mgmt-users.properties)
 	b) Application User (application-users.properties)
	(a): a

	Enter the details of the new user to add.
	Realm (ManagementRealm) : ManagementRealm
	Username : admin
	Password :
	Re-enter Password :
	The username 'admin' is easy to guess
	Are you sure you want to add user 'admin' yes/no? yes
	About to add user 'admin' for realm 'ManagementRealm'
	Is this correct yes/no? yes
	Added user 'admin' to file 'C:\Users\limaes\Documents\SIPSE\jboss\jboss-as-7.1.1
	.Final\standalone\configuration\mgmt-users.properties'
	Added user 'admin' to file 'C:\Users\limaes\Documents\SIPSE\jboss\jboss-as-7.1.1
	.Final\domain\configuration\mgmt-users.properties'
	Presione una tecla para continuar . . .

5. Ingrese a la consola de adminsitracion http://localhost:9990/console el puerto por defecto es el 9990
usar el usuario y contraseña definido anteriormente tener en cuenta que en el realm, osea la primera pregunta va ManagementRealm el usr fue admin psw jboss

CREACION DEL DATASOURCE EN EL EL JBOSS 7.1

1. Se debe descargar el ojdbc7.jar para el jdk 1.7, si la version de jdk cambia el ojdbc tambien (creo)

2. El driver se debe configurar en la carpeta modules, yo la configure en la carpeta /modules/com/oracle/ojdbc7/main/ y ahi se pone el jar (no olvidar la carpeta main).

3. Al lado del jar se crea un archivo que se debe llamar modules.xml que debe ser parecido al siguiente (cambiaria deacuerdo a la configuracion de cada cual)
	<?xml version="1.0" encoding="UTF-8"?>
	<!-- ~ JBoss, Home of Professional Open Source. ~ Copyright 2010, Red Hat, Inc., and individual contributors ~ as indicated by the @author tags. See the copyright.txt file in the ~ distribution for a full listing of individual contributors. ~ ~ This is free software; you can redistribute it and/or modify it ~ under the terms of the GNU Lesser General Public License as ~ published by the Free Software Foundation; either version 2.1 of ~ the License, or (at your option) any later version. ~ ~ This software is distributed in the hope that it will be useful, ~ but WITHOUT ANY WARRANTY; without even the implied warranty of ~ MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU ~ Lesser General Public License for more details. ~ ~ You should have received a copy of the GNU Lesser General Public ~ License along with this software; if not, write to the Free ~ Software Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA ~ 02110-1301 USA, or see the FSF site: http://www.fsf.org. -->
	<module name="com.oracle.ojdbc7" xmlns="urn:jboss:module:1.1">
	<resources>
	<resource-root path="ojdbc7.jar"/>
		<!-- Insert resources here -->
	</resources>
	<dependencies>
		<module name="javax.api"/>
		<module name="javax.transaction.api"/>
		<module name="javax.servlet.api" optional="true"/>
	</dependencies>
	</module>
4a. En el archivo standalone.xml qeu se encuentra en la carpeta standalone/configuration/ (esto si el servidor se esta ejecutando en standalone.bat O standalone.sh) este es el que yo utilizo 
lo mismo es para el archivo domain.xml qeu se encuentra en la carpeta domain/configuration/ (esto si el servidor se esta ejecutando con el domain.bat)

	<drivers>
	    <driver name="OracleJDBCDriver" module="oracle.jdbc" />
	    <driver name="h2" module="com.h2database.h2">
	        <xa-datasource-class>
	            org.h2.jdbcx.JdbcDataSource
	        </xa-datasource-class>
	    </driver>
	</drivers>
- See more at: http://middlewaremagic.com/jboss/?p=350#sthash.EBdRhpP1.dpuf


jdbc:oracle:thin:@//localhost:1521/XE










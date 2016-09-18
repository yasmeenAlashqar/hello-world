# Smart-EIS-Desktop
You can build an application like this in 5 min,  and implement a full information system in one single day. 
![Smart-EIS-Desktop](https://raw.githubusercontent.com/kiswanij/smart-desktop/master/doc/screenshots/4.PNG "Smart-EIS-Desktop")  

**Important:** JK-Smart-Desktop is free for Personal use only, for quotation , kindly contact me at Kiswanij@GMail.com 
  
## Prerequisites:
  1-Install MySql database (e.g. `root` as username , `123456` as password)  
  2-On mysql , create database name (e.g. `jk-smart-desktop-db`)  
  **Optional**: for Feedback and Support functionality `SMTP` mail server settings will be needed

## Usage:
1-	Create Maven project
2-	Add smart-desktop dependency to your pom.xml file:

	<dependencies>
		<dependency>
			<groupId>com.jalalkiswani</groupId>
			<artifactId>smart-eis-desktop</artifactId>
			<version>0.0.9-4</version>
		</dependency>
	</dependencies>

3- Be sure to set the minimum JDK level in your pom file to 1.7 and tell maven to ignore web.xml by adding the following sections to your pom file:

	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.3</version>
				<configuration>
					<!-- http://maven.apache.org/plugins/maven-compiler-plugin/ -->
					<source>1.7</source>
					<target>1.7</target>
				</configuration>
			</plugin>
		</plugins>
	</build>

*Important for eclipse users:*

After you add the above section for Java version , it is important it refresh maven projects by right click on the `project-->Maven-->Update Project`

4- Create config files named `system.config` in your project root folder:

	db-host=localhost
	db-user=root
	db-password=123456
	db-port=3306
	db-name=jk-smart-desktop-db
	
	encoded=false
	tablemeta.dynamic.generate=true
	
	jk-support-mail-from=XYZ@Domain.com
	jk-support-mail-to=XYZ2@Domain2.com
	
	jk-mail-user=Username
	jk-mail-password=Password
	
5- Create your application main class , as follows:

	package com.jalalkiswani.demo;
	
	import com.fs.commons.application.ApplicationManager;
	
	public class SmartDesktopDemo {
		public static void main(String[] args)  {
			ApplicationManager.getInstance().start();
		}
	}
	 
6- Now run your main class, to check your installation:  
![Application Console](https://raw.githubusercontent.com/kiswanij/smart-desktop/master/doc/screenshots/1.PNG "JK-Smart-Desktop Application console")

7- If this was your first installation, a confirmation dialog will apear to ask you installing the base script on the specified database in the configuration file:   
![Database script confirmation dialog](https://raw.githubusercontent.com/kiswanij/smart-desktop/master/doc/screenshots/2.PNG "JK-Smart-Desktop script confirmation")  
Click `Yes`  

7- The appliction login-dialog will appear , Enter `admin` as username , `123456` as password:  
![Application Login Dialog](https://raw.githubusercontent.com/kiswanij/smart-desktop/master/doc/screenshots/3.PNG "JK-Smart-Desktop login dialog")  

8- Now you have the application and the framework up and running :  
![Application home page](https://raw.githubusercontent.com/kiswanij/smart-desktop/master/doc/screenshots/4.PNG "JK-Smart-Desktop")  

## Features
There is a very long long of features which you can't expect, be patient until i have some time to summarize it. 
Coming soon ...

## Example 1 : Simple Add new CRUD page
1- In the database, create sample table , for example:

	CREATE TABLE `employees` (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(255) DEFAULT NULL,
	  `salary` double DEFAULT NULL,
	  PRIMARY KEY (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8;

2- Create file named name `menu.xml` in `src/main/resources` folder , and add the below to it: 

	<main-menu>
		<menu name="HR">
			<menu-item name="Employees Management">
				<properties>
					<property name="table-meta" value="employees" />
				</properties>
			</menu-item>
		</menu>
	</main-menu>
 
3- Run your application , login , on the top of the windows, click on the `Application` module , click on `Hr` menu , then `Employees` menu-item,
To enter new record , click on `Add`  on the top left on the windows , enter the information , then click add:
 ![Example CRUD Views](https://raw.githubusercontent.com/kiswanij/smart-desktop/master/doc/screenshots/5.PNG "JK-Smart-Desktop")

4- To edit any record , double record on the record in the Data table : 
 ![Edit mode](https://raw.githubusercontent.com/kiswanij/smart-desktop/master/doc/screenshots/6.PNG "JK-Smart-Desktop")
 

 
## Example 2 - Simple foreign key relation:     
1- Create table `departments` as follows:  

	CREATE TABLE `departments` (
	  `dep_id` int(11) NOT NULL AUTO_INCREMENT,
	  `dep_name` varchar(255) NOT NULL,
	  PRIMARY KEY (`dep_id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8  
	
2- Update `employees` table to include relation to the `departments` table  

	ALTER TABLE `employees`
	ADD COLUMN `dep_id`  int NULL;
	
	ALTER TABLE `employees` ADD FOREIGN KEY (`dep_id`) REFERENCES `departments` (`dep_id`);    

Now , the department id field is visible in the employees views automatically as follows:  
![Department field in employees management](https://raw.githubusercontent.com/kiswanij/smart-desktop/master/doc/screenshots/7.PNG)   
However since the table is empty,it will not be very usefull , so let is proceed with the next step:

3- Add management screen for `departments` table by modifying the `meta\menu.xml` as follows:  

	<main-menu>
		<menu name="HR">	
			<menu-item name="Departments">
				<properties>
					<property name="table-meta" value="departments" />
				</properties>
			</menu-item>	
			<menu-item name="Employees">
				<properties>
					<property name="table-meta" value="employees" />
				</properties>
			</menu-item>
		</menu>
	</main-menu>   

Now , you can edit the departments table as in the following screenshot:  
![Department management](https://raw.githubusercontent.com/kiswanij/smart-desktop/master/doc/screenshots/8.PNG).     
Now you if you get back to the employees,you can edit the employees records to show the orders table  

## Example 3 - Advanced CRUD with Master Details (One-To-One,One-Many,Many-To-Many): 
Now, let us add some detail information for the employees, 

	CREATE TABLE `employee_family` (
	  `member_id` int(11) NOT NULL AUTO_INCREMENT,
	  `member_name` varchar(255) NOT NULL,
	  `age` int(11) NOT NULL,
	  `emp_id` int(11) DEFAULT NULL,
	  PRIMARY KEY (`member_id`),
	  KEY `emp_id` (`emp_id`),
	  CONSTRAINT `employee_family_ibfk_1` FOREIGN KEY (`emp_id`) REFERENCES `employees` (`id`)
	) ENGINE=InnoDB DEFAULT CHARSET=utf8    

Modify `meta/menu.xml` as follows:

	<main-menu>
		<menu name="HR">	
			<menu-item name="Departments">
				<properties>
					<property name="table-meta" value="departments" />
				</properties>
			</menu-item>				
			<menu-item name="Employees">
				<properties>
					<property name="table-meta" value="employees" />
					<property name="detail-tables" value="employee_family" />
					<property name="detail-fields" value="emp_id" />
				</properties>
			</menu-item>
		</menu>
	</main-menu>
	
TBC....

##Customizing the Theme:  
Create file names `color.properties` in `src\main\resources` folder , and set the color for every key as the following example:  

	main_panel_bg=255,255,255
	
	module_panel_bg=#404040
	module_button_bg=22,125,219
	module_button_fc=255,255,255
	 
	 
	menu_panel_bg=#B71427
	menu_button_bg=191,215,255
	menu_button_fc=#FFFFFF
	
	mi_panel_bg=#f2efe8
	mi_button_bg=#D0DEF5
	mi_button_fc=#05193B
	mi-panel-title-bg=#B71427
	mi-panel-title-fg=#FFFFFF
	
	label_bg=191,215,255
	label_fg=30,30,100
	
	button_bg=240,240,255
	button_fg=0,0,0
	
	title_bg=#B71427
	title_fg=255,255,255
	
	title_bar_bg=#c2d4d8
	title_bar_fg=22,125,219
	
	title_border_bg=0,0,255
	
	table_even_row=238,246,255
	table_odd_row=255,255,255

The colors can be `R,G,B` or hex started with `#`.   
## Example 4 - Customize the metadata 
## Example 5 Database UI Components   
## Example 6 : 
Build Simple UI   
## Example 7 : 
Build Advanced UI   
## Example 8 : 
Reports    
And many others...

###Tested on 
-Microsoft Windows XP,7,8,8.1,10   
-Mac OS Captain  
Please let us know if you make it on any other O.S.   
##FRAMEWORK IMPLEMENTATION AND USAGE EXAMPLE
![Smart-Desktop dependencies](https://raw.githubusercontent.com/kiswanij/smart-desktop/master/design/smart-dsktop-the-full-picture.PNG)
<http://wwww.JalalKiswani.com>

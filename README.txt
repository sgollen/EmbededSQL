DESIGN DOCUMENT BY SAMAKSH GOLLEN                     

FOR COMPILATION AND RUNING TESTs
./compile : compiles both apps
./authorbib AuthorName  : runs the bibauthor on AuthorName
./mainbibliography < InputFile : runs the maint on input provided in the InputFile


mainbibliography.sqc design
To solve the problem of updating and inserting authors into different types of tables, my program is divided into different parts. In the main function, it reads line of input provided in stdin using getline and then tokenizes the input line using strtok, then it checks the type of the update/insert by looking at the first token in the tokenized line. Then depending on its type it calls the appropriate helper functions and passes it the reamining token line as an argument to solve the problem.
In helper functions first assigns various variabes required for the SQL quries such pubid, name, etc using the strtok for geting the token for appropriate variables depending on their positions/format of input. Additionally, strcpy is used to store the values into string variables and atoi is used to convert string into int if the required varible is of type int. 
Subsequently, in order to determine if the request is an update or an insertSQL query that uses Count(*) is used. The query updates the value of the varible occurs depending on the number of instances of the input in the table. IF the occurs is 0  then that means the request is to insert as the given input/request is not in the table. if occurs is NOT 0 then request is Update. 
Lastly, if the update request is for author, then we either update or insert the author name depending on if exits in the table(using method described earlier) and nothing is printed. If request is for authorurl, the url is only updated if the author exits and nothing is printd due to format request by the question. 
If the request is for a publication such as book, article etc, my program first check if the publication exists and updates and updates the table accordingly. In case of books and articles, if an update is requested then the author tuples
are first deleted according to the pubid given in the input and new authors are added as provided by input. The order of the authors is depend on their position  in given input 
(e.g first author = aorder 1 etc)

authorbib.sqc Design
Similar to the bibmainit, the problem of outputing different tables are divided into different parts. Firstly, my main fuction reads in the argument provided and store it into variable givenName usng strcpy the main function also uses queries to get the publications written by the given author and journals and
proceedings since they can not be written by an author. In my main function i try to create a temp table that has the pubid, Author name, year, document type for books, article, proceedings and jorunals.In the With query, bookTable tries to get the  pubid, Author name, year and creates a column with the type as'book'.
The next table created in the nested with query tries to get the pubid, Author name, year and sets the type as 'article'. The year column of the article is determined by year of the journal or proceeding it appearsin. 
The last table uses article to to get the  pubid, Author name, year and creates a column with the type 'proceedings' or journal depending on the the type which the article appears in. The combination of these three tables are then sorted by the latest year. 
Ultimately, the goal of the with statement is to have table with books, articles, same articles but different type(divided into proceedings or journal) and use one cursor to interate through each row of the table and put values into host variables such as pubid, name, year, docType using a loop and use these values into helper functions. 
The program determines the helper function to be used by the the docType. And use these helper functions to print the info required by the question. Since an article of a particluar name will have 2 tupes dedicated to it (One As Article, One as proceedings or journal), correct output isprinted.


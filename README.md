Download Link: https://assignmentchef.com/product/solved-cis407a-week-5-ilab-transaction-processing
<br>
<header class="entry-header"></header>

5/5 - (2 votes)

This week, we will use the .NET OleDbTransaction functions to either commit a set of changes to the database, if all of them were done correctly, or to roll back all of the changes if there was an error in any one of them. We will first modify the code we created last week so that it saves personnel data in the database in two steps; first by inserting a personnel record for a new employee, and then by updating that record to fill in the start and end dates. This two-step approach is not really needed in this simple case, but we will use it to simulate a more complex database transaction that would have to be done in multiple steps, such as one involving more than one table or even more than one database. We will then see what happens when there is an error in the second operation (the update), allowing a record to be created containing incomplete information–not a good result! We will fix the problem by wrapping both operations (the insert and the update) into a single transaction that will be committed (made permanent) only if both operations succeed or that will be rolled back (undone) if either operation fails. We will also add client-side validation using the ASP.Net validation controls, and we will allow the user an easy way to edit all employees.Instructions for Week 5 iLab: “Transaction Processing”

<strong>Deliverables</strong>All files are located in the subdirectory of the project. The project should function as specified: When you press the Submit button in frmPersonnel, a record should be saved in the tblPersonnel table containing the FirstName, LastName, PayRate, StartDate, and EndDate you entered. Test that the transaction will rollback by entering invalid information in one or more items, such as Hello for a StartDate. Check tht client-side validation works: The ability to edit employees in a grid is working. Once you have verified that it works, save your website, zip up all files, and submit them to the Dropbox.

<strong>iLAB STEPS</strong><strong>STEP 1:</strong> Modify the clsDataLayer to use a two-step process (10 points)1. Open Microsoft Visual Studio.NET 2008.2. Click the ASP.NET project called PayrollSystem to open it.3. Open the clsDataLayer class.4. Modify the SavePersonnel() function so that instead of just doing a single SQL INSERT operation with all the personnel data, it does an INSERT with only the FirstName and LastName, followed by an UPDATE to save the PayRate, StartDate, and EndDate into the new record. (This two-step approach is not really necessary here because we are dealing with only one table, tblPersonnel, but we are doing it to simulate a case with more complex processing requirements, where we would need to insert or update data in more than one table or maybe even more than one database.) Find the following existing code in the SavePersonnel() function:

6. Set frmMain as the startup form and run the PayrollSystem Web application to test the changes. When valid data values are entered for a new employee, things should work exactly as they did before. To test this, enter valid data for a new employee in frmPersonnel and click Submit. The frmPersonnelVerified form should be displayed with the entered data values and a message that the record was saved successfully. Click the View Personnel button and check that the new personnel record was indeed saved to the database and that all the entered data values, including the PayRate, StartDate, and EndDate, were stored correctly. Close the browser window.7. Now run the PayrollSystem Web application again, but this time enter some invalid data (a nonnumeric value) in the PayRate field to cause an error, like this:frmPersonnel With Bad Data8. Click here for text description of this image.9. Now when you click Submit, the frmPersonnelVerified form should display a message indicating the record was not saved:frmPersonnel Verified With Error Message10. Click here for text description of this image.11. However, when you click on the View Personnel button to display the personnel records, you should see that an incomplete personnel record was in fact created, with missing values for the PayRate, StartDate and EndDate fields:Incomplete Personnel Record12. Click here for text description of this image.13. This occurred because the Insert statement succeeded but the following Update statement did not. We do not want to allow this to happen because we end up with incomplete or incorrect data in the database. If the Update statement fails, we want the Insert statement to be rolled back, or undone, so that we end up with no record at all. We will fix this by adding transaction code in the next step.

<strong>STEP 2:</strong> Add transaction code (10 points)7. In the clsDataLayer.cls class file, add code to the SavePersonnel() function to create a transaction object. Begin the transaction, commit the transaction if all database operations are successful, and roll back the transaction if any database operation fails. The following listing shows the complete SavePersonnel() function; the lines you will need to add are marked with ** NEW ** in the preceding comment and are shown in bold.

8. Run your Web application. First, enter valid data in all the fields of frmPersonnel. When you press the Submit button in frmPersonnel, a record should be saved in the tblPersonnel table containing the FirstName, LastName, PayRate, StartDate, and EndDate. With valid data entered in all the items, the “successfully saved” message should appear indicating that the transaction was committed.frmPersonnelVerified After Commit9. Click here for text description of this image.10. Click the View Personnel button and verify that the new record was in fact added to the database table correctly.frmViewPersonnel After Commit11. Click here for text description of this image.12. Now close the browser, run the Web application again, and this time test that the transaction will roll back after entering incorrect information. On the frmPersonnel form, enter invalid data for PayRate and click Submit. The “not saved” message should appear, which indicates that the transaction was rolled back.frmPersonnel Verified After Rollback13. Click here for text description of this image.14. Click the View Personnel button and verify that this time, as desired, an incomplete record was not added to the database table.frmViewPersonnel After Rollback15. Click here for text description of this image.16. You have seen how we used the try/catch block to catch an unexpected error. You may have noticed that if you enter bad data for the dates, an exception is thrown. Go back to the validation code you added in the frmPersonnel code and add a try/catch with logic to prevent an invalid date from causing a server error.17. In the Week 3 and Week 5 labs, you learned how to validate code once the page was posted back to the server. There is some validation that must be done on the server because it requires server resources such as the database. Some validation can also be done on the client. If you can do validation on the client it saves a round trip to the server, which will improve performance. In this approach, we will check values before the page is submitted to the server for processing. Normally, there is a combination of server and client validation used in a web application. ASP.Net includes validation controls which will use JavaScript on the client to perform validation. You will find these controls in the Validation group in the toolbox.18. Add validation controls to the frmPersonnel form as follows: For the first and last name, make sure each field has data in it. Use the RequiredFieldValidator for this. Add the control to the right of the text box you are validating. The location of the validator control is where the error message (if there is one) will appear for the control you link the validator to. You will be adding one validator control for each text box you want to validate. Remember to set the ControlToValidate and ErrorMessage properties on the validator control. Making this change eliminates the need for the server-side check you were doing previously. Use a regular expression validator to check that the start and end date are in the correct format.frmPersonnel Validation Controls19. Click here for text description of this image.20. Remove the View Personnel and Cancel buttons from the frmPersonnel form as they will cause a Postback and invoke the client-side editing you just added. The user is able to get to the View Personnel from the main form and from the personnel verification screen, so there is no need for these buttons now.21. Because you have entered data in this lab that is invalid and those partial records are in the database, you will need to add the ability to remove or update data. Add a new main form option called Edit Employees. Add the link and image for this. This option will take the user to a new form called frmEditPersonnel.frmMain with links added22. Click here for text description of this image.23. Add the new form frmEditPersonnel. On frmEditPersonnel, add the CoolBiz log at the top of the form. Add a label that says “Edit Employees.” Add a GridView control with an ID of grdEditPersonnel.24. You will now add a SQLDataSource to the page. You will be using a databound grid for this form unlike the previous grids, in which you added as unbound (in the designer).25. Add a new SQLDataSource control to the frmEditPersonnel in the design view. This is not a visible control; that is, it will only appear in design view but the user will never see it. Note: If you change the folder name or location of your database, you will need to reconfigure the data source (right-click on the data source control and select the “Configure Data Source” option.26. There is a small &gt; indicator in the design view of the SQL Data Source control you added if the configuration menu is collapsed (press it to open the menu), or there is a &lt; with the menu displayed. From the data source menu, select “Configure Data Source.” 27. Press the New Connection button and select the database. 28. Press the Next button. 29. When asked if you want to save the connection in the application configuration file, check the Yes check box and press Next. 30. Select the tblPersonnel table. 31. Select all columns (you can use the * for this). 32. Press the Advanced button and check the Generate Insert, Update, and Delete option and press the OK button. 33. Press the Next button. 34. Press the Test Query button and make sure everything works as it is supposed to. If it does not repeat the above steps to make sure you did everything properly. Press the Finish button. 7. Click on the grid you added in the design view and expand the properties menu (the little &gt; in the upper right of the control). Choose the data source you just added. On the GridView tasks menu, select Edit columns. Add an Edit, Update, and Cancel Command field. Add a Delete Command field. Press OK. You can now test the grid, which is a fully functioning Update and Delete grid. Try it out!Edit Employees8. Click here for text description of this image.9. Hints: Make sure you re-establish your database connection if you copied the files from a previous lab.In order to keep the validation controls from causing wrapping, you may want to increase the Panel width.A regular expression for mm/dd/yyyy is this:^(0[1-9]|1[012])[- /.](0[1-9]|[12][0-9]|3[01])[- /.](19|20)dd$Experiment with the editable grid and command buttons for different display styles.

<strong>STEP 3</strong>: Test and submit (10 points)29. Once you have verified that everything works as it is supposed to, save your project, zip up all files, and submit in the Dropbox.

<strong>NOTE:</strong> Make sure you include comments in the code provided where specified (where the ” // Your comments here” is mentioned) and for any code you write, or else a five point deduction per item (form, class, function) will be made.

<table border="0" cellspacing="0" cellpadding="0">

 <tbody>

  <tr>

   <td class="code"></td>

  </tr>

 </tbody>

</table>

<code class="php comments">// Add your comments here</code>

<code class="php plain">strSQL = </code><code class="php string">"Insert into tblPersonnel "</code> <code class="php plain">+</code>

<code class="php string">"(FirstName, LastName, PayRate, StartDate, EndDate) values ('"</code> <code class="php plain">+</code>

<code class="php plain">FirstName + </code><code class="php string">"', '"</code> <code class="php plain">+ LastName + </code><code class="php string">"', "</code> <code class="php plain">+ PayRate + </code><code class="php string">", '"</code> <code class="php plain">+ StartDate +</code>

<code class="php string">"', '"</code> <code class="php plain">+ EndDate + </code><code class="php string">"')"</code><code class="php plain">;</code>

<code class="php comments">// Add your comments here</code>

<code class="php plain">command.CommandType = CommandType.Text;</code>

<code class="php plain">command.CommandText = strSQL;</code>

<code class="php comments">// Add your comments here</code>

<code class="php plain">command.ExecuteNonQuery();</code>

<code class="php plain">5. Modify it so that it reads </code><code class="php keyword">as</code> <code class="php plain">follows:</code>

<code class="php comments">// Add your comments here</code>

<code class="php plain">strSQL = </code><code class="php string">"Insert into tblPersonnel "</code> <code class="php plain">+</code>

<code class="php string">"(FirstName, LastName) values ('"</code> <code class="php plain">+</code>

<code class="php plain">FirstName + </code><code class="php string">"', '"</code> <code class="php plain">+ LastName + </code><code class="php string">"')"</code><code class="php plain">;</code>

<code class="php comments">// Add your comments here</code>

<code class="php plain">command.CommandType = CommandType.Text;</code>

<code class="php plain">command.CommandText = strSQL;</code>

<code class="php comments">// Add your comments here</code>

<code class="php plain">command.ExecuteNonQuery();</code>

<code class="php comments">// Add your comments here</code>

<code class="php plain">strSQL = </code><code class="php string">"Update tblPersonnel "</code> <code class="php plain">+</code>

<code class="php string">"Set PayRate="</code> <code class="php plain">+ PayRate + </code><code class="php string">", "</code> <code class="php plain">+</code>

<code class="php string">"StartDate='"</code> <code class="php plain">+ StartDate + </code><code class="php string">"', "</code> <code class="php plain">+</code>

<code class="php string">"EndDate='"</code> <code class="php plain">+ EndDate + </code><code class="php string">"' "</code> <code class="php plain">+</code>

<code class="php string">"Where ID=(Select Max(ID) From tblPersonnel)"</code><code class="php plain">;</code>

<code class="php comments">// Add your comments here</code>

<code class="php plain">command.CommandType = CommandType.Text;</code>

<code class="php plain">command.CommandText = strSQL;</code>

<code class="php comments">// Add your comments here</code>

<code class="php plain">command.ExecuteNonQuery();</code>

<table border="0" cellspacing="0" cellpadding="0">

 <tbody>

  <tr>

   <td class="code"></td>

  </tr>

 </tbody>

</table>

<code class="php comments">// This function saves the personnel data</code>

<code class="php keyword">public</code> <code class="php keyword">static</code> <code class="php plain">bool SavePersonnel(string Database, string FirstName, string LastName,</code>

<code class="php plain">string PayRate, string StartDate, string EndDate)</code>

<code class="php plain">{</code>

<code class="php plain">bool recordSaved;</code>

<code class="php comments">// ** NEW ** Add your comments here</code>

<code class="php plain">OleDbTransaction myTransaction = null;</code>

<code class="php keyword">try</code>

<code class="php plain">{</code>

<code class="php comments">// Add your comments here</code>

<code class="php plain">OleDbConnection conn = </code><code class="php keyword">new</code> <code class="php plain">OleDbConnection(</code><code class="php string">"PROVIDER=Microsoft.Jet.OLEDB.4.0;"</code> <code class="php plain">+</code>

<code class="php string">"Data Source="</code> <code class="php plain">+ Database);</code>

<code class="php plain">conn.Open();</code>

<code class="php plain">OleDbCommand command = conn.CreateCommand();</code>

<code class="php plain">string strSQL;</code>

<code class="php comments">// ** NEW ** Add your comments here</code>

<code class="php plain">myTransaction = conn.BeginTransaction();</code>

<code class="php plain">command.Transaction = myTransaction;</code>

<code class="php comments">// Add your comments here</code>

<code class="php plain">strSQL = </code><code class="php string">"Insert into tblPersonnel "</code> <code class="php plain">+</code>

<code class="php string">"(FirstName, LastName) values ('"</code> <code class="php plain">+</code>

<code class="php plain">FirstName + </code><code class="php string">"', '"</code> <code class="php plain">+ LastName + </code><code class="php string">"')"</code><code class="php plain">;</code>

<code class="php comments">// Add your comments here</code>

<code class="php plain">command.CommandType = CommandType.Text;</code>

<code class="php plain">command.CommandText = strSQL;</code>

<code class="php comments">// Add your comments here</code>

<code class="php plain">command.ExecuteNonQuery();</code>

<code class="php comments">// Add your comments here</code>

<code class="php plain">strSQL = </code><code class="php string">"Update tblPersonnel "</code> <code class="php plain">+</code>

<code class="php string">"Set PayRate="</code> <code class="php plain">+ PayRate + </code><code class="php string">", "</code> <code class="php plain">+</code>

<code class="php string">"StartDate='"</code> <code class="php plain">+ StartDate + </code><code class="php string">"', "</code> <code class="php plain">+</code>

<code class="php string">"EndDate='"</code> <code class="php plain">+ EndDate + </code><code class="php string">"' "</code> <code class="php plain">+</code>

<code class="php string">"Where ID=(Select Max(ID) From tblPersonnel)"</code><code class="php plain">;</code>

<code class="php comments">// Add your comments here</code>

<code class="php plain">command.CommandType = CommandType.Text;</code>

<code class="php plain">command.CommandText = strSQL;</code>

<code class="php comments">// Add your comments here</code>

<code class="php plain">command.ExecuteNonQuery();</code>

<code class="php comments">// ** NEW ** Add your comments here</code>

<code class="php plain">myTransaction.Commit();</code>

<code class="php comments">// Add your comments here</code>

<code class="php plain">conn.Close();</code>

<code class="php plain">recordSaved = true;</code>

<code class="php plain">}</code>

<code class="php keyword">catch</code> <code class="php plain">(Exception ex)</code>

<code class="php plain">{</code>

<code class="php comments">// ** NEW ** Add your comments here</code>

<code class="php plain">myTransaction.Rollback();</code>

<code class="php plain">recordSaved = false;</code>

<code class="php plain">}</code>

<code class="php keyword">return</code> <code class="php plain">recordSaved;</code>
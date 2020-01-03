---
type: rule
archivedreason: 
title: Schema - Do you use Natural or Surrogate primary keys?
guid: 3a507ef5-5b84-446a-aeca-8725d67a4037
uri: schema---do-you-use-natural-or-surrogate-primary-keys
created: 2019-11-05T23:14:05.0000000Z
authors:
- id: 1
  title: Adam Cogan
related: []

---


<p>Now, this is a controversial one. Which one do you use?<br></p><ol><li>A &quot;Natural&quot; (or &quot;Intelligent&quot;) key is actual data</li><ul><li>Surname, FirstName, DateOfBirth</li></ul><li>An &quot;Acquired Surrogate&quot; (or &quot;Artificial​&quot; or &quot;System Generated&quot;) key is NOT derived from data eg. Autonumber</li><ul><li>eg. ClientID 1234<br></li><li>eg. ClientID JSKDYF</li><li>eg. ReceiptID 1234</li></ul><li>A &quot;Derived Surrogate&quot; (or &quot;User Provided&quot;) key is indirectly derived from data eg. Autonumber</li><ul><li>eg. ClientID SSW (for SSW)</li><li>eg. EmpID AJC (for Adam Jon Cogan)</li><li>eg. ProdID CA&#160;(for Code Auditor)</li></ul><li>A &quot;GUID&quot; key automatically generated by SQL Server​<br></li></ol>
<br><excerpt class='endintro'></excerpt><br>
<p>
   <strong>The problems with Natural Keys&#58;</strong></p><ul><li>Because they have a business meaning, if that meaning changes (eg. they change their surname), then that value NEEDS to change. Changing a value with data is a little hard - but a lot easier with Cascade Update.</li><li>The main problem is that the key is large and combined and this needs to be used in all joins</li></ul><p>
   <strong>The Problem with Acquired Surrogate Keys&#58;</strong></p><ul><li>A surrogate key has no meaning to a user</li><li>It always requires a join when browsing a child table eg. The InvoiceDetail table</li></ul><p>
   <strong>The Problem with Derived Surrogate</strong></p><ul><li>The user needs to enter a unique value</li><li>Because they have a business meaning, if that meaning changes (eg. they change their company name), then that value MAY NEED to change. Changing a value with data is a little hard - but a lot easier with Cascade Update</li><li>More likely to have a problem with Merge Replication<br></li></ul><p>
   <strong>The Problem with GUID key</strong></p><p>We like GUID keys. However, GUID generation produces essentially random numbers which cannot realistically be used as primary keys since new rows are inserted into the table at random positions leading to extremely slow inserts as the table grows to a moderate size. Inserting into the middle of a table with a clustered index, rather than appending to the end can potentially cause the database to have to move large portions of the data to accommodate space for the insert. This can be very slow.<br></p><p>
   <strong>Recommendations</strong></p><ol><li>We do not&#160;use Natural keys ever</li><li>We use Acquired Surrogate for some tables</li><ul><li>eg. Invoice table</li><li>eg. Receipt table<br></li></ul><li>a combination of Acquired Surrogate and Derived Surrogate&#160;for other tables</li><ul><li>eg. Customer table</li><li>eg. Employee table</li><li>eg. Product table<br></li></ul></ol>When we say combination because if the user doesn't enter a value then we put a random value in (by a middle tier function, so it works with Access or SQL). eg. ClientID JSKDYF<p>The user can then change the value to anything else and we validate it is not used, and then perform a cascade update - or if it is more then 3 levels deep we execute a stored proc. Unfortunately, this is a complicated proc that cycles through all related tables and performs an UPDATE. Here is an&#160;<a href="https&#58;//www.ssw.com.au/ssw/KB/CodeBase/04SQLServer/A-RenamePrimaryKey-RD.txt">example</a>.<br></p><p>The Derived Surrogate has the benefit of being easy for people to remember and can be used in the interface or even the query string</p><p>Over the years experience has lead me to the opinion that the natural vs surrogate key argument comes down to a style issue. If a client or employer has a standard one way or another, fine use it. If not, us​e whichever you method you prefer, recognizing that there may be some annoyances you face down the road. But don't let somebody criticize you because your style doesn't fit his preconceived notions.​<br></p>


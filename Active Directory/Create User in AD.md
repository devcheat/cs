# Create User in AD

```cs
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.DirectoryServices;
using System.DirectoryServices.ActiveDirectory;
using System.DirectoryServices.AccountManagement;

namespace ActiveDirectory_User
 {
  class Program
  {
  // our User class
  public class User
  {
   // our user variables
   public string Firstname;
   public string Lastname;
   public string LoginId;
   public string Department;
   public string Office;
   public string EmailAddress;
   public bool IsDisabled;

   // Default constructor for our user object
   public User() { }

   // Custom constructor that takes a set of parameters and sets the respective variable - notice how I change
   // the variable from camelCase to PascalCase - this is how I program, you do not have to do this but do get in to a
   // habit of standardising your code variables
   public User(string firstName, string lastName, string loginId, string department, string office, string emailAddress)
   {
    Firstname = firstName;
    Lastname = lastName;
    LoginId = loginId;
    Department = department;
    Office = office;
    EmailAddress = emailAddress;
   }

   // user method one (CreateAccount)
   public bool CreateAccount()
   {
    // wrap our connection in a try catch block which will safeguard us against application crash if for example we can't connect to AD
    try
    {
     string FullPath = "LDAP://PLABDC01/ou=" + Department + ",dc=PRACTICELABS,dc=COM";

     // Active directory connection
     DirectoryEntry Directory = new DirectoryEntry(FullPath, "Administrator", "Passw0rd");

     // New user
     DirectoryEntry NewUser = Directory.Children.Add("CN=" + LoginId, "user");

     // givenName is first name
     NewUser.Properties["givenName"].Value = Firstname;

     // sn is last name
     NewUser.Properties["sn"].Value = Lastname;

     // login id
     NewUser.Properties["sAMAccountName"].Value = LoginId;

     // office
     NewUser.Properties["physicalDeliveryOfficeName"].Value = Office;

     // commit the new user
     NewUser.CommitChanges();

     // update the user to be enabled, here we CAST the value as an integer (i.e. we instruct the compiler that the return type value will be an int.
     // casting this will cause exceptions if the return type is not what you specify so beware!
     int val = (int)NewUser.Properties["userAccountControl"].Value;
     NewUser.Properties["userAccountControl"].Value = val & ~0x2;
     NewUser.CommitChanges();

     // everything worked ok, return a value of true
     return true;
    }
    catch (Exception error)
    {
     // an error occured, write the error message out and return a value of false
     Console.Write("An error occured while creating user:{0} {1}: \n{2}", Firstname, Lastname, error.Message);
     return false;
    }
   }

   // user method that searches active directory to find our account and then disable it
   public bool DisableAccount()
   {
    // wrap our method in a try catch block in case it fails when we connect to AD or something
    try
    {
     // attach to AD
     DirectoryEntry Directory = new DirectoryEntry("LDAP://dc=PRACTICELABS,dc=COM");

     // create an AD search object
     DirectorySearcher SearchAD = new DirectorySearcher(Directory);

     // add our filter which is our login id
     SearchAD.Filter = "(SAMAccountName=" + LoginId + ")";
     SearchAD.CacheResults = false;

     // get the result
     SearchResult Result = SearchAD.FindOne();
     Directory = Result.GetDirectoryEntry();

     // if we get the result, disable the account and commit the changes
     Directory.Properties["userAccountControl"].Value = 0x0002;
     Directory.CommitChanges();
     return true;
    }
    catch (Exception error)
    {
     // oh dear something went wrong, again write the error message out including the login id
     Console.WriteLine("An error occured when disabling this user:{0}\n{1}", LoginId, error.Message);
     return false;
    }
   }
  }

  static void Main(string[] args)
  {
   // create our OU's
   string[] Departments = { "Marketing", "Sales", "Human Resources" };
   foreach (string dept in Departments)
   {
    CreateOU(dept);
   }

   // generate our random users - this populates AD
   List MyRandomUsers = GenerateRandomUserAccounts();

   // write our user information to the console so we can see the users that were created
   foreach (User u in MyRandomUsers)
   {
    Console.WriteLine((u.Firstname + " " + u.Lastname).PadRight(18, ' ') + u.Department.PadRight(18, ' ') + u.Office);
   }

   // disable every other account
   int Count = 0;
   foreach (User u in MyRandomUsers)
   {
    if (Count % 2 == 0)
    {
     if (u.DisableAccount())
     {
      Console.WriteLine("Disabled: {0} {1} in department: {2}", u.Firstname, u.Lastname, u.Department);
     }
     else
     {
      Console.WriteLine("Failed to disable: {0} {1} in department: {2}", u.Firstname, u.Lastname, u.Department);
     }
    }
    Count++;
   }

   // this is an added function not explained in the tutorial, for more information see the explaination of the code beloew
   UpdateDisabled(MyRandomUsers);

   // write out the users (proves the update of the IsDisabled flag of the user object
   foreach (User u in MyRandomUsers)
   {
    if (u.IsDisabled)
    {
     Console.WriteLine("Account:{0} is disabled", u.LoginId);
    }
   }

   // wait for a keypress so everything doesnt disapear
   Console.ReadLine();
  }

  public static void UpdateDisabled(List users)
  {
   Console.WriteLine("Updating disabled accounts");
   foreach (User u in users)
   {
    try
    {
     DirectoryEntry Directory = new DirectoryEntry("LDAP://dc=PRACTICELABS,dc=COM");

     DirectorySearcher Search = new DirectorySearcher(Directory);
     Search.Filter = "(SAMAccountName=" + u.LoginId + ")";
     Search.CacheResults = false;
     SearchResult Result = Search.FindOne();

     Directory = Result.GetDirectoryEntry();
     object o = Directory.Properties["userAccountControl"].Value;

     // (514 = disabled)
     if ((int)Directory.Properties["userAccountControl"].Value == 514)
     {
      // this account is disabled
      u.IsDisabled = true;
     }
     else
     {
      // this account is not disabled
      u.IsDisabled = false;
     }
    }
    catch (Exception error)
    {
     Console.WriteLine("An error occured when disabling this user:{0}\n{1}", u.LoginId, error.Message);
    }
   }
  }

  public static void CreateOU(string ou)
  {
  try
  {
   if (!DirectoryEntry.Exists("LDAP://PLABDC01/ou=" + ou + ",dc=PRACTICELABS,dc=COM"))
   {
    try
    {
     DirectoryEntry ActiveDirectory = new DirectoryEntry("LDAP://PLABDC01/dc=PRACTICELABS,dc=COM", "Administrator", "Passw0rd");
     DirectoryEntry NewOU = ActiveDirectory.Children.Add("OU=" + ou, "OrganizationalUnit");
     NewOU.CommitChanges();
     ActiveDirectory.CommitChanges();
     Console.WriteLine("Created OU:{0}", ou);
     }
    catch (Exception error)
    {
     Console.WriteLine("An error occured while creating group:{0} :\n{1}", ou, error.Message);
    }
   }
   else
   {
    Console.WriteLine("OU already exists");
   }
  }
  catch (Exception error)
  {
   Console.WriteLine("We couldnt connect to AD! Is the server powered on?. Exception generated was\n{0}", error.Message);
  }
 }

 public static List GenerateRandomUserAccounts()
 {
  List RandomUsers = new List();
  string[] Firstnames = { "Bart", "Homer", "Maggie", "Marge", "Lisa", "Ned" }; // our firstname
  string[] Lastnames = { "Simpson", "Flanders", "Smith" }; // our lastname
  string[] Departments = { "Marketing", "Sales", "Human Resources" }; // our OU
  string[] Offices = { "London", "New York", "Hong Kong", "Japan" }; // our office

  Random RandomNumber = new Random();

  foreach (string l in Lastnames)
  {
   foreach (string f in Firstnames)
   {
    int DepartmentId = RandomNumber.Next(0, Departments.Length);
    int OfficeId = RandomNumber.Next(0, Offices.Length);
    User NewUser = new User(f, l, f + "." + l, Departments[DepartmentId], Offices[OfficeId], f + "." + l + "@practicelabs.com");
    RandomUsers.Add(NewUser);
    if (NewUser.CreateAccount())
    {
     Console.WriteLine("Created user OK:" + f + " " + l);
    }
   }
  }
  return RandomUsers;
 }
 }
}
```

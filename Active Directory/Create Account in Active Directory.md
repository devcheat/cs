# Create Account in Active Directory

```cs
using System.DirectoryServices;  
using System.DirectoryServices.ActiveDirectory;  
using System.DirectoryServices.AccountManagement;

namespace ActiveDirectory_User  
{    
 class Program    
 {    
  public class User     
  { 
    public string Firstname;    
    public string Lastname;    
    public string LoginId;    
    public string Office;    
    public string EmailAddress;    
    public bool IsDisabled; 
    
    public User()
    {}
    
    public User(string firstName, string lastName)    
    {     
     Firstname = firstName;     
     Lastname = lastName;    
    } 
    
    public User(string firstName, string lastName, string office = "London",)  
    {   
     Firstname = firstName;   
     Lastname = lastName;   
     Office = office;  
    }
  }     
  static void Main(string[] args)     
  {
    
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
  
  public bool DisableAccount()  
  {   
   try   
   {    
    DirectoryEntry Directory = new DirectoryEntry("LDAP://dc=PRACTICELABS,dc=COM");    
    DirectorySearcher SearchAD = new DirectorySearcher(Directory);    
    SearchAD.Filter = "(SAMAccountName=" + LoginId + ")";    
    SearchAD.CacheResults = false;    
    SearchResult result = SearchAD.FindOne();    
    Directory = result.GetDirectoryEntry();    
    Directory.Properties["userAccountControl"].Value = 0x0002;    
    Directory.CommitChanges();    
    return true;   
   }   
   catch (Exception error)   
   {    
    Console.WriteLine("An error occured when disabling this user:{0}\n{1}", LoginId, error.Message);    
    return false;   
   }  
  }//
     
  } 
 }
```

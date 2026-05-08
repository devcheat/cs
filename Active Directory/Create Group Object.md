# Create Group Object.

This C# code snippet creates a Group Object in Active Directory.

```cs
using System.DirectoryServices;
using System.Reflection;
 
public class ADGroup
{
 
   private String ADGRPOUP = "group";
 
   // Grouptype-Definition 
   enum GrpType : uint
   {
      UnivGrp = 0x08,
      DomLocalGrp = 0x04,
      GlobalGrp = 0x02,
      SecurityGrp = 0x80000000
   }
 
   DirectoryEntry ent = new DirectoryEntry("LDAP://RootDSE");
   String str = (String)ent.Properties["defaultNamingContext"][0];
   deAD = new DirectoryEntry("LDAP://" + str);
   GrpType gt = GrpType.GlobalGrp | GrpType.SecurityGrp;
   int typeNum = (int)gt; 
   DirectoryEntry ou = deAD.Children.Find("OU=Users");
   DirectoryEntry group = ou.Children.Add("cn=myGroupName", ADGRPOUP );
   group.Properties["sAMAccountName"].Add("myGroupName");
   group.Properties["description"].Add(" description myGroupName");
   group.Properties["groupType"].Add(typeNum);
   group.CommitChanges();
}
```

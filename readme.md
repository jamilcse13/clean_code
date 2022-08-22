# Clean Code

## Why Writing Clean Code Matters
### Reasons of Writing Clean Code:
* Reading is harder than writing
* Technical debt is depressing
* You're lazy
* Sloppy = Slow
* Don't be a verb

## Clean Code Principles
1. Right tool for the job
   - Watch for boundaries
   - Stay native over framework or tools
   - One language per file
2. High signal-to-noise ratio
   - TED: Terse, Expressive, Does one thing
   - DRY
   - Watch for repeated patterns
3. Self-documenting
   - Clear intent
   - Layers of abstractions
   - Format for readability
   - Favor code over comments

* The **_Rule_** of 7 effects
  - Number of parameters per method
  - Number of methods per class
  - Number of variables currently in scope


* **_DRY_** Principle: Don’t Repeat Yourself
* **_YAGNI_** Principle: You ain't gonna need it (add complexity when needed)


## Naming
### Why Naming Matters:
* To maintain code readability
* To avoid variable confliction
* To understand the intent of the variable
```angular2html
// bad practice
List<decimal> p = new List<decimal>() {5.50, 1.48};
decimal t = 0;
foreach(var i in p)
{
    t += i;
}
return t;
```
```angular2html
// good practice
List<decimal> prices = new List<decimal>() {5.50, 1.48};
decimal total = 0;
foreach(var i in prices)
{
    total += i;
}
return total;
```

### Naming Classes and Methods:
- **_Class_**
  * Bad naming:
      * WebsiteBO
      * Utility
      * Common
      * MyFunctions
      * JimmysUtils
      * Manager/Processor/Info
  * Good naming:
      * User
      * Account
      * QueryBuilder
      * productRepository
  * Naming Guidelines:
    - Noun: Good class names are noun, not verbs
    - Be Specific
    - Single Responsibility
    - Avoid generic suffixes: A class that contains products business logic should be named *Product* not *ProductManager* or *ProductInfo*


- **_Method_**:
    * Bad naming: when a user read the method name, he won't understand what's its responsibility
        - Go
        - Complete
        - Get
        - Process
        - Dolt
        - Start
        - Page_Load
    * Good naming: With good method name, the reader doesn't need to read the method to know what it does. These are self-explanatory names.
      - GetRegisteredUsers
      - IsValidSubmissions
      - ImportDocument
      - SendMail
      - LoadPage


- **_Avoid_**:
    - Side Effects:
      - mismatch method name and work
      - Exm: UserInfo method sends email
    - Warning Signs:
        - and, or, if
        - Don't user method name like
          - CalculateAndDisplay
          - ProcessOrDeny
    - Abbreviation:
      - RegUsr
      - RegisUser


- **_Naming Variables: Boolean_**:
  * Bad naming:
    - open
    - start
    - status
    - login
    ```angular2html
    if (login) {
      ...  
    }
    ```
  * Good naming:
    - isOpen
    - done
    - isActive
    - LoggedIn
    ```angular2html
    if (loggedIn) {
    ...  
    }
    ```
    
## Writing Conditionals That Convey Intent
### Boolean Assignments:
* Bad way:
    ```angular2html
    bool goingToChipotleForLunch;
    if (cashInWallet > 6.00)
    {
        goingToChipotleForLunch = true;
    }
    else
    {
        goingToChipotleForLunch = false;
    }
    ```
* Good way:
    ```angular2html
    bool goingToChipotleForLunch = cashInWallet > 6.00;
    ```

### Ternary is Beautiful:
* Bad way:
    ```angular2html
    int registrationFee;
    if (isSpeaker)
    {
        registrationFee = 0;
    }
    else
    {
        registrationFee = 50;
    }
    ```
* Good way:
    ```angular2html
    int registrationFee = isSpeaker ? 0 : 50;
    ```

### Avoid Magic Numbers:
* Bad code:
    ```angular2html
    if (age > 21)
    {
        // body here
    }
    
    if (status == 2)
    {
        // body here
    }
    ```
* Good code:
    ```angular2html
    const int legalVoterAge = 18;
    if (age > legalVoterAge)
    {
        // body here
    }
    
    if (status == Status.active)
    {
        // body here
    }
    ```


### Handling Complex Conditions:
- **_Intermediate Variables_**:
* Bad approach:
    ```angular2html
    if (employee.Age >55
    && employee.YearsEmployed > 10
    && employee.IsRetired == true)
    {
        // body here
    }
    ```
* Good approach:
    ```angular2html
    bool eligibleFOrPension = employee.Age >55
    && employee.YearsEmployed > 10
    && employee.IsRetired == true;
    
    if (eligibleFOrPension)
    {
        // body here
    }
    ```

- **_Encapsulate Complex Conditionals_**:
  * Principle: Favor expressive code over comments
  * Bad approach:
    ```angular2html
    // check for valid file extensions, confirm is admin or active
    if ( (fileExt == ".mp4"
        || fileExt == ".mpg"
        || fileExt == ".avi")
        && (isAdmin == 1 || isActiveFile)
    )
    ```
  * Good approach:
    ```angular2html
    private bool ValidFileRequest(string fileExtension, bool isActiveFile, bool isAdmin)
    {
        return (fileExt == ".mp4"
            || fileExt == ".mpg"
            || fileExt == ".avi")
            && (isAdmin == 1 || isActiveFile)
    }
    ```
  * Better approach:
      ```angular2html
      private bool ValidFileRequest(string fileExtension, bool isActiveFile, bool isAdmin)
      {
          var validFileExtensions = new List<string>() {"mp4", "mpg", "avi"};
        
          bool validFileType = validFileExtensions.Contains(fileExtension);
          bool userIsAllowedToViewFile = isAdmin && isActiveFile;
    
          return validFileType && userIsAllowedToViewFile;
      }
      ```

- **_Favor Polymorphism Over Switch and Enums_**:
* Bad approach:
```angular2html
public void LoginUser(User user)
{
    switch (user.Status)
    {
        case Status.Active:
            // active user logic
            break
        case Status.Inactive:
            // inactive user logic
            break
        case Status.Locked:
            // locked user logic
            break
    }
}
```
* Good approach:
```angular2html
public void LoginUser(User user)
{
    user.Login();
}

public abstract class User
{
    public string FirstName;
    public string LastName;
    public int status;
    public int AccountBalance;
    
    public abstract void Login();
}

public class ActiveUser: User
{
    public override void Login()
    {
        // Active user logic here
    }
}

public class InactiveUser: User
{
    public override void Login()
    {
        // Inactive user logic here
    }
}

public class LockedUser: User
{
    public override void Login()
    {
        // Locked user logic here
    }
}
```
- Each class knows hor to handle its unique behaviours. So no redundant switches are required.
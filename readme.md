# Clean Code
Code is clean if it can be understood easily – by everyone on the team. Clean code can be read and enhanced by a developer other than its original author. With understandability comes readability, changeability, extensibility and maintainability.<br/>
We have tried to discuss on **Clean Code** below. **C#** will be using as coding example.

## Why Writing Clean Code Matters
****
### Reasons of Writing Clean Code:
* 5 reasons to care
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
****
### Why Naming Matters:
* To maintain code readability
* To avoid variable confliction
* To understand the intent of the variable
```javascript
// bad practice
List<decimal> p = new List<decimal>() {5.50, 1.48};
decimal t = 0;
foreach(var i in p)
{
    t += i;
}
return t;
```
```javascript
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
    ```javascript
    if (login) {
      ...  
    }
    ```
  * Good naming:
    - isOpen
    - done
    - isActive
    - LoggedIn
    ```javascript
    if (loggedIn) {
    ...  
    }
    ```
    
## Writing Conditionals That Convey Intent
****
### Boolean Assignments:
* Bad way:
    ```javascript
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
    ```javascript
    bool goingToChipotleForLunch = cashInWallet > 6.00;
    ```

### Ternary is Beautiful:
* Bad way:
    ```javascript
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
    ```javascript
    int registrationFee = isSpeaker ? 0 : 50;
    ```

### Avoid Magic Numbers:
* Bad code:
    ```javascript
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
    ```javascript
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
      ```javascript
      if (employee.Age > 55
      && employee.YearsEmployed > 10
      && employee.IsRetired == true)
      {
          // body here
      }
      ```
  * Good approach:
      ```javascript
      bool eligibleForPension = employee.Age > 55
      && employee.YearsEmployed > 10
      && employee.IsRetired == true;
    
      if (eligibleForPension)
      {
          // body here
      }
      ```

- **_Encapsulate Complex Conditionals_**:
  * Principle: Favor expressive code over comments
  * Bad approach:
    ```javascript
    // check for valid file extensions, confirm is admin or active
    if ( (fileExt == ".mp4"
        || fileExt == ".mpg"
        || fileExt == ".avi")
        && (isAdmin == 1 || isActiveFile)
    )
    ```
  * Good approach:
    ```javascript
    private bool ValidFileRequest(string fileExtension, bool isActiveFile, bool isAdmin)
    {
        return (fileExt == ".mp4"
            || fileExt == ".mpg"
            || fileExt == ".avi")
            && (isAdmin == 1 || isActiveFile)
    }
    ```
  * Better approach:
      ```javascript
      private bool ValidFileRequest(string fileExtension, bool isActiveFile, bool isAdmin)
      {
          var validFileExtensions = new List<string>() {"mp4", "mpg", "avi"};
        
          bool validFileType = validFileExtensions.Contains(fileExtension);
          bool userIsAllowedToViewFile = isAdmin && isActiveFile;
    
          return validFileType && userIsAllowedToViewFile;
      }
      ```

### Favor Polymorphism Over Switch and Enums:
  * Bad approach:
      ```javascript
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
      ```javascript
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


### Be declarative:
* Bad approach:
    ```javascript
    List<User> matchingUsers = new List<User>();
    
    foreach (var user in users)
    {
        if (user.AccountBalance < minAccountBalance
            && user.Status == status.Active)
        {
            matchingUsers.Add(user);
        }
    }
    return matchingUsers;
    ```
* Good approach:
    ```javascript
    return users
        .where(u => u.AccountBalance < minAccountBalance)
        .where(u => u.Status < status.Active);
    ```

### Table Driven Method:
* Great for dynamic logic
* Avoids hard coding
* Write less code
* Avoids complex data structure
* Make changes without a code deployment
* Example -
* Bad approach:
    ```javascript
    if (age < 20)
    {
        return 345.60;
    }
    else if(age < 30)
    {
        return 319.50;
    }
    else if (age < 40)
    {
        return 376.38;
    }
    else if (age < 50)
    {
        return 516.25;
    }
    ```
* Good approach
  - Consider using the DB
  - store the data to a data table. Let table name: InsuranceRate
  - It has InsuranceRateId, MaximumAge, Rate
  ```javascript
  return Repository.GetInsuranceRate(age);
  ```
  
## Writing Clean Methods
****
### When To Create a Function
- **_Function vs Method:_**
    - Both functions and methods are pieces of code, called by name
    - Core difference: **Methods** are associated with an object


- **_Reasons of writing function:_**
    - To Avoid
      - Code Duplication
      - Excessive Indentation
      - Unclear intent 
      - More than 1 task/ multiple tasks


- **_Solution of Excessive Indentation:_**
    - _Extract Method_:
      - Before:
          ```javascript
          if
            if
              while
                do
                some
                complicated
                thing
              end while
            end if
          end if
          ```
      
      - After:
          ```javascript
          if
            if
              doComplicatedThing()
            end if
          end if

          doComplicatedThing()
          {
            while
              do some complicated thing
            end while
          }
          ```
    - _Fail Fast_:
        - throw exception as early as possible
        - Bad way:
            ```javascript
            public void RegisterUser(string username, string password)
            {
                if (!string.IsNullOrWhitespace(username)) {
                    if (!string.IsNullOrWhitespace(password)) {
                        // register user
                    } else {
                        throw new ArgumentException("Password is Required.");
                    }
                } else {
                    throw new ArgumentException("Username is Required.");
                }
            }
            ```
        
        - Good way:
            ```javascript
             public void RegisterUser(string username, string password)
             {
               if (!string.IsNullOrWhitespace(username)) {
                   throw new ArgumentException("Username is Required.");
               }
      
               if (!string.IsNullOrWhitespace(password)) {
                    throw new ArgumentException("Password is Required.");
               }
             }
             ```
      - Every switch should have a default statement. If the cases in switch fails, then the default statement will execute.
        ```javascript
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
               default:
                  throw new ApplicationException("Unknown status: " + user.Status)
            }
        }
        ```
  - _Return Early_:
    - `Use a return when it enhances readability... In certain routines, once you know the answer...not returning  immediately means  that you have to write more code, - Steve McConnell, "Code Complete"`
    
    - Old Code:
        ```javascript
        private bool ValidUsername(string username)
        {
            bool isValid = false;
            const int MinUsernameLength = 6;
            if (username.Length >= MinUsernameLength)
            {
                const int MaxUsernameLength = 25;
                if (username.Length <= MaxUsernameLength)
                {
                   bool isAlphaNumeric = username.All(char.IsLetterOrDigit);
                   if (isAlphaNumeric)
                   {
                       if (!ContainCurseWords(username))
                       {
                           isValid = IsUniqueUsername(username);
                       }
                   }
                }
            }
            return isValid;
        }
        ```
    - After Refactor:
      ```javascript
      const int MinUsernameLength = 6;
      if (username.Length < MinUsernameLength) return false;

      const int MaxUsernameLength = 25;
      if (username.Length > MaxUsernameLength) return false;

      bool isAlphaNumeric = username.All(char.IsLetterOrDigit);
      if (!isAlphaNumeric) return false;

      if (ContainCurseWords(username)) return false;

      return IsUniqueUsername(username);
      ```

- **_Solution of Excessive Indentation:_**
    - Convey Intent
    - Old Code:
        ```javascript
        private bool ValidFileRequest(string fileExtension, bool isActiveFile, bool isAdmin)
        {
            if ((fileExt == ".mp4" || fileExt == ".mpg" || fileExt == ".avi")
               && (isAdmin == 1 || isActiveFile))
        }
        ```
    - After refactor:
        ```javascript
        private bool ValidFileRequest(string fileExtension, bool isActiveFile, bool isAdmin)
        {
            var validFileExtensions = new List<string>() {"mp4", "mpg", "avi"};
        
            bool validFileType = validFileExtensions.Contains(fileExtension);
            bool userIsAllowedToViewFile = isAdmin && isActiveFile;
    
            return validFileType && userIsAllowedToViewFile;
        }
        ```

- **_Do One Thing:_**
    - A function have just single responsibility
    - Advantage:
        - Aids the reader
        - Promotes reuse
        - Eases naming and testing
        - Avoids side-effects

### How Many Parameters:
* Follow 7 Rules
* But we should strive for 0-2 parameters
* Focus on writing small focused functions
* Bad Way:
    ```javascript
    public void SaveUser(User user, bool sendEmail, int emailFormat,
        bool printReport, bool sendBill)
    ```
* Good Way:
  * Emails, printing, and billing are separate concerns from saving a user
  * They should be handled, therefore, in separate functions
* Avoid Flag Arguments (bool parameters)
  * Bad Approach:
    ```javascript
    private void SaveUser(User user, bool emailUser)
    {
       // save user here, then
       if (emailUser)
       {
          // email user
       }
    }
    ```
  * Good Approach:
    ```javascript
    private void SaveUser(User user)
    {
        // save user
    }

    private void EmailUser(User user)
    {
        // email user
    }
    ```

### Signs a Method is Too Long:
* Too Long Whitespace and Comments
* Scrolling Required
* Naming Issues
  * A function  should be easy to name if it has a single, clear, and defined task
  * If it is tough to name a function by its task, then refactor it into separate functions
* Multiple Conditionals
  * When multiple conditionals exist in a function, consider separate function to extract out the conditional
* Hard to Digest
  * Consider the rule of 7
  * if a function has more than one layer of abstraction at a time, then it should refactor. Each function should work with a single layer of abstraction.


* **Bob Martin** says in his book _Clean Code_: 
  * A function should rarely be over 20 lines and hardly ever over 100 lines.
  * A function shouldn't have more than 3 parameters.

### Handling Exceptions:
**_Exception Types:_**
* Unrecoverable
  * Null Reference
  * File not found
  * Access denied
* Recoverable:
  * Retry connection
  * Try different file
  * Wait and try again
* Ignorable
  * Logging click
  
**Only catch exceptions, you can handle. Can't handle the exception? - Fail fast, Fail loud**

**_handle Try Catch Block:_**
* Bad Approach:
  * if the RegisterSpeaker() method fails, then it should move on.  
  ```javascript
  try
  {
      RegisterSpeaker();
  }
  catch(Exception e)
  {
      LogError(e)
  }
  EmailSpeaker();
  ```
* Good Approach:
  * if the RegisterSpeaker() method fails, then it should handle and stop the process.
  ```javascript
  RegisterSpeaker()
  EmailSpeaker()
  ```

* Keep try catch block easily readable
* Consider moving the body of the try into a well-named function
* Bad Way:
    ```javascript
    try
    {
        // many lines
        // of complicated
        // logic here
    }
    catch (Exception e)
    {
        // do something
    }
    
    ```
* Good Way:
    ```javascript
    try
    {
        saveThePlanet();
    }
    catch (Exception e)
    {
        // do something
    }
    
    private void saveThePlanet()
    {
        // many lines
        // of complicated
        // logic here
    }
    ```

## Writing Clean Classes
****
### When To Create a Class:
* New Concept
  * Abstract or real-world
* Low Cohesion
  * Methods should relate
* Promote Reuse
  * Small, targeted => reuse
* Reduce Complexity
  * Solve once, hide away
* Clarify Parameters
  * Identify a group of data
  

### Class Cohesion:
* **Cohesion:** How strongly the responsibilities of a class are related.
* Cohesive Classes:
  * Enhances responsibility
  * Increases likelihood of reuse
    * Highly cohesive classes are more like to be reused
  * Avoids attracting the lazy
    * Classes are often given overly generic names that attract lazy developers who don't want to think about where the logic truly belongs
    * To avoid lazy developers: keep class names specific and descriptive to keep cohesion high.


* To avoid creating low cohesive classes, focus on:
  * Standalone methods
  * Fields used by only one method
  * Classes that change often

### Low vs High Cohesion:
**_Low Cohesion:_**
- In a **Vehicle** class, these methods are exist.
    - Edit vehicle options
    - Update pricing
    - Schedule maintain
    - Send maintenance reminder
    - Select financing
    - Calculate monthly payment

**_High Cohesion:_**
- We have maintained separate classes for separate methods based on responsibility.
- We create 3 classes for maintaining these methods
  - **Vehicle:**
    - Edit vehicle options
    - Update pricing
  - **VehicleMaintenance:**
    - Schedule maintain
    - Send maintenance reminder
  - **VehicleFinance:**
    - Select financing
    - Calculate monthly payment


### Primitive Obsession:
* Bad Approach:
    - It breaks the Rule of 7
    ```javascript
    private void SaveUser(string firstname, string lastname, string state
    string zip, string eyeColor, string phone, string fax, string maidenName)
    ```

* Good Approach:
    - The parameters are the clearly properties of a user
    - So the best way to resolve the approach is: pass the user object
    - Benefits:
      - Helps reader conceptualize
      - implicit -> explicit
      - Encapsulation: Adding another property doesn't impact the method signature
      - Easily find all references to the user object in code
    ```javascript
    private void SaveUser(User user)
    ```

### Proximity Principle:
* Try to maintain top down method for the methods which are called from another method
* Make code read top to bottom when possible. keep related actions together.
    ```javascript
    public void validateRegistration()
    {
        validateData();
        if (!SpeakerMeetOurRequirements())
        {
            // code here
        }
    }
    
    private void validateData()
    {
        // code here
    }
    
    private void SpeakerMeetOurRequirements()
    {
        // code here
    }
    ```

### Outline Rule:
* Multiple layers of abstraction
* Should read like a high-level outline
* **Not Do:**
  * Method 1
    * method 1a
      * method 1ai
      * method 1aii
      * method 1aiii
    * method 1b
    * method 1c


* **Do:**
    * Method 1
        * Method 1a
            * Method 1ai
            * Method 1aii
        * Method 1b
          * Method1bi
          * Method1bii
        * Method 1c
    * Method 2
      * Method 2a
      * Method 2b
    * Method 3
      * Method 3a
      * Method 3b


## Writing Clean Comments
****
**Remind two points:**
- Prefer expressive code over comments
- Use comments when code is not sufficient

**Comments to Avoid:**
- Redundant
- Intent
- Apology
- Warning
- Zombie Code
- Divider
- Brace Tracker
- Bloated Header
- Defect Log

Let's go ahead and walk through these points one by one.


### Redundant Comments:
```javascript
int i = 1   // Set i = 1

var cory = new User()   // Instantiate a new user

// <summary>
// Default Constructor
// </summary>
public User()
{
}

// <summary>
// Calculates Total Charges
// </summary>
public CalculateTotalCharges()
{
    // Total charges calculated here
}
```

**_Problem with Redundant Comments:_**
* Break the **DRY** principle. They are needless repetition.
* Comments lower the signal-to-noise ration.
* It is not necessary to add a comment on every single method, if your method name is well.

**_Two Rules to Avoid Redundant Comments:_**
* Assume your reader can read code
* Don't repeat yourself (DRY)


### Intent Comment:
* Bad Approach:
    ```javascript
    // Assure user's account is deactivated
    if (user.Status == 2)
    {
    }
    ```
* Good Approach:
    ```javascript
    if (user.Status == Status.Inactive)
    {
    }
    ```
* Can often eliminate intent comments via a well-named constant or enum.
* Clarify intent without comments:
  * Improve function name
  * Declare intermediate variable
  * Declare a constant or enum
  * Extract conditional to function


### Apology Comments:
```javascript
// Sorry, this crashes sometimes so i'm swallowing the exception

// I was tried to refactor this pile when I was done...
```
* Suggestions:
  * Don't apologize
  * Fix it before commit/merge
  * Add a TODO marker comment if you can't fix it that time


### Warning comments:
```javascript
// Here be dragons - see Bob

// Great sins against code below...
```


### Zombie Code:
`The code that are commented out code are zombie code`
```javascript
private bool ValidUsername(string username)
{
    bool isValid = false;
    const int MinUsernameLength = 6;
    if (username.Length >= MinUsernameLength)
    {
        const int MaxUsernameLength = 25;
        // bool isAlphaNumeric = username.All(char.IsLetterOrDigit);
        // if (isAlphaNumeric)
        // {
        //     if (!ContainCurseWords(username))
        //     {
        //         isValid = IsUniqueUsername(username);
        //     }
        // }
        if (username.Length <= MaxUsernameLength)
        {
           bool isAlphaNumeric = username.All(char.IsLetterOrDigit);
           if (isAlphaNumeric)
           {
               if (!ContainCurseWords(username))
               {
                   isValid = IsUniqueUsername(username);
               }
           }
        // if (!ContainCurseWords(username))
        // {
        //     isValid = IsUniqueUsername(username);
        // }
        }
    }
    return isValid;
}
```
- Zombie code is not really "dead".
- Commented code is "alive" because it's still in the code. It effects searches and refactoring.
- Causes for the ongoing problem of zombie code
  - **Risk Aversion**
    - Developers think they use code next time which is not needed now, so they comment it out.
  - **Hoarding Mentality**
    - Developers comment out code just in case it might be useful to someone later
    - They should remember: _With source control, deleted code can be retrieved later if needed_

- Zombie code:
  - Reduces readability
  - Created ambiguity
  - Hinders refactoring
  - Add noise to searches
  - Code is not "lost" anyway


### Divider comment:
* avoid this way
* if functions are too long, refactor it to separate the function with multiple functions
```javascript
private void myLongFunction()
{
    // lots
    // of
    // codes
    
    // Start search for available concert tickets

    // lots
    // of
    // concert
    // search
    // codes
    
    // End of concert ticket search
    
    // lots
    // more
    // code
}
```


### Brace Tracker Comment:
* These comments use for tracing long statements brace
* Long coding are the only reason to use this comment
* with Brace Tracker Comment:
    ```javascript
    private void AuthenticateUsers()
    {
        bool validLogin = false;
        // deeply
          // nested
            // code
            if (validLogin)
            {
                // lots
                // of
                // codes
                // here
            } // end user login
          // even
        // more code
    }
    ```
* After Refactor:
    ```javascript
    private void AuthenticateUsers()
    {
        bool validLogin = false;
        // deeply
          // nested
            // code
            if (validLogin)
            {
                loginUser();
            }
          // even
        // more code
    }
    ```
  
    - Benefits:
      - Enhances readability
      - Reduces cyclomatic complexity


### Clean Comments:
**_To Do Comments:_**
```javascript
// TODO Refactor out duplication
// HACK The API doesn't expose needed call yet
```
* Maintain:
  * Standardize
  * Watch Out:
    * Maybe an apology or warning comment in disguise
    * Often ignored

**_Summary Comments:_**
```javascript
// Encapsulates logic for calculating retire benefits
// Generates custom newslatter emails
```
* Maintain:
    * Describe intent at general level higher than the code
    * Often useful to provide high level overview of classes
    * **Risk:** Don't use to simply augment poor naming


**_Documentation:_**
```javascript
// See microsoft.com/api for documentation
```
* Maintain:
    * Useful when it can't be expressed in code

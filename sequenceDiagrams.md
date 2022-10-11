# Access Dashboard
### Scenario 1 - User creates an account on the application

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant LoginUI as #lt;#lt;boundary#gt;#gt;<br>LoginUI
    participant UserC as #lt;#lt;control#gt;#gt;<br>UserController
    participant DataC as #lt;#lt;control#gt;#gt;<br>DatabaseController
    participant Data as #lt;#lt;entity#gt;#gt;<br>Database
    participant Profile as #lt;#lt;entity#gt;#gt;<br>Profile
    User    ->>+    LoginUI : register()
    LoginUI ->>+    UserC   : register()
    UserC   ->>+    DataC   : add_profile()
    DataC   -->     Profile : new Profile()
    DataC   -->     Data    : add credentials
    DataC   -->>-   UserC   : return Profile
    UserC   -->>-   LoginUI : resolve login
    LoginUI -->>-   User    : redirect to /dashboard
```
### Scenario 2 - User logs in to the application (correct credentials)

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant LoginUI as #lt;#lt;boundary#gt;#gt;<br>LoginUI
    participant UserC as #lt;#lt;control#gt;#gt;<br>UserController
    participant DataC as #lt;#lt;control#gt;#gt;<br>DatabaseController
    participant Data as #lt;#lt;entity#gt;#gt;<br>Database
    participant Profile as #lt;#lt;entity#gt;#gt;<br>Profile
    User    ->>+    LoginUI : login()
    LoginUI ->>+    UserC   : login()
    Note over UserC         : self.build_user_credentials()
    activate UserC
    UserC   ->>+    DataC   : match_profile()
    DataC   -->     Data    : match credentials
    DataC   -->     Profile : new Profile(...)
    DataC   -->>-   UserC   : return Profile
    deactivate UserC
    UserC   -->>-   LoginUI : resolve login
    LoginUI -->>-   User    : redirect to /dashboard
```
### Scenario 3 - User logs in to the application (incorrect credentials)

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant LoginUI as #lt;#lt;boundary#gt;#gt;<br>LoginUI
    participant UserC as #lt;#lt;control#gt;#gt;<br>UserController
    participant DataC as #lt;#lt;control#gt;#gt;<br>DatabaseController
    participant Data as #lt;#lt;entity#gt;#gt;<br>Database
    User    ->>+    LoginUI : login()
    LoginUI ->>+    UserC   : login()
    Note over UserC         : self.build_user_profile()
    activate UserC
    UserC   ->>+    DataC   : match_profile()
    DataC   -->     Data    : match credentials
    DataC   -->>-   UserC   : return None
    deactivate UserC
    UserC   -->>-   LoginUI : reject login
    LoginUI -->>-   User    : redirect to /login
```
### Scenario 4 - User links external services provider to the application

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant DashUI as #lt;#lt;boundary#gt;#gt;<br>DashboardUI
    participant UserC as #lt;#lt;control#gt;#gt;<br>UserController
    participant DataC as #lt;#lt;control#gt;#gt;<br>DatabaseController
    participant Data as #lt;#lt;entity#gt;#gt;<br>Database
    User    ->>+    DashUI  : link_account()
    DashUI  ->>+    UserC   : link_account()
    UserC   ->>+    DataC   : update_profile()
    DataC   -->     Data    : match credentials
    DataC   -->     Data    : update profile
    DataC   -->>-   UserC   : return Profile
    UserC   -->>-   DashUI  : done
    DashUI  -->>-   User    : reload dashboard
```
### Scenario 5 - User unlinks external service provider

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant DashUI as #lt;#lt;boundary#gt;#gt;<br>DashboardUI
    participant UserC as #lt;#lt;control#gt;#gt;<br>UserController
    participant DataC as #lt;#lt;control#gt;#gt;<br>DatabaseController
    participant Data as #lt;#lt;entity#gt;#gt;<br>Database
    User    ->>+    DashUI  : unlink_account()
    DashUI  ->>+    UserC   : unlink_account()
    UserC   ->>+    DataC   : update_profile()
    DataC   -->     Data    : match credentials
    DataC   -->     Data    : update profile
    DataC   -->>-   UserC   : return Profile
    UserC   -->>-   DashUI  : done
    DashUI  -->>-   User    : reload dashboard
```
### Scenario 6 - User submits requirements via the application

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant ClassUI as #lt;#lt;boundary#gt;#gt;<br>ClassesUI
    participant LearnC as #lt;#lt;control#gt;#gt;<br>LearningEnvController
    participant LearnE as #lt;#lt;entity#gt;#gt;<br>LearningEnvClass
    User    ->>+    ClassUI : submit_assignment()
    ClassUI -->>    User    : request confirmation
    User    -->>    ClassUI : confirm
    ClassUI ->>+    LearnC  : submit_assignment()
    LearnC  -->     LearnE  : upload assignment
    LearnC  -->>-   ClassUI : done
    ClassUI -->>-   User    : done
```

# View Requirements
### Scenario 1 - User accessed dashboard view
```mermaid
sequenceDiagram
    autonumber
    actor User
    participant DashUI as #lt;#lt;boundary#gt;#gt;<br>DashboardUI
    participant LearnC as #lt;#lt;control#gt;#gt;<br>LearningEnvController
    participant LearnE as #lt;#lt;entity#gt;#gt;<br>LearningEnvClass
    User    ->>+    DashUI  : display()
    Note over DashUI        : self.display_deadlines()
    activate DashUI
    DashUI  ->>+    LearnC  : get_deadlines()
    LearnC  -->     LearnE  : get deadlines
    LearnC  -->>-   DashUI  : return deadlines
    deactivate DashUI
    DashUI  -->>-   User    : return dashboard
```
### Scenario 2 - User accessed calendar view
```mermaid
sequenceDiagram
    autonumber
    actor User
    participant TimeUI as #lt;#lt;boundary#gt;#gt;<br>CalendarUI
    participant LearnC as #lt;#lt;control#gt;#gt;<br>LearningEnvController
    participant LearnE as #lt;#lt;entity#gt;#gt;<br>LearningEnvClass
    User    ->>+    TimeUI  : display()
    Note over TimeUI        : self.display_deadlines()
    activate TimeUI
    TimeUI  ->>+    LearnC  : get_deadlines()
    LearnC  -->     LearnE  : get deadlines
    LearnC  -->>-   TimeUI  : return deadlines
    deactivate TimeUI
    TimeUI  -->>-   User    : return calendar
```
### Scenario 3 - User accessed class card view
```mermaid
sequenceDiagram
    autonumber
    actor User
    participant ClassUI as #lt;#lt;boundary#gt;#gt;<br>ClassesUI
    participant LearnC as #lt;#lt;control#gt;#gt;<br>LearningEnvController
    participant LearnE as #lt;#lt;entity#gt;#gt;<br>LearningEnvClass
    User    ->>+    ClassUI : display()
    Note over ClassUI       : self.display_deadlines()
    activate ClassUI
    ClassUI ->>+    LearnC  : get_deadlines()
    LearnC  -->     LearnE  : get deadlines
    LearnC  -->>-   ClassUI : return deadlines
    deactivate ClassUI
    Note over ClassUI       : self.display_classes()
    activate ClassUI
    ClassUI ->>+    LearnC  : get_classes()
    LearnC  -->     LearnE  : get classes
    LearnC  -->>-   ClassUI : return classes
    deactivate ClassUI
    ClassUI -->>-   User    : return classes
```
# Access Dashboard
### Scenario 1 - User creates an account on the application

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant LoginUI as &lt;&lt;boundary&gt;&gt;<br>LoginUI
    participant UserC as &lt;&lt;control&gt;&gt;<br>UserController
    participant DataC as &lt;&lt;control&gt;&gt;<br>DatabaseController
    participant Data as &lt;&lt;entity&gt;&gt;<br>Database
    participant Profile as &lt;&lt;entity&gt;&gt;<br>Profile
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
    participant LoginUI as &lt;&lt;boundary&gt;&gt;<br>LoginUI
    participant UserC as &lt;&lt;control&gt;&gt;<br>UserController
    participant DataC as &lt;&lt;control&gt;&gt;<br>DatabaseController
    participant Data as &lt;&lt;entity&gt;&gt;<br>Database
    participant Profile as &lt;&lt;entity&gt;&gt;<br>Profile
    User    ->>+    LoginUI : login()
    LoginUI ->>+    UserC   : login()
    UserC   ->>+    UserC   : build_user_credentials()
    UserC   ->>+    DataC   : match_profile()
    DataC   -->     Data    : match credentials
    DataC   -->     Profile : new Profile(...)
    DataC   -->>-   UserC   : return Profile
    UserC   -->>-   UserC   : return Profile
    UserC   -->>-   LoginUI : resolve login
    LoginUI -->>-   User    : redirect to /dashboard
```

### Scenario 3 - User logs in to the application (incorrect credentials)

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant LoginUI as &lt;&lt;boundary&gt;&gt;<br>LoginUI
    participant UserC as &lt;&lt;control&gt;&gt;<br>UserController
    participant DataC as &lt;&lt;control&gt;&gt;<br>DatabaseController
    participant Data as &lt;&lt;entity&gt;&gt;<br>Database
    User    ->>+    LoginUI : login()
    LoginUI ->>+    UserC   : login()
    UserC   ->>+    UserC   : build_user_profile()
    UserC   ->>+    DataC   : match_profile()
    DataC   -->     Data    : match credentials
    DataC   -->>-   UserC   : return None
    UserC   ->>-    UserC   : return None
    UserC   -->-    LoginUI : reject login
    LoginUI -->>-   User    : redirect to /login
```

### Scenario 4 - User links external services provider to the application

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant DashUI as &lt;&lt;boundary&gt;&gt;<br>DashboardUI
    participant UserC as &lt;&lt;control&gt;&gt;<br>UserController
    participant DataC as &lt;&lt;control&gt;&gt;<br>DatabaseController
    participant Data as &lt;&lt;entity&gt;&gt;<br>Database
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
    participant DashUI as &lt;&lt;boundary&gt;&gt;<br>DashboardUI
    participant UserC as &lt;&lt;control&gt;&gt;<br>UserController
    participant DataC as &lt;&lt;control&gt;&gt;<br>DatabaseController
    participant Data as &lt;&lt;entity&gt;&gt;<br>Database
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
    participant ClassUI as &lt;&lt;boundary&gt;&gt;<br>ClassesUI
    participant LearnC as &lt;&lt;control&gt;&gt;<br>LearningEnvController
    User    ->>+    ClassUI : submit_assignment()
    ClassUI -->>    User    : request confirmation
    User    -->>    ClassUI : confirm
    ClassUI ->>+    LearnC  : submit_assignment()
    LearnC  -->>    LearnC  : upload assignment
    LearnC  -->>-   ClassUI : done
    ClassUI -->>-   User    : done
```

# View Requirements

### Scenario 1 - User accessed dashboard view
### Scenario 2 - User accessed calendar view
### Scenario 3 - User accessed class card view
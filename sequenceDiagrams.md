# Access Dashboard
### Scenario 1 - User creates an account on the application

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant LoginUI as <<boundary>><br>LoginUI
    participant UserC as <<control>><br>UserController
    participant DataC as <<control>><br>DatabaseController
    participant Data as <<entity>><br>Database
    participant Profile as <<entity>><br>Profile
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
    participant LoginUI as <<boundary>><br>LoginUI
    participant UserC as <<control>><br>UserController
    participant DataC as <<control>><br>DatabaseController
    participant Data as <<entity>><br>Database
    participant Profile as <<entity>><br>Profile
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
    participant LoginUI as <<boundary>><br>LoginUI
    participant UserC as <<control>><br>UserController
    participant DataC as <<control>><br>DatabaseController
    participant Data as <<entity>><br>Database
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
    participant DashUI as <<boundary>><br>DashboardUI
    participant UserC as <<control>><br>UserController
    participant DataC as <<control>><br>DatabaseController
    participant Data as <<entity>><br>Database
    User    ->>+    DashUI  : link_account()
    DashUI  ->>+    UserC   : link_account()
    UserC   ->>+    DataC   : update_profile()
    DataC   -->     Data    : match credentials
    DataC   -->     Data    : update profile
    DataC   -->>-   UserC   : return Profile
    UserC   -->>-   DashUI  : done
    DashUI  -->>-   User    : reload dashboard
```

### Scenario 5 - User unlinks external services provider to the application

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant DashUI as <<boundary>><br>DashboardUI
    participant UserC as <<control>><br>UserController
    participant DataC as <<control>><br>DatabaseController
    participant Data as <<entity>><br>Database
    User    ->>+    DashUI  : unlink_account()
    DashUI  ->>+    UserC   : unlink_account()
    UserC   ->>+    DataC   : update_profile()
    DataC   -->     Data    : match credentials
    DataC   -->     Data    : update profile
    DataC   -->>-   UserC   : return Profile
    UserC   -->>-   DashUI  : done
    DashUI  -->>-   User    : reload dashboard
```
```mermaid
classDiagram
    direction LR
    DisplayUI <|.. ClassesUI
    DisplayUI <|.. DashboardUI
    DisplayUI <|.. CalendarUI

    DatabaseController "1" -- "1" UserController
    DatabaseController "1" -- "1" Database
    Database "1" o-- "1..*" Profile
    Profile "1" o-- "1..*" LearningEnvCredentials 

    UserController "1" -- "1" LoginUI

    LearningEnvController "1" -- "1" ClassesUI
    LearningEnvController "1" -- "1" DashboardUI
    LearningEnvController "1" -- "1" CalendarUI
    LearningEnvController "1" -- "1" UserController
    LearningEnvController "1" *-- "1..*" LearningEnvClass

%% Entities
    %% interface
    class LearningEnvClass {
        <<entity>>
        -string id
        -string name
        -string description
        -string url
    }

    %% interface
    class LearningEnvCredentials {
        <<entity>>
    }

    class Profile {
        <<entity>>
        -string id
        -string name
        -Dict~string, LearningEnvCredentials~ accounts
    }

    class Database {
        <<entity>>
        -Dict~string, Profile~ user_profiles
    }

%% Controls
    %% abstract
    class LearningEnvController {
        <<control>>
        -string id
        -string name
        +get_class(string id) LearningEnvClass
        +get_classes() LearningEnvClass[]
        +get_deadline(string id) Deadline
        +get_deadlines() Deadline[]
        +submit_assignment(string courseID, string assignmentID, Blob[] files)
    }

    class DatabaseController {
        <<control>>
        +match_profile(Credentials creds) Profile
        +add_profile(Credentials creds, Profile profile?) Profile
        +remove_profile(Credentials creds)
        +update_profile(Credentials creds, Profile profile) Profile
    }

    class UserController {
        <<control>>
        +register(Credentials creds)
        +login(Credentials creds)
        +logout()
        +update_user_account(Dict options)
        +link_account(LearningEnvCredentials creds)
        +unlink_account(string id)
        -build_user_profile() Profile
    }

%% Boundaries
    class LoginUI {
        <<boundary>>
        +login()
        +register()
    }

    %% interface
    class DisplayUI {
        <<boundary>>
        +display(Dict options?)
    }

    class DashboardUI~DisplayUI~ {
        <<boundary>>
        +link_account(Credentials creds)
        +unlink_account(string id)
        +delete_account()
        -display_deadlines(Dict options?)
    }

    class CalendarUI~DisplayUI~ {
        <<boundary>>
        -display_deadlines(Dict options?)
    }

    class ClassesUI~DisplayUI~ {
        <<boundary>>
        +submit_assignment(string courseID, string assignmentID, Blob[] files)
        -display_classes(Dict options?)
        -display_deadlines(Dict options?)
    }
```
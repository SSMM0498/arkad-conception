- [INTRODUCTION](#introduction)
  - [CONTEXTUALIZATION](#contextualization)
  - [PRESENTATION](#presentation)
- [CONCEPTION](#conception)
  - [LEXIQUE](#lexique)
  - [ACTORS](#actors)
    - [VIEWER](#viewer)
    - [CREATOR](#creator)
    - [ADMINISTRATOR](#administrator)
  - [USE CASES](#use-cases)
  - [USE CASE TEXTUAL DESCRIPTION](#use-case-textual-description)
    - [REGISTER](#register)
    - [REGISTER WITH GOOGLE/GITHUB](#register-with-googlegithub)
    - [LOG IN](#log-in)
    - [SIGN OUT](#sign-out)
    - [SEARCH ARKAST](#search-arkast)
    - [USE ARKADOR](#use-arkador)
- [ARCHITECTURE](#architecture)
- [USER INTERFACE](#user-interface)


#   INTRODUCTION

##  CONTEXTUALIZATION
To learn the code, people have a lot of possibilities. Some of these solutions are chargeable and others are free. But platforms also use different methods for teaching.Those methods allow users to:
+ Watch videos that are recorded while the teacher codes and explains; like many youtube channels or udemy teacher do,
+ Read a bit, type the code directly into the browser, and immediately see the results like Codecademy and freeCodeCamp do,
+ Take on challenges to progress to higher ranks like CodeWars.

It can be seen that some of these methods try to focus on practice because coding is above all a practical activity. However, this type of platform often does not use video for its lessons. But learning with video tutorials is very effective, the teacher can explain the whole coding process. However the big problem with video is that it destroys all possibilities for interactivity and don't ease praticing. The code you watch is inert, you can neither copy, edit or run the code. All you can do is watch it and often for practising you are bound to go back and forth between your text editor and the video. It is very tedious and a little boring. Especially as interactivity during teaching between teacher and students is fully possible in online learning, but not with this old-fashioned video format. In addition what would have taken you five minutes to explain in-person, can often take an hour to explain online because the creation process for those videos are tedious and frustrating for teacher.
That's why our platform is created to combine these 2 things. We used a different video format created for learning while practising. This one allows users to interact with the code directly in the player and make the learning experience more efficient.

##  PRESENTATION
The plateform is named arkad. This name is use to reference of the gaming world because his purpose is to offer a fun and easy way to learn coding. The simplicity is that you just have to go to the plateform, click RECORD and start talking while you code. The core of the plateform is our recorder and replayer program. With this one we store exactly all actions you did when you recorded your video and replay it for the viewer which can pause it at any time copy, edit or run the code if they want. Thereby users can share their coding skills, tips easely and learn from others in a better way. We name this kind of video arkast which is a merge between arkad and screencast.

#   CONCEPTION

##  LEXIQUE
1.  Arkad
2.  Arkador
3.  Arkast
4.  Creator
5.  Follow
6.  Follower
7.  Record
8.  Recorder
9.  Watch
10. Player
11. Tag word

##  ACTORS
### VIEWER 
Viewers are an unregister users. They can list and watch arkasts but can't comment it, take notes... They are not able to create a arkast or follow an creator. They must register to unlock all this features.

### CREATOR
Creators can perform all viewer's actions and can additionnaly use the arkad recorder to create an arkast and publy it. They can also comment an arkast and take notes and interact with other creator.

### ADMINISTRATOR
Administrators manage the arkad plateform. They have access to all accounts and arkasts and can delete them at any time. They can send a message to the creator. All the advertising system are managed by them. They can modify the layouts, texts, images of the platform.

##  USE CASES
+   Register : User creates a new account.
    +   With Google
    +   With Github
+   Log In : User logs in to the system.
    +   With Google
    +   With Github
+   Sign Out : User logs out from the system.
+   Search Arkast : User enters a search term.
+   Use Arkador : User use the Arkador
    +   Manage Files
    +   Type Code
    +   Use Mini-Browsing
    +   Use Slides
    +   Record Session
+   Create Arkast : User creates a arkast.
+   Watch Arkast : User watches a arkast.
    +   Play Arkast
    +   Pause Arkast
    +   Change Arkast
    +   Change Volume
    +   Manage Notes
        +   See Notes
        +   Take Notes
        +   Save/Delete Notes
        +   Share Notes
+   Evaluate Arkast
    +   Rate Arkast
    +   Comment Arkast
+   Manage Arkast : User manages their arkasts.
+   Manage Playlists : User manages their playlist.
+   Manage Account : User performs edit profile or delete account.
    +   Editor Account
    +   Delete Account
+   Interact with Creator : User interact with creator
    +   Follow/Unfollow Creator
    +   Report/Unreport Creator
    +   Reply Creator's comment
+   Manage Account : Administrator performs manages creator accounts.
    +   List Account
    +   Report Account
    +   Delete Account
+   Manage Arkast : Administrator manages a video from the database.
    +   List Arkast
    +   Report Arkast
    +   Delete Arkast

All the Include and Extends relationships can be understood from the diagram. As the diagram is large, It is divided into 2 parts. Hence, the Use Case Diagram of Online Video Database Management System is :

##  USE CASE TEXTUAL DESCRIPTION

### REGISTER

| Name | Register to the plateform |
|:--|--:|
| Actor(s) | Users |
| Description | Users create an account and unlock all feature of the plateform |
| Preconditions | none |
| Beginning | Viewer requested the "Register page" |
| **Process** |
| Nominal Scenario |
| 1. | System displays page containing a list of input. | 
| 2. | The user enters his name. |
| 3. | The user enters his username. |
| 3. | The user enters his email. |
| 4. | The user enters his password. |
| 5. | The user types again his password to confirm it. |
| 6. | The user submits his those typed information. |
| 7. | The system checks if username or email are not all ready insert in the database |
| 8. | The system creates the user account. |
| 9. | The system puts the user logged in. |
| 10. | The system returns to the display the last page before the User requester this page|
| Alternatif Scenario |
| 6.a | The user doesn't fill all inputs |
| 6.b | The user chooses an already take username |
| 6.c | The user chooses an already take email |
| 6.d | The user writes differents password |
| 6.e | The user writes differents password |
| 6.f | The user's password is not almost strong |
| End | Nominal scenario : in steps 2, 3, 4, 5 or 6, by decision of the user |
| Postconditions | The user is logged in |
| **Complements** |
| Ergonomics |
|  |
| Expected performance |
|  |
| Unresolved issues |
|  |

### REGISTER WITH GOOGLE/GITHUB

| Name | Register to the plateform |
|:--|--:|
| Actor(s) | Users |
| Description | Users create an account and unlock using his Google/Github account all feature of the plateform |
| Preconditions | none |
| Beginning | Viewer requested the "Register page"
| **Process** |
| Nominal Scenario |
| 1. | System displays page containing a list of input. | 
| 2. | The user clicks on the resigter with Google/Github button. |
| 3. | The system init the Google/Github authentification process. |
| 4. | The system retrieves user informations from Google/Github server |
| 5. | The system creates the user account. |
| 6. | The system puts the user logged in. |
| 7. | The system returns to the display profile edition page|
| Alternatif Scenario |
| 4.a | The system cannot retrives the user informations. |
| End | Nominal scenario : in steps 2, 3, 4, 5 or 6, by decision of the user |
| Postconditions | The user is logged in |
| **Complements** |
| Ergonomics |
|  |
| Expected performance |
|  |
| Unresolved issues |
|  |

### LOG IN

| Name | Log in to the plateform |
|:--|--:|
| Actor(s) | Users |
| Description | Users log in to his account and have access to their wall |
| Preconditions | The users already have an account in the plateform |
| Beginning | The user requested the "Login page"
| **Process** |
| Nominal Scenario |
| 1. | System displays page containing a list of input. |
| 2. | The user enters his username/email. |
| 3. | The user enters his password. |
| 4. | The user submits the form. |
| 5. | The system checks if this combination username/email and password exist in the database |
| 6. | The system puts the user logged in. |
| 7. | The system returns to the display the last page before the User requester this page|
| Alternatif Scenario |
| 6.a | The user doesn't fill all inputs |
| 6.b | The system doesn't find this combination of username and password |
| End | Nominal scenario : in steps 2, 3 or 4, by decision of the user |
| Postconditions | The user is logged in |
| **Complements** |
| Ergonomics |
|  |
| Expected performance |
|  |
| Unresolved issues |
|  |

### SIGN OUT

| Name | Sign out from the platform |
|:--|--:|
| Actor(s) | Users |
| Description | Users sign out to his account and can only watch arkast from now |
| Preconditions | The users already is logged in the plateform |
| Beginning | The user click the "Sign out button"
| **Process** |
| Nominal Scenario |
| 1. | The system puts the user offline in the database. |
| 2. | The system deletes all session and environment variables |
| 3. | The system returns to the display the last page before the User sign out|
| Alternatif Scenario |
| none |
| End |
| Postconditions | The user is logged out |
| **Complements** |
| Ergonomics |
|  |
| Expected performance |
|  |
| Unresolved issues |
|  |

### SEARCH ARKAST

| Name | Search for an arkast |
|:--|--:|
| Actor(s) | Users |
| Description | Users use searching input to look for an arkast by using tag word |
| Preconditions | None |
| Beginning | The user click in the "Searching input"
| **Process** |
| Nominal Scenario |
| 1. | The user enters the keyword what he was looking for |
| 2. | The user submits this typing |
| 3. | The system checks if this typing is contained in an arkast title, description in the database |
| 4. | The system displays all matching arkast |
| Alternatif Scenario |
| 1.a | The user enters a keyword that he has already entered before |
| 3.a | The system doesn't find this keyword in all arkast in the database |
| End | Nominal scenario : in steps 1, 2, by decision of the user |
| Postconditions | The user is logged in |
| **Complements** |
| Ergonomics |
|  |
| Expected performance |
|  |
| Unresolved issues |
|  |

### USE ARKADOR

| Name | Use Arkador |
|:--|--:|
| Actor(s) | Users |
| Description | Users use the arkad integrated text editor : arkador |
| Preconditions | The users already have an account in the plateform |
| Beginning | The user pauses the arkast he is watching or he is recording an arkast
| **Process** |
| Nominal Scenario |
| 1. | System displays page containing all Arkador components |
| 2. | The user types some code in the text editor |
| 3. | The user clicks to the run code |
| 5. | The system parses and format this code |
| 6. | The system executes the code |
| 7. | The system displays the result in the Mini-Browser |
| Alternatif Scenario |
| 5.a | The system detects some errors in the code |
| End | Nominal scenario : in steps 2 or 3 by decision of the user |
| Postconditions | The user see the code result |
| **Complements** |
| Ergonomics |
|  |
| Expected performance |
|  |
| Unresolved issues |
|  |



#   ARCHITECTURE
#   USER INTERFACE

1.  Login Page
    Users can enter User Id and Password to login or can create a new account.
2.  Search Page
    This page is opened after a successful login. Users can enter a search term to search any video. The search results are displayed in the non-header part of the page. Each search result shows name, thumbnail, download button (for free videos), like and dislike icons, number of views etc., corresponding to a video. Also, an upload button is located at the top right corner of the page.
1.  Record Page
    If on the search page, download button corresponding to a video is clicked, a dialog box is displayed asking to set resolution and format. After selection of required resolution and format, this page is opened showing the percentage of download completed etc,. User cannot do any work until the download is completed or cancelled. So, network connection must be good.
2.  Watch Page
    If on the search page, name corresponding to a video is clicked, this page is opened if the video is a free video else a dialog box requesting to rent/purchase video is opened. A Non Free video is displayed if and only if it is taken for rent or purchased by the user. Unlike YouTube, video is shown full screen only and only play/pause, replay, close etc,. buttons are present on this page.
3.  Profile Page
    If on the search page, upload button is clicked, this page is opened and asks to select a video and fill the details required. After selection of video and filling the details, the video is uploaded to the administrator database and gets ready for processing.
4.  Process Page
    After the upload of video into the administrator database from the upload page, this page opens automatically showing the details of processing.
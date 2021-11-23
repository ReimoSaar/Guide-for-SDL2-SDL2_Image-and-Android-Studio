# Guide-for-SDL2-and-Android-Studio-for-Linux(Ubuntu)

1) Download JDK. sudo apt install openjdk-8-jdk
2) Download Android Studio (at the time, my version was 2020.3.1)
3) Download SDL2 from https://www.libsdl.org/download-2.0.php
4) Download SDL2_Image from https://www.libsdl.org/projects/SDL_image/
5) Create a folder called androidlib under /home/user and
6) Extract SDL2 and SDL2_image into androidlib folder
7) Start Android Studio
8) Press next after reaching here and download SDK components.
![as-initial-download](https://user-images.githubusercontent.com/63605374/140167045-06e68ff3-8d30-428e-96be-2d374a1ec1ce.png)
9) Click More Actions -> Import Project
![as-welcome-page](https://user-images.githubusercontent.com/63605374/140644819-55a3dcd9-1d8e-450a-8cbe-c45a37d04c89.png)
10) Choose android project under /home/user/androidlib/                
![import-project](https://user-images.githubusercontent.com/63605374/140644823-0342f204-5807-4cda-8871-31d60f7039de.png)
11) Move your project folder to a more suitable location. (e.g /home/user/Projects/)
12) Press ctrl+shift+a and search for SDK Manager.
![search-sdk-manager](https://user-images.githubusercontent.com/63605374/141821433-91b35b70-95cd-4b39-b82f-7aa569559e8a.png)
13) Click SDK Tools tab and check Show Package Details, pick SDK version (I picked SDK 31.0.0).
![specify-sdk-version](https://user-images.githubusercontent.com/63605374/141822037-ffd152d9-af78-4e42-a22b-3fc0369221c1.png)
14) Also check Android SDK Command-line Tools!
[install-SDK-cmd-line-tools](https://user-images.githubusercontent.com/63605374/143089491-dfe8200b-69ed-4853-917c-88e316905eec.png)
15) And finally, check Android NDK. I installed version 21.1.6352462 because newer version seem to be missing a "platforms" directory
![install-NDK](https://user-images.githubusercontent.com/63605374/143092077-bf916904-0064-499c-86df-0f5439584113.png)
16) Sync project with gradle files. To do that, either press Try Again button or press ctrl+shift+a and write Sync project with gradle files
![try_again_button](https://user-images.githubusercontent.com/63605374/141829161-7c569803-9192-4c70-a934-83cde4614fa2.png)
![sync-project-with-gradle-files](https://user-images.githubusercontent.com/63605374/141829178-e3de73c4-f1cf-49a6-b313-d70c8f96f59c.png)
17) You should get two warnings and one error. Both warnings say the following: "license for package android sdk platform <version> not accepted". To fix it, go to /home/user/Android/sdk/tools/bin/ and run ./sdkmanager --licenses

  
  For more help, also check out https://wiki.libsdl.org/Android

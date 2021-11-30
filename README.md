# Guide-for-SDL2-and-Android-Studio-for-Linux(Ubuntu)

1) Download JDK. sudo apt install openjdk-8-jdk
2) Download Android Studio (at the time, my version was 2020.3.1)
3) Download SDL2 source code from https://www.libsdl.org/download-2.0.php
4) Download SDL2_Image from https://www.libsdl.org/projects/SDL_image/
5) Create a folder called androidlib under /home/user
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
15) And finally, check Android NDK and click Apply. I installed version 21.1.6352462 because newer version seem to be missing a "platforms" directory
![install-NDK](https://user-images.githubusercontent.com/63605374/143092077-bf916904-0064-499c-86df-0f5439584113.png)
16) Sync project with gradle files. To do that, either press Try Again button or press ctrl+shift+a and write Sync project with gradle files
![try_again_button](https://user-images.githubusercontent.com/63605374/141829161-7c569803-9192-4c70-a934-83cde4614fa2.png)
![sync-project-with-gradle-files](https://user-images.githubusercontent.com/63605374/141829178-e3de73c4-f1cf-49a6-b313-d70c8f96f59c.png)
17) You should get two warnings and one error. Both warnings say the following: "license for package android sdk platform <version> not accepted". To fix it, go to /home/user/Android/sdk/tools/bin/ and run ./sdkmanager --licenses
18) If you sync project with gradle files again you shuld get the following error: NDK not configured. To fix it, add the following line to application.properties below sdk.dir: ndk.dir=/home/user/Android/Sdk/ndk/21.1.6352462
![add-NDK](https://user-images.githubusercontent.com/63605374/143287962-a44516d2-51de-45f6-8dc4-a502c97f369e.png)
19) Sync project with gradle again and you should get BUILD SUCCESSFUL.
20) Click No Devices -> AVD Manager
![choose-AVD-manager](https://user-images.githubusercontent.com/63605374/143486632-15136d22-c296-4f8b-a096-53b71d330a31.png)
21) Then Press Create Virtual Device
22) Select Hardware (I chose Pixel 4XL) and press Next
23) Verify configuration and click Finish
23) Select system image (I chose Oreo Android 8.1)
24) Create a file under jni/src called main.cpp
![add-main](https://user-images.githubusercontent.com/63605374/143470114-4cda1784-f5b9-47f4-b314-6c6145d826f6.png)
25) Write the following inside main.cpp:

```
#include "SDL.h"
#include "SDL_image.h"

/* XPM */
static char * icon_xpm[] = {
  "32 23 3 1",
  "     c #FFFFFF",
  ".    c #000000",
  "+    c #FFFF00",
  "                                ",
  "            ........            ",
  "          ..++++++++..          ",
  "         .++++++++++++.         ",
  "        .++++++++++++++.        ",
  "       .++++++++++++++++.       ",
  "      .++++++++++++++++++.      ",
  "      .+++....++++....+++.      ",
  "     .++++.. .++++.. .++++.     ",
  "     .++++....++++....++++.     ",
  "     .++++++++++++++++++++.     ",
  "     .++++++++++++++++++++.     ",
  "     .+++++++++..+++++++++.     ",
  "     .+++++++++..+++++++++.     ",
  "     .++++++++++++++++++++.     ",
  "      .++++++++++++++++++.      ",
  "      .++...++++++++...++.      ",
  "       .++............++.       ",
  "        .++..........++.        ",
  "         .+++......+++.         ",
  "          ..++++++++..          ",
  "            ........            ",
  "                                "};

int main(int argc, char *argv[])
{
  SDL_Window *window;
  SDL_Renderer *renderer;
  SDL_Surface *surface;
  SDL_Texture *texture;
  int done;
  SDL_Event event;

  if (SDL_CreateWindowAndRenderer(0, 0, 0, &window, &renderer) < 0) {
    SDL_LogError(SDL_LOG_CATEGORY_APPLICATION,
        "SDL_CreateWindowAndRenderer() failed: %s", SDL_GetError());
    return(2);
  }

  surface = IMG_ReadXPMFromArray(icon_xpm);
  texture = SDL_CreateTextureFromSurface(renderer, surface);
  if (!texture) {
    SDL_LogError(SDL_LOG_CATEGORY_APPLICATION,
        "Couldn't load texture: %s", SDL_GetError());
    return(2);
  }
  SDL_SetWindowSize(window, 800, 480);

  done = 0;
  while (!done) {
    while (SDL_PollEvent(&event)) {
      if (event.type == SDL_QUIT)
        done = 1;
    }
    SDL_RenderCopy(renderer, texture, NULL, NULL);
    SDL_RenderPresent(renderer);
    SDL_Delay(100);
  }
  SDL_DestroyTexture(texture);

  SDL_Quit();
  return(0);
}
```

26) Modify jni/src/Android.mk. Replace LOCAL_SRC_FILES := YourSourceHere.c with LOCAL_SRC_FILES := main.cpp
27) Press run
![press-run](https://user-images.githubusercontent.com/63605374/143783195-a2851ecb-ea01-467d-bb20-ce4756336c39.png)
28) You got and error that says: Module main depends on undefined modules: SDL2. That`s because our program cant access SDL2 files. To fix it, add SDL2 to src/jni with symbolic link. command: ln -s /home/user/androidlib/SDL2-2.0.16/ SDL2
![symlinked SDL2](https://user-images.githubusercontent.com/63605374/143783722-7097df58-1476-4f18-8dc1-d6394adebcc3.png)
29) Also, symlink SDL_image to the same location: ln -s /home/reimo/androidlib/SDL2_image-2.0.5/ SDL2_image
30) next you have to run the following command: ~/Android/Sdk/ndk/21.1.6352462/ndk-build -C ~/Projects/android-project/app/jni/


For more help, also check out https://wiki.libsdl.org/Android

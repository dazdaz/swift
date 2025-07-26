## Step 1: Set Up the Project in Xcode

1.  **Open Xcode**: Launch Xcode (version 16 or later is recommended as of 2025).
2.  **Create a New Project**:
    * From the welcome screen, select **Create a new Xcode project**.
    * Alternatively, go to `File` > `New` > `Project`.
3.  **Choose a Template**:
    * Select **macOS** from the top tabs.
    * Choose **App** under `Application`.
    * Click **Next**.
4.  **Configure Options**:
    * **Product Name**: `HelloWorld` (or your preferred name).
    * **Team**: Select your Apple ID (sign in if needed). For personal use, "None" works but might limit some features.
    * **Bundle Identifier**: This will typically default to something like `com.yourname.HelloWorld`.
    * **Interface**: Choose **SwiftUI** (for simplicity).
    * **Language**: Select **Swift**.
    * Ensure **"Use Core Data"** and **"Include Tests"** are unchecked.
    * Click **Next**, then choose a save location for your project.

This process will create a basic SwiftUI project structure.


## Step 2: Write the Code

```
import SwiftUI

struct ContentView: View {
    var body: some View {
        Text("Hello, World!")
            .font(.largeTitle)  // Makes the text bigger for visibility
            .padding()  // Adds some space around the text
            .frame(minWidth: 300, minHeight: 200)  // Sets a minimum window size
    }
}
```

## Step 3: Compile and Run the App

* Run the App: In Xcode, click the Run button (▶️) in the toolbar, or press Command + R.
* Observe the Output: Xcode will compile your code and launch the app. You should see a window appear displaying "Hello, World!".
* If you encounter build errors, double-check for typos or ensure your Mac is running macOS 15 or later (SwiftUI requires macOS 10.15+).
* Stop the App: To stop the app, click the red close button on the app window or press Command + Q.


## Step 4: Package as a .app Bundle

* Xcode's compilation process automatically generates a .app bundle, which is a self-contained macOS application package.
* Build the Project: Go to Product > Build (or press Command + B). This action compiles the code without running the app.
* Locate the .app:
* In the Xcode sidebar, expand the Products folder (located under the project navigator).
* Right-click on HelloWorld.app and select Show in Finder. This will open the build directory (typically ~/Library/Developer/Xcode/DerivedData/HelloWorldApp-.../Build/Products/Debug/).
* Your HelloWorld.app file is located there. You can double-click it in Finder to run it independently of Xcode.


## Step 5: Distribution

* Create a Release Build: For an optimized build without debug information, go to Product > Scheme > Edit Scheme. Under the Run tab, set "Build Configuration" to Release, then build again.
* Signing and Notarization: If you plan to share your app outside your Mac, signing and notarization are required. Go to Product > Archive, then use the Organizer window to distribute. You will need a free Apple Developer account for notarization.
* Package as a DMG Installer: You can use Finder to create a disk image, or utilize tools like create-dmg via Homebrew for more advanced packaging.

```
brew install create-dmg
mkdir ~/build
cp -R ~/Library/Developer/Xcode/DerivedData/HelloWorld-hkuitqttajolmkaalqkyqvioufcf/Build/Products/Debug/HelloWorld.app/ ~/build/HelloWorld.app
curl -o ~/build/app-icon.icns https://raw.githubusercontent.com/borgbase/vorta/main/src/vorta/assets/icons/app-icon.icns
curl -o ~/build/background.png https://raw.githubusercontent.com/create-dmg/create-dmg/master/examples/01-main-example/installer_background.png
```

```
create-dmg \
  --volname "HelloWorld Installer" \
  --volicon ~/build/app-icon.icns \
  --background ~/build/background.png \
  --window-pos 200 120 \
  --window-size 800 400 \
  --icon-size 100 \
  --icon "HelloWorldApp.app" 200 190 \
  --app-drop-link 600 185 \
  --hide-extension "HelloWorld.app" \
  ~/build/HelloWorld.dmg \
  ~/build/HelloWorld.app
```

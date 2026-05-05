# self-updater
Self Updater
================================================

Overview
--------
This project demonstrates a self-updating application written in Python. 
It has:
 - A client application that updates itself by checking for and downloading
   the latest version (if there is one).

 - An update server (Flask) that is hosted locally. It servers the version metadata and zipped
   releases to the app.

 - A two-stage update process (updater.py and updater_launcher.py). This method was used to
    resolve some issues with Windows file locking.


How to Run the Program
----------------------

Step 1. Start the Update Server
--------------------------------
Open a terminal and navigate to:

    update_server/

Then run:

    python server.py

This starts a local Flask server at:

    http://127.0.0.1:5000

It automatically detects the newest release in the "releases" folder.


Step 2. Run the Application
----------------------------
Open a second terminal window, navigate to:

    selfupdater/

Then run:

    python main.py

On launch, the program will:

 1. Read its current version from version.txt.
 2. Contact the update server
 3. Compare versions
 4. Download and verify the new release (if there is one).
 5. Launch a separate updater process
 6. Restart the application

You will see console output indicating whether an update is available or not.

Adding New Versions
-------------------
To test the update process:

 1. Modify the app code in version_test_examples/app/
 2. Update version.txt with the new version number
 3. Create a ZIP file containing only the *contents* of the app folder (app.py, updater.py, version.txt).
    For example, from within version_test_examples/app/, the following command can be used in PowerShell
    to create the ZIP file in version_test_examples/:

        Compress-Archive -Path .\* -DestinationPath ..\app-X.Y.Z.zip

 4. Move the new ZIP into:

        update_server/releases/

 5. Restart the application (python main.py)
 6. The app will detect and install the new version automatically.


NOTE
---------------
ALWAYS run the app via `python main.py`, not by running app.py directly. Running from inside the "app" directory can cause Windows to lock the folder, resulting in the app potentially crashing when updating.

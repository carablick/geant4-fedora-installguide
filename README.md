# geant4-fedora-installguide
Geant4 installation guide from source for fedora42 made as painless as possible

Installation guide: Geant4-v11.3.2

NOTE: If a new version of geant4 is released, replace the version nummbers in steps 3-5 with the desired version.
PREREQS: Fedora 42 or later (Tested only on Fedora 42 KDE Plasma, but it should work with any Fedora 42 or later).

NOTE: You can paste these commands into the terminal with ctrl+shift+v.

NOTE: All steps can be run through the same terminal from the default directory. This is written to install to
/home/YOURUSERNAMEHERE/Documents/geant4. If you wish to install to a different directory. Replace the path in all
commands with your desired one.

WARNING: DO NOT INSTALL TO A PROTECTED DIRECTORY, such /opt, as this will prevent cmake from being able to compile
            any source code for simulations.


INSTALLATION - Copy/paste the commands into your terminal then hit enter.

1.

Open a new terminal and run
            sudo dnf upgrade
then
            sudo dnf update

NOTE: If it asks for confirmation, type y and hit enter

2.
            sudo dnf install -y gcc gcc-c++ make cmake git gdb patch \
                   redhat-rpm-config rpm-build ninja-build wget \
                   qt5-qtbase-devel qt5-qttools-devel qt5-qtx11extras-devel \
                   mesa-libGL-devel mesa-libGLU-devel freeglut-devel \
                   libX11-devel libXrandr-devel libXxf86vm-devel \
                   libXinerama-devel libXi-devel libXcursor-devel libXrender-devel \
                   expat-devel zlib-devel xerces-c-devel \
                   libXmu-devel libXt-devel


3.
            mkdir /home/YOURUSERNAMEHERE/Documents/geant4 && cd /home/YOURUSERNAMEHERE/Documents/geant4

4.
            wget https://gitlab.cern.ch/geant4/geant4/-/archive/v11.3.2/geant4-v11.3.2.tar.gz

5.
            tar xzf geant4-v11.3.2.tar.gz

6.
            mkdir geant4-build && cd geant4-build

8.

            cmake \
              -DCMAKE_INSTALL_PREFIX=~/Documents/geant4/geant4-install \
              -DGEANT4_BUILD_MULTITHREADED=ON \
              -DGEANT4_USE_QT=ON \
              -DGEANT4_USE_OPENGL_X11=ON \
              -DGEANT4_USE_GDML=ON \
              -DGEANT4_INSTALL_DATA=ON \
              -DGEANT4_BUILD_TLS_MODEL=auto \
              ../geant4-v11.3.2

NOTE: If any errors occur, rerun step 7. If the errors are related to a missing package. Rerun steps 1 and 2, then rerun step 7.

8.

WARNING: If you are running any other programs, use cmake --build . -- -j$(nproc/2) instead. nproc uses all processors on your computer, and it may error if threads are blocked. nrpoc/2 will take significantly longer to compile, but it is safer to use if you are multitasking.

NOTE: Alternatively, you can run just nproc to get the number of processors, then replace nproc in the below command with the number you wish to use.

NOTE: This step can take from 30 minutes to several hours depending on how fast your cpu is. Just let it run. Take a walk or get some coffee. If any errors occured it will list them at the end.


            cmake --build . -- -j$(nproc)

NOTE: If any errors occur, rerun steps 7-8. It will only rerun what it missed and shouldn't take nearly as long.

9.
            cmake --install .

NOTE: If any errors occur, rerun steps 6-8. This may take multiple attempts, as the

10.
            source /home/YOURUSERNAMEHERE/Documents/geant4/geant4-install/bin/geant4.sh

VERIFY INSTALLATION:
1.

WARNING: If you left the terminal, you must run "source /home/YOURUSERNAMEHERE/Documents/geant4/geant4-install/bin/geant4.sh" (or the alternative directory you installed to) again first. You will need to do this every time you open a new terminal following installation to initialize the program.

            geant4-config --version

If you followed these steps correctly, this should print the version number, currently "11.3.2".

BUILD AND OPEN A SIMULATION:
WARNING: If you left the terminal, you must run "source /home/YOURUSERNAMEHERE/Documents/geant4/geant4-install/bin/geant4.sh" (or the alternative directory you installed to) again first. You will need to do this every time you open a new terminal following installation to initialize the program.

1.
            cp -r /home/YOURUSERNAMEHERE/Documents/geant4/geant4-install/share/Geant4/examples/basic/B1 /home/YOURUSERNAMEHERE/Documents/geant4/geant4_B1

NOTE: This copies the path for the example provided by CERN to your documents folder. This also has the benefit of preserving the original source files in case you break something while learning your way around it.

2.
            cd /home/YOURUSERNAMEHERE/Documents/geant4/geant4_B1

3,
            mkdir build && cd build

4.
            cmake ..

NOTE: If you get an error that it could not find Geant4Config.cmake, you probably forgot to init the source (see the WARNING at the beginning of this section)
source /home/YOURUSERNAMEHERE/Documents/geant4/geant4-install/bin/geant4.sh

5.
            make -j$(nproc)

6.
WARNING: You must include the export QT_QPA_PLATFORM=xcb portion of the command every time you run a simulation. (This can be added to bashrc to automate, but I am trying to keep this guide as simple as possible.)

            export QT_QPA_PLATFORM=xcb
            ./exampleB1

NOTE: If you did everything right, this will open a new window which runs the simulation. If it errors, you may have missed a step.

RUNNING YOUR SIMULATION:
1.
In the geant4 window for your simulation, at the bottom of the screen is a text field that starts with "Session:"
In that field, enter the following command, then hit enter:
            /run/beamOn 10
NOTE: This will run the simulation 10 times. If you want to run it a different number of times, replace 10 with your desired runs.

If you did everything correctly, the simulation should display in the viewing window.

2.
To export the logs, click the little blue square on the far right side below the simulation screen (in the same row as Threads:).
